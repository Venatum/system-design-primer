# Conception du classement des ventes par catégorie d'Amazon

*Remarque : Ce document renvoie directement aux domaines pertinents des [sujets de conception de systèmes](https://github.com/donnemartin/system-design-primer#index-of-system-design-topics) pour éviter les duplications. Référez-vous au contenu lié pour les points de discussion généraux, les compromis et les alternatives.*

## Étape 1 : Définir les cas d'utilisation et les contraintes

> Recueillir les exigences et délimiter le problème.
> Poser des questions pour clarifier les cas d'utilisation et les contraintes.
> Discuter des hypothèses.

Sans un intervieweur pour répondre aux questions de clarification, nous allons définir quelques cas d'utilisation et contraintes.

### Cas d'utilisation

#### Nous allons limiter le problème pour traiter uniquement les cas d'utilisation suivants

* **Service** calcule les produits les plus populaires de la semaine passée par catégorie
* **Utilisateur** consulte les produits les plus populaires de la semaine passée par catégorie
* **Service** à haute disponibilité

#### Hors du champ d'application

* Le site e-commerce général
    * Concevoir uniquement les composants pour calculer le classement des ventes

### Contraintes et hypothèses

#### Hypothèses

* Le trafic n'est pas uniformément distribué
* Les articles peuvent être dans plusieurs catégories
* Les articles ne peuvent pas changer de catégories
* Il n'y a pas de sous-catégories, c'est-à-dire `foo/bar/baz`
* Les résultats doivent être mis à jour toutes les heures
    * Les produits plus populaires pourraient nécessiter des mises à jour plus fréquentes
* 10 millions de produits
* 1000 catégories
* 1 milliard de transactions par mois
* 100 milliards de requêtes de lecture par mois
* Ratio lecture/écriture de 100:1

#### Calcul d'utilisation

**Clarifiez avec votre intervieweur si vous devez effectuer des calculs d'utilisation approximatifs.**

* Taille par transaction :
    * `created_at` - 5 octets
    * `product_id` - 8 octets
    * `category_id` - 4 octets
    * `seller_id` - 8 octets
    * `buyer_id` - 8 octets
    * `quantity` - 4 octets
    * `total_price` - 5 octets
    * Total: ~40 octets
* 40 Go de nouveau contenu de transaction par mois
    * 40 octets par transaction * 1 milliard de transactions par mois
    * 1,44 To de nouveau contenu de transaction en 3 ans
    * Supposons que la plupart sont de nouvelles transactions plutôt que des mises à jour de transactions existantes
* 400 transactions par seconde en moyenne
* 40 000 requêtes de lecture par seconde en moyenne

Guide de conversion pratique :

* 2,5 millions de secondes par mois
* 1 requête par seconde = 2,5 millions de requêtes par mois
* 40 requêtes par seconde = 100 millions de requêtes par mois
* 400 requêtes par seconde = 1 milliard de requêtes par mois

## Étape 2 : Créer une conception de haut niveau

> Esquisser une conception de haut niveau avec tous les composants importants.

![Imgur](http://i.imgur.com/vwMa1Qu.png)

## Étape 3 : Concevoir les composants principaux

> Approfondir les détails de chaque composant principal.

### Cas d'utilisation : Le service calcule les produits les plus populaires de la semaine passée par catégorie

Nous pourrions stocker les fichiers journaux bruts du serveur **API de Ventes** sur un **Stockage d'Objets** géré comme Amazon S3, plutôt que de gérer notre propre système de fichiers distribué.

**Clarifiez avec votre intervieweur la quantité de code que vous êtes censé écrire**.

Nous supposerons qu'il s'agit d'un exemple d'entrée de journal, délimité par des tabulations :

```
timestamp   product_id  category_id    qty     total_price   seller_id    buyer_id
t1          product1    category1      2       20.00         1            1
t2          product1    category2      2       20.00         2            2
t2          product1    category2      1       10.00         2            3
t3          product2    category1      3        7.00         3            4
t4          product3    category2      7        2.00         4            5
t5          product4    category1      1        5.00         5            6
...
```

Le **Service de Classement des Ventes** pourrait utiliser **MapReduce**, en utilisant les fichiers journaux du serveur **API de Ventes** comme entrée et en écrivant les résultats dans une table agrégée `sales_rank` dans une **Base de données SQL**. Nous devrions discuter des [cas d'utilisation et des compromis entre le choix de SQL ou NoSQL](https://github.com/donnemartin/system-design-primer#sql-or-nosql).

Nous utiliserons un **MapReduce** en plusieurs étapes :

* **Étape 1** - Transformer les données en `(catégorie, product_id), sum(quantité)`
* **Étape 2** - Effectuer un tri distribué

```python
class SalesRanker(MRJob):

    def within_past_week(self, timestamp):
        """Renvoie True si l'horodatage est dans la semaine passée, False sinon."""
        ...

    def mapper(self, _ line):
        """Analyse chaque ligne de journal, extrait et transforme les lignes pertinentes.

        Émet des paires clé-valeur de la forme :

        (category1, product1), 2
        (category2, product1), 2
        (category2, product1), 1
        (category1, product2), 3
        (category2, product3), 7
        (category1, product4), 1
        """
        timestamp, product_id, category_id, quantity, total_price, seller_id, \
            buyer_id = line.split('\t')
        if self.within_past_week(timestamp):
            yield (category_id, product_id), quantity

    def reducer(self, key, value):
        """Somme les valeurs pour chaque clé.

        (category1, product1), 2
        (category2, product1), 3
        (category1, product2), 3
        (category2, product3), 7
        (category1, product4), 1
        """
        yield key, sum(values)

    def mapper_sort(self, key, value):
        """Construit la clé pour assurer un tri correct.

        Transforme la clé et la valeur sous la forme :

        (category1, 2), product1
        (category2, 3), product1
        (category1, 3), product2
        (category2, 7), product3
        (category1, 1), product4

        L'étape de mélange/tri de MapReduce effectuera ensuite un
        tri distribué sur les clés, résultant en :

        (category1, 1), product4
        (category1, 2), product1
        (category1, 3), product2
        (category2, 3), product1
        (category2, 7), product3
        """
        category_id, product_id = key
        quantity = value
        yield (category_id, quantity), product_id

    def reducer_identity(self, key, value):
        yield key, value

    def steps(self):
        """Exécute les étapes de map et reduce."""
        return [
            self.mr(mapper=self.mapper,
                    reducer=self.reducer),
            self.mr(mapper=self.mapper_sort,
                    reducer=self.reducer_identity),
        ]
```

Le résultat serait la liste triée suivante, que nous pourrions insérer dans la table `sales_rank` :

```
(category1, 1), product4
(category1, 2), product1
(category1, 3), product2
(category2, 3), product1
(category2, 7), product3
```

La table `sales_rank` pourrait avoir la structure suivante :

```
id int NOT NULL AUTO_INCREMENT
category_id int NOT NULL
total_sold int NOT NULL
product_id int NOT NULL
PRIMARY KEY(id)
FOREIGN KEY(category_id) REFERENCES Categories(id)
FOREIGN KEY(product_id) REFERENCES Products(id)
```

Nous créerons un [index](https://github.com/donnemartin/system-design-primer#use-good-indices) sur `id`, `category_id` et `product_id` pour accélérer les recherches (temps logarithmique au lieu de parcourir toute la table) et pour garder les données en mémoire. La lecture séquentielle de 1 Mo depuis la mémoire prend environ 250 microsecondes, tandis que la lecture depuis un SSD prend 4 fois plus de temps et depuis un disque dur 80 fois plus longtemps.<sup><a href=https://github.com/donnemartin/system-design-primer#latency-numbers-every-programmer-should-know>1</a></sup>

### Cas d'utilisation : L'utilisateur consulte les produits les plus populaires de la semaine passée par catégorie

* Le **Client** envoie une requête au **Serveur Web**, fonctionnant comme un [proxy inverse](https://github.com/donnemartin/system-design-primer#reverse-proxy-web-server)
* Le **Serveur Web** transmet la requête au serveur **API de Lecture**
* Le serveur **API de Lecture** lit depuis la table `sales_rank` de la **Base de données SQL**

Nous utiliserons une [**API REST**](https://github.com/donnemartin/system-design-primer#representational-state-transfer-rest) publique :

```
$ curl https://amazon.com/api/v1/popular?category_id=1234
```

Réponse :

```
{
    "id": "100",
    "category_id": "1234",
    "total_sold": "100000",
    "product_id": "50",
},
{
    "id": "53",
    "category_id": "1234",
    "total_sold": "90000",
    "product_id": "200",
},
{
    "id": "75",
    "category_id": "1234",
    "total_sold": "80000",
    "product_id": "3",
},
```

Pour les communications internes, nous pourrions utiliser [Remote Procedure Calls](https://github.com/donnemartin/system-design-primer#remote-procedure-call-rpc).

## Étape 4 : Mettre à l'échelle la conception

> Identifier et traiter les goulots d'étranglement, compte tenu des contraintes.

![Imgur](http://i.imgur.com/MzExP06.png)

**Important : Ne passez pas directement de la conception initiale à la conception finale !**

Indiquez que vous 1) **Benchmarkeriez/Testeriez en charge**, 2) **Profileriez** pour identifier les goulots d'étranglement 3) traiteriez les goulots d'étranglement tout en évaluant les alternatives et les compromis, et 4) répéteriez. Voir [Concevoir un système qui s'adapte à des millions d'utilisateurs sur AWS](../scaling_aws/README.md) comme exemple sur la façon de mettre à l'échelle de manière itérative la conception initiale.

Il est important de discuter des goulots d'étranglement que vous pourriez rencontrer avec la conception initiale et comment vous pourriez les traiter. Par exemple, quels problèmes sont résolus en ajoutant un **Équilibreur de charge** avec plusieurs **Serveurs Web** ? **CDN** ? **Répliques Maître-Esclave** ? Quelles sont les alternatives et les **Compromis** pour chacun ?

Nous introduirons certains composants pour compléter la conception et traiter les problèmes de scalabilité. Les équilibreurs de charge internes ne sont pas montrés pour réduire l'encombrement.

*Pour éviter de répéter les discussions*, référez-vous aux [sujets de conception de systèmes](https://github.com/donnemartin/system-design-primer#index-of-system-design-topics) suivants pour les principaux points de discussion, les compromis et les alternatives :

* [DNS](https://github.com/donnemartin/system-design-primer#domain-name-system)
* [CDN](https://github.com/donnemartin/system-design-primer#content-delivery-network)
* [Équilibreur de charge](https://github.com/donnemartin/system-design-primer#load-balancer)
* [Mise à l'échelle horizontale](https://github.com/donnemartin/system-design-primer#horizontal-scaling)
* [Serveur web (proxy inverse)](https://github.com/donnemartin/system-design-primer#reverse-proxy-web-server)
* [Serveur API (couche application)](https://github.com/donnemartin/system-design-primer#application-layer)
* [Cache](https://github.com/donnemartin/system-design-primer#cache)
* [Système de gestion de base de données relationnelle (SGBDR)](https://github.com/donnemartin/system-design-primer#relational-database-management-system-rdbms)
* [Basculement maître-esclave SQL](https://github.com/donnemartin/system-design-primer#fail-over)
* [Réplication maître-esclave](https://github.com/donnemartin/system-design-primer#master-slave-replication)
* [Modèles de cohérence](https://github.com/donnemartin/system-design-primer#consistency-patterns)
* [Modèles de disponibilité](https://github.com/donnemartin/system-design-primer#availability-patterns)

La **Base de données d'Analyse** pourrait utiliser une solution d'entrepôt de données comme Amazon Redshift ou Google BigQuery.

Nous pourrions ne vouloir stocker qu'une période de temps limitée de données dans la base de données, tout en stockant le reste dans un entrepôt de données ou dans un **Stockage d'Objets**. Un **Stockage d'Objets** comme Amazon S3 peut facilement gérer la contrainte de 40 Go de nouveau contenu par mois.

Pour gérer les 40 000 requêtes de lecture *moyennes* par seconde (plus élevées en période de pointe), le trafic pour le contenu populaire (et leur classement de ventes) devrait être géré par le **Cache Mémoire** plutôt que par la base de données. Le **Cache Mémoire** est également utile pour gérer le trafic inégalement distribué et les pics de trafic. Avec le grand volume de lectures, les **Répliques de Lecture SQL** pourraient ne pas être en mesure de gérer les échecs de cache. Nous aurons probablement besoin d'employer des techniques de mise à l'échelle SQL supplémentaires.

400 écritures *moyennes* par seconde (plus élevées en période de pointe) pourraient être difficiles pour un seul **Maître-Esclave SQL d'Écriture**, indiquant également un besoin de techniques de mise à l'échelle supplémentaires.

Les modèles de mise à l'échelle SQL comprennent :

* [Fédération](https://github.com/donnemartin/system-design-primer#federation)
* [Partitionnement](https://github.com/donnemartin/system-design-primer#sharding)
* [Dénormalisation](https://github.com/donnemartin/system-design-primer#denormalization)
* [Optimisation SQL](https://github.com/donnemartin/system-design-primer#sql-tuning)

Nous devrions également envisager de déplacer certaines données vers une **Base de données NoSQL**.

## Points de discussion supplémentaires

> Sujets supplémentaires à approfondir, selon la portée du problème et le temps restant.

#### NoSQL

* [Stockage clé-valeur](https://github.com/donnemartin/system-design-primer#key-value-store)
* [Stockage de documents](https://github.com/donnemartin/system-design-primer#document-store)
* [Stockage en colonnes larges](https://github.com/donnemartin/system-design-primer#wide-column-store)
* [Base de données graphe](https://github.com/donnemartin/system-design-primer#graph-database)
* [SQL vs NoSQL](https://github.com/donnemartin/system-design-primer#sql-or-nosql)

### Mise en cache

* Où mettre en cache
    * [Mise en cache côté client](https://github.com/donnemartin/system-design-primer#client-caching)
    * [Mise en cache CDN](https://github.com/donnemartin/system-design-primer#cdn-caching)
    * [Mise en cache du serveur web](https://github.com/donnemartin/system-design-primer#web-server-caching)
    * [Mise en cache de la base de données](https://github.com/donnemartin/system-design-primer#database-caching)
    * [Mise en cache de l'application](https://github.com/donnemartin/system-design-primer#application-caching)
* Quoi mettre en cache
    * [Mise en cache au niveau de la requête de base de données](https://github.com/donnemartin/system-design-primer#caching-at-the-database-query-level)
    * [Mise en cache au niveau de l'objet](https://github.com/donnemartin/system-design-primer#caching-at-the-object-level)
* Quand mettre à jour le cache
    * [Cache-aside](https://github.com/donnemartin/system-design-primer#cache-aside)
    * [Write-through](https://github.com/donnemartin/system-design-primer#write-through)
    * [Write-behind (write-back)](https://github.com/donnemartin/system-design-primer#write-behind-write-back)
    * [Refresh ahead](https://github.com/donnemartin/system-design-primer#refresh-ahead)

### Asynchronisme et microservices

* [Files d'attente de messages](https://github.com/donnemartin/system-design-primer#message-queues)
* [Files d'attente de tâches](https://github.com/donnemartin/system-design-primer#task-queues)
* [Contre-pression](https://github.com/donnemartin/system-design-primer#back-pressure)
* [Microservices](https://github.com/donnemartin/system-design-primer#microservices)

### Communications

* Discuter des compromis :
    * Communication externe avec les clients - [API HTTP suivant REST](https://github.com/donnemartin/system-design-primer#representational-state-transfer-rest)
    * Communications internes - [RPC](https://github.com/donnemartin/system-design-primer#remote-procedure-call-rpc)
* [Découverte de service](https://github.com/donnemartin/system-design-primer#service-discovery)

### Sécurité

Référez-vous à la [section sécurité](https://github.com/donnemartin/system-design-primer#security).

### Chiffres de latence

Voir [Chiffres de latence que tout programmeur devrait connaître](https://github.com/donnemartin/system-design-primer#latency-numbers-every-programmer-should-know).

### En continu

* Continuer à benchmarker et à surveiller votre système pour traiter les goulots d'étranglement au fur et à mesure qu'ils apparaissent
* La mise à l'échelle est un processus itératif