# Conception de Mint.com

*Remarque : Ce document renvoie directement aux domaines pertinents des [sujets de conception de systèmes](https://github.com/donnemartin/system-design-primer#index-of-system-design-topics) pour éviter les duplications. Référez-vous au contenu lié pour les points de discussion généraux, les compromis et les alternatives.*

## Étape 1 : Définir les cas d'utilisation et les contraintes

> Recueillir les exigences et délimiter le problème.
> Poser des questions pour clarifier les cas d'utilisation et les contraintes.
> Discuter des hypothèses.

Sans un intervieweur pour répondre aux questions de clarification, nous allons définir quelques cas d'utilisation et contraintes.

### Cas d'utilisation

#### Nous allons limiter le problème pour traiter uniquement les cas d'utilisation suivants

* **Utilisateur** se connecte à un compte financier
* **Service** extrait les transactions du compte
    * Mises à jour quotidiennes
    * Catégorise les transactions
        * Permet à l'utilisateur de remplacer manuellement la catégorie
        * Pas de recatégorisation automatique
    * Analyse les dépenses mensuelles, par catégorie
* **Service** recommande un budget
    * Permet aux utilisateurs de définir manuellement un budget
    * Envoie des notifications lorsqu'on approche ou dépasse le budget
* **Service** a une haute disponibilité

#### Hors du champ d'application

* **Service** effectue des journalisations et analyses supplémentaires

### Contraintes et hypothèses

#### Hypothèses

* Le trafic n'est pas uniformément distribué
* La mise à jour quotidienne automatique des comptes s'applique uniquement aux utilisateurs actifs au cours des 30 derniers jours
* L'ajout ou la suppression de comptes financiers est relativement rare
* Les notifications budgétaires n'ont pas besoin d'être instantanées
* 10 millions d'utilisateurs
    * 10 catégories de budget par utilisateur = 100 millions d'éléments de budget
    * Exemples de catégories :
        * Logement = 1 000 $
        * Alimentation = 200 $
        * Essence = 100 $
    * Les vendeurs sont utilisés pour déterminer la catégorie de transaction
        * 50 000 vendeurs
* 30 millions de comptes financiers
* 5 milliards de transactions par mois
* 500 millions de requêtes de lecture par mois
* Ratio écriture/lecture de 10:1
    * Beaucoup d'écritures, les utilisateurs effectuent des transactions quotidiennes, mais peu visitent le site quotidiennement

#### Calcul d'utilisation

**Clarifiez avec votre intervieweur si vous devez effectuer des calculs d'utilisation approximatifs.**

* Taille par transaction :
    * `user_id` - 8 octets
    * `created_at` - 5 octets
    * `seller` - 32 octets
    * `amount` - 5 octets
    * Total : ~50 octets
* 250 Go de nouveau contenu de transaction par mois
    * 50 octets par transaction * 5 milliards de transactions par mois
    * 9 To de nouveau contenu de transaction en 3 ans
    * Supposons que la plupart sont de nouvelles transactions plutôt que des mises à jour de transactions existantes
* 2 000 transactions par seconde en moyenne
* 200 requêtes de lecture par seconde en moyenne

Guide de conversion pratique :

* 2,5 millions de secondes par mois
* 1 requête par seconde = 2,5 millions de requêtes par mois
* 40 requêtes par seconde = 100 millions de requêtes par mois
* 400 requêtes par seconde = 1 milliard de requêtes par mois

## Étape 2 : Créer une conception de haut niveau

> Esquisser une conception de haut niveau avec tous les composants importants.

![Imgur](http://i.imgur.com/E8klrBh.png)

## Étape 3 : Concevoir les composants principaux

> Approfondir les détails de chaque composant principal.

### Cas d'utilisation : L'utilisateur se connecte à un compte financier

Nous pourrions stocker les informations sur les 10 millions d'utilisateurs dans une [base de données relationnelle](https://github.com/donnemartin/system-design-primer#relational-database-management-system-rdbms). Nous devrions discuter des [cas d'utilisation et des compromis entre le choix de SQL ou NoSQL](https://github.com/donnemartin/system-design-primer#sql-or-nosql).

* Le **Client** envoie une requête au **Serveur Web**, fonctionnant comme un [proxy inverse](https://github.com/donnemartin/system-design-primer#reverse-proxy-web-server)
* Le **Serveur Web** transmet la requête au serveur **API des Comptes**
* Le serveur **API des Comptes** met à jour la table `accounts` de la **Base de données SQL** avec les nouvelles informations de compte saisies

**Clarifiez avec votre intervieweur la quantité de code que vous êtes censé écrire**.

La table `accounts` pourrait avoir la structure suivante :

```
id                      int             NOT NULL AUTO_INCREMENT
created_at              datetime        NOT NULL
last_update             datetime        NOT NULL
account_url             varchar(255)    NOT NULL
account_login           varchar(32)     NOT NULL
account_password_hash   char(64)        NOT NULL
user_id                 int             NOT NULL
PRIMARY KEY(id)
FOREIGN KEY(user_id) REFERENCES users(id)
```

Nous créerons un [index](https://github.com/donnemartin/system-design-primer#use-good-indices) sur `id`, `user_id` et `created_at` pour accélérer les recherches (temps logarithmique au lieu de parcourir toute la table) et pour garder les données en mémoire. La lecture séquentielle de 1 Mo depuis la mémoire prend environ 250 microsecondes, tandis que la lecture depuis un SSD prend 4 fois plus de temps et depuis un disque dur 80 fois plus longtemps.<sup><a href=https://github.com/donnemartin/system-design-primer#latency-numbers-every-programmer-should-know>1</a></sup>

Nous utiliserons une [**API REST**](https://github.com/donnemartin/system-design-primer#representational-state-transfer-rest) publique :

```
$ curl -X POST --data '{ "user_id": "foo", "account_url": "bar", \
    "account_login": "baz", "account_password": "qux" }' \
    https://mint.com/api/v1/account
```

Pour les communications internes, nous pourrions utiliser [Remote Procedure Calls](https://github.com/donnemartin/system-design-primer#remote-procedure-call-rpc).

Ensuite, le service extrait les transactions du compte.

### Cas d'utilisation : Le service extrait les transactions du compte

Nous voudrons extraire des informations d'un compte dans ces cas :

* L'utilisateur lie le compte pour la première fois
* L'utilisateur actualise manuellement le compte
* Automatiquement chaque jour pour les utilisateurs qui ont été actifs au cours des 30 derniers jours

Flux de données :

* Le **Client** envoie une requête au **Serveur Web**
* Le **Serveur Web** transmet la requête au serveur **API des Comptes**
* Le serveur **API des Comptes** place une tâche dans une **File d'attente** comme [Amazon SQS](https://aws.amazon.com/sqs/) ou [RabbitMQ](https://www.rabbitmq.com/)
    * L'extraction des transactions pourrait prendre un certain temps, nous voudrions probablement le faire [de manière asynchrone avec une file d'attente](https://github.com/donnemartin/system-design-primer#asynchronism), bien que cela introduise une complexité supplémentaire
* Le **Service d'Extraction de Transactions** fait ce qui suit :
    * Récupère depuis la **File d'attente** et extrait les transactions pour le compte donné depuis l'institution financière, stockant les résultats sous forme de fichiers journaux bruts dans le **Stockage d'Objets**
    * Utilise le **Service de Catégorisation** pour catégoriser chaque transaction
    * Utilise le **Service de Budget** pour calculer les dépenses mensuelles agrégées par catégorie
        * Le **Service de Budget** utilise le **Service de Notification** pour informer les utilisateurs s'ils approchent ou dépassent leur budget
    * Met à jour la table `transactions` de la **Base de données SQL** avec les transactions catégorisées
    * Met à jour la table `monthly_spending` de la **Base de données SQL** avec les dépenses mensuelles agrégées par catégorie
    * Notifie l'utilisateur que les transactions sont terminées via le **Service de Notification** :
        * Utilise une **File d'attente** (non illustrée) pour envoyer des notifications de manière asynchrone

La table `transactions` pourrait avoir la structure suivante :

```
id          int         NOT NULL AUTO_INCREMENT
created_at  datetime    NOT NULL
seller      varchar(32) NOT NULL
amount      decimal     NOT NULL
user_id     int         NOT NULL
PRIMARY KEY(id)
FOREIGN KEY(user_id) REFERENCES users(id)
```

Nous créerons un [index](https://github.com/donnemartin/system-design-primer#use-good-indices) sur `id`, `user_id` et `created_at`.

La table `monthly_spending` pourrait avoir la structure suivante :

```
id          int         NOT NULL AUTO_INCREMENT
month_year  date        NOT NULL
category    varchar(32)
amount      decimal     NOT NULL
user_id     int         NOT NULL
PRIMARY KEY(id)
FOREIGN KEY(user_id) REFERENCES users(id)
```

Nous créerons un [index](https://github.com/donnemartin/system-design-primer#use-good-indices) sur `id` et `user_id`.

#### Service de catégorisation

Pour le **Service de Catégorisation**, nous pouvons initialiser un dictionnaire vendeur-à-catégorie avec les vendeurs les plus populaires. Si nous estimons 50 000 vendeurs et estimons que chaque entrée prend moins de 255 octets, le dictionnaire ne prendrait qu'environ 12 Mo de mémoire.

**Clarifiez avec votre intervieweur la quantité de code que vous êtes censé écrire**.

```python
class DefaultCategories(Enum):

    HOUSING = 0
    FOOD = 1
    GAS = 2
    SHOPPING = 3
    ...

seller_category_map = {}
seller_category_map['Exxon'] = DefaultCategories.GAS
seller_category_map['Target'] = DefaultCategories.SHOPPING
...
```

Pour les vendeurs non initialement inclus dans la carte, nous pourrions utiliser un effort de crowdsourcing en évaluant les remplacements manuels de catégorie fournis par nos utilisateurs. Nous pourrions utiliser un tas (heap) pour rechercher rapidement le remplacement manuel le plus courant par vendeur en temps O(1).

```python
class Categorizer(object):

    def __init__(self, seller_category_map, seller_category_crowd_overrides_map):
        self.seller_category_map = seller_category_map
        self.seller_category_crowd_overrides_map = \
            seller_category_crowd_overrides_map

    def categorize(self, transaction):
        if transaction.seller in self.seller_category_map:
            return self.seller_category_map[transaction.seller]
        elif transaction.seller in self.seller_category_crowd_overrides_map:
            self.seller_category_map[transaction.seller] = \
                self.seller_category_crowd_overrides_map[transaction.seller].peek_min()
            return self.seller_category_map[transaction.seller]
        return None
```

Implémentation de Transaction :

```python
class Transaction(object):

    def __init__(self, created_at, seller, amount):
        self.created_at = created_at
        self.seller = seller
        self.amount = amount
```

### Cas d'utilisation : Le service recommande un budget

Pour commencer, nous pourrions utiliser un modèle de budget générique qui alloue des montants par catégorie en fonction des tranches de revenus. Avec cette approche, nous n'aurions pas à stocker les 100 millions d'éléments de budget identifiés dans les contraintes, mais seulement ceux que l'utilisateur remplace. Si un utilisateur remplace une catégorie de budget, nous pourrions stocker le remplacement dans la table `budget_overrides`.

```python
class Budget(object):

    def __init__(self, income):
        self.income = income
        self.categories_to_budget_map = self.create_budget_template()

    def create_budget_template(self):
        return {
            DefaultCategories.HOUSING: self.income * .4,
            DefaultCategories.FOOD: self.income * .2,
            DefaultCategories.GAS: self.income * .1,
            DefaultCategories.SHOPPING: self.income * .2,
            ...
        }

    def override_category_budget(self, category, amount):
        self.categories_to_budget_map[category] = amount
```

Pour le **Service de Budget**, nous pouvons potentiellement exécuter des requêtes SQL sur la table `transactions` pour générer la table agrégée `monthly_spending`. La table `monthly_spending` aurait probablement beaucoup moins de lignes que les 5 milliards de transactions totales, puisque les utilisateurs ont généralement de nombreuses transactions par mois.

Alternativement, nous pouvons exécuter des tâches **MapReduce** sur les fichiers de transactions bruts pour :

* Catégoriser chaque transaction
* Générer des dépenses mensuelles agrégées par catégorie

L'exécution d'analyses sur les fichiers de transactions pourrait réduire considérablement la charge sur la base de données.

Nous pourrions appeler le **Service de Budget** pour relancer l'analyse si l'utilisateur met à jour une catégorie.

**Clarifiez avec votre intervieweur la quantité de code que vous êtes censé écrire**.

Format d'exemple de fichier journal, délimité par des tabulations :

```
user_id   timestamp   seller  amount
```

Implémentation **MapReduce** :

```python
class SpendingByCategory(MRJob):

    def __init__(self, categorizer):
        self.categorizer = categorizer
        self.current_year_month = calc_current_year_month()
        ...

    def calc_current_year_month(self):
        """Renvoie l'année et le mois actuels."""
        ...

    def extract_year_month(self, timestamp):
        """Renvoie les parties année et mois du timestamp."""
        ...

    def handle_budget_notifications(self, key, total):
        """Appelle l'API de notification si on approche ou dépasse le budget."""
        ...

    def mapper(self, _, line):
        """Analyse chaque ligne de journal, extrait et transforme les lignes pertinentes.

        L'argument line sera de la forme :

        user_id   timestamp   seller  amount

        En utilisant le categorizer pour convertir le vendeur en catégorie,
        émet des paires clé-valeur de la forme :

        (user_id, 2016-01, shopping), 25
        (user_id, 2016-01, shopping), 100
        (user_id, 2016-01, gas), 50
        """
        user_id, timestamp, seller, amount = line.split('\t')
        category = self.categorizer.categorize(seller)
        period = self.extract_year_month(timestamp)
        if period == self.current_year_month:
            yield (user_id, period, category), amount

    def reducer(self, key, value):
        """Somme les valeurs pour chaque clé.

        (user_id, 2016-01, shopping), 125
        (user_id, 2016-01, gas), 50
        """
        total = sum(values)
        yield key, sum(values)
```

## Étape 4 : Mettre à l'échelle la conception

> Identifier et traiter les goulots d'étranglement, compte tenu des contraintes.

![Imgur](http://i.imgur.com/V5q57vU.png)

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
* [Asynchronisme](https://github.com/donnemartin/system-design-primer#asynchronism)
* [Modèles de cohérence](https://github.com/donnemartin/system-design-primer#consistency-patterns)
* [Modèles de disponibilité](https://github.com/donnemartin/system-design-primer#availability-patterns)

Nous ajouterons un cas d'utilisation supplémentaire : **L'utilisateur** accède aux résumés et aux transactions.

Les sessions utilisateur, les statistiques agrégées par catégorie et les transactions récentes pourraient être placées dans un **Cache Mémoire** comme Redis ou Memcached.

* Le **Client** envoie une requête de lecture au **Serveur Web**
* Le **Serveur Web** transmet la requête au serveur **API de Lecture**
    * Le contenu statique peut être servi depuis le **Stockage d'Objets** comme S3, qui est mis en cache sur le **CDN**
* Le serveur **API de Lecture** fait ce qui suit :
    * Vérifie le **Cache Mémoire** pour le contenu
        * Si l'URL est dans le **Cache Mémoire**, renvoie le contenu mis en cache
        * Sinon
            * Si l'URL est dans la **Base de données SQL**, récupère le contenu
                * Met à jour le **Cache Mémoire** avec le contenu

Référez-vous à [Quand mettre à jour le cache](https://github.com/donnemartin/system-design-primer#when-to-update-the-cache) pour les compromis et les alternatives. L'approche ci-dessus décrit [cache-aside](https://github.com/donnemartin/system-design-primer#cache-aside).

Au lieu de conserver la table agrégée `monthly_spending` dans la **Base de données SQL**, nous pourrions créer une **Base de données d'Analyse** séparée en utilisant une solution d'entrepôt de données comme Amazon Redshift ou Google BigQuery.

Nous pourrions ne vouloir stocker qu'un mois de données `transactions` dans la base de données, tout en stockant le reste dans un entrepôt de données ou dans un **Stockage d'Objets**. Un **Stockage d'Objets** comme Amazon S3 peut facilement gérer la contrainte de 250 Go de nouveau contenu par mois.

Pour gérer les 200 requêtes de lecture *moyennes* par seconde (plus élevées en période de pointe), le trafic pour le contenu populaire devrait être géré par le **Cache Mémoire** plutôt que par la base de données. Le **Cache Mémoire** est également utile pour gérer le trafic inégalement distribué et les pics de trafic. Les **Répliques de Lecture SQL** devraient pouvoir gérer les échecs de cache, tant que les répliques ne sont pas surchargées par la réplication des écritures.

2 000 écritures de transactions *moyennes* par seconde (plus élevées en période de pointe) pourraient être difficiles pour un seul **Maître-Esclave SQL d'Écriture**. Nous pourrions avoir besoin d'employer des modèles de mise à l'échelle SQL supplémentaires :

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