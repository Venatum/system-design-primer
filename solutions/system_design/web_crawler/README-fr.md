# Conception d'un robot d'exploration web

*Remarque : Ce document renvoie directement aux domaines pertinents des [sujets de conception de systèmes](https://github.com/donnemartin/system-design-primer#index-of-system-design-topics) pour éviter les duplications. Référez-vous au contenu lié pour les points de discussion généraux, les compromis et les alternatives.*

## Étape 1 : Définir les cas d'utilisation et les contraintes

> Recueillir les exigences et délimiter le problème.
> Poser des questions pour clarifier les cas d'utilisation et les contraintes.
> Discuter des hypothèses.

Sans un intervieweur pour répondre aux questions de clarification, nous allons définir quelques cas d'utilisation et contraintes.

### Cas d'utilisation

#### Nous allons limiter le problème pour traiter uniquement les cas d'utilisation suivants

* **Service** explore une liste d'URLs :
    * Génère un index inversé des mots vers les pages contenant les termes de recherche
    * Génère des titres et des extraits pour les pages
        * Les titres et extraits sont statiques, ils ne changent pas en fonction de la requête de recherche
* **Utilisateur** saisit un terme de recherche et voit une liste de pages pertinentes avec les titres et extraits générés par le robot d'exploration
    * Esquisser uniquement les composants de haut niveau et les interactions pour ce cas d'utilisation, pas besoin d'entrer dans les détails
* **Service** à haute disponibilité

#### Hors du champ d'application

* Analytique de recherche
* Résultats de recherche personnalisés
* Classement des pages

### Contraintes et hypothèses

#### Hypothèses

* Le trafic n'est pas uniformément distribué
    * Certaines recherches sont très populaires, tandis que d'autres ne sont exécutées qu'une seule fois
* Support uniquement pour les utilisateurs anonymes
* La génération des résultats de recherche doit être rapide
* Le robot d'exploration ne doit pas se retrouver dans une boucle infinie
    * Nous nous retrouvons dans une boucle infinie si le graphe contient un cycle
* 1 milliard de liens à explorer
    * Les pages doivent être explorées régulièrement pour assurer leur fraîcheur
    * Taux de rafraîchissement moyen d'environ une fois par semaine, plus fréquent pour les sites populaires
        * 4 milliards de liens explorés chaque mois
    * Taille moyenne stockée par page web : 500 Ko
        * Par simplicité, compter les modifications comme des nouvelles pages
* 100 milliards de recherches par mois

Exercer l'utilisation de systèmes plus traditionnels — ne pas utiliser de systèmes existants tels que [solr](http://lucene.apache.org/solr/) ou [nutch](http://nutch.apache.org/).

#### Calcul d'utilisation

**Clarifiez avec votre intervieweur si vous devez effectuer des calculs d'utilisation approximatifs.**

* 2 Po de contenu de page stocké par mois
    * 500 Ko par page * 4 milliards de liens explorés par mois
    * 72 Po de contenu de page stocké en 3 ans
* 1 600 requêtes d'écriture par seconde
* 40 000 requêtes de recherche par seconde

Guide de conversion pratique :

* 2,5 millions de secondes par mois
* 1 requête par seconde = 2,5 millions de requêtes par mois
* 40 requêtes par seconde = 100 millions de requêtes par mois
* 400 requêtes par seconde = 1 milliard de requêtes par mois

## Étape 2 : Créer une conception de haut niveau

> Esquisser une conception de haut niveau avec tous les composants importants.

![Imgur](http://i.imgur.com/xjdAAUv.png)

## Étape 3 : Concevoir les composants principaux

> Approfondir les détails de chaque composant principal.

### Cas d'utilisation : Le service explore une liste d'URLs

Nous supposerons que nous avons une liste initiale de `links_to_crawl` classée initialement en fonction de la popularité globale du site. Si cette hypothèse n'est pas raisonnable, nous pouvons alimenter le robot d'exploration avec des sites populaires qui renvoient vers du contenu externe comme [Yahoo](https://www.yahoo.com/), [DMOZ](http://www.dmoz.org/), etc.

Nous utiliserons une table `crawled_links` pour stocker les liens traités et leurs signatures de page.

Nous pourrions stocker `links_to_crawl` et `crawled_links` dans une **Base de données NoSQL** clé-valeur. Pour les liens classés dans `links_to_crawl`, nous pourrions utiliser [Redis](https://redis.io/) avec des ensembles triés pour maintenir un classement des liens de page. Nous devrions discuter des [cas d'utilisation et des compromis entre le choix de SQL ou NoSQL](https://github.com/donnemartin/system-design-primer#sql-or-nosql).

* Le **Service de Robot d'Exploration** traite chaque lien de page en faisant ce qui suit dans une boucle :
    * Prend le lien de page le mieux classé à explorer
        * Vérifie `crawled_links` dans la **Base de données NoSQL** pour une entrée avec une signature de page similaire
            * Si nous avons une page similaire, réduit la priorité du lien de page
                * Cela nous empêche de nous retrouver dans un cycle
                * Continue
            * Sinon, explore le lien
                * Ajoute une tâche à la file d'attente du **Service d'Index Inversé** pour générer un [index inversé](https://en.wikipedia.org/wiki/Search_engine_indexing)
                * Ajoute une tâche à la file d'attente du **Service de Document** pour générer un titre et un extrait statiques
                * Génère la signature de la page
                * Supprime le lien de `links_to_crawl` dans la **Base de données NoSQL**
                * Insère le lien de page et la signature dans `crawled_links` dans la **Base de données NoSQL**

**Clarifiez avec votre intervieweur la quantité de code que vous êtes censé écrire**.

`PagesDataStore` est une abstraction au sein du **Service de Robot d'Exploration** qui utilise la **Base de données NoSQL** :

```python
class PagesDataStore(object):

    def __init__(self, db):
        self.db = db
        ...

    def add_link_to_crawl(self, url):
        """Ajoute le lien donné à `links_to_crawl`."""
        ...

    def remove_link_to_crawl(self, url):
        """Supprime le lien donné de `links_to_crawl`."""
        ...

    def reduce_priority_link_to_crawl(self, url):
        """Réduit la priorité d'un lien dans `links_to_crawl` pour éviter les cycles."""
        ...

    def extract_max_priority_page(self):
        """Renvoie le lien de priorité la plus élevée dans `links_to_crawl`."""
        ...

    def insert_crawled_link(self, url, signature):
        """Ajoute le lien donné à `crawled_links`."""
        ...

    def crawled_similar(self, signature):
        """Détermine si nous avons déjà exploré une page correspondant à la signature donnée"""
        ...
```

`Page` est une abstraction au sein du **Service de Robot d'Exploration** qui encapsule une page, son contenu, ses URLs enfants et sa signature :

```python
class Page(object):

    def __init__(self, url, contents, child_urls, signature):
        self.url = url
        self.contents = contents
        self.child_urls = child_urls
        self.signature = signature
```

`Crawler` est la classe principale au sein du **Service de Robot d'Exploration**, composée de `Page` et `PagesDataStore`.

```python
class Crawler(object):

    def __init__(self, data_store, reverse_index_queue, doc_index_queue):
        self.data_store = data_store
        self.reverse_index_queue = reverse_index_queue
        self.doc_index_queue = doc_index_queue

    def create_signature(self, page):
        """Crée une signature basée sur l'URL et le contenu."""
        ...

    def crawl_page(self, page):
        for url in page.child_urls:
            self.data_store.add_link_to_crawl(url)
        page.signature = self.create_signature(page)
        self.data_store.remove_link_to_crawl(page.url)
        self.data_store.insert_crawled_link(page.url, page.signature)

    def crawl(self):
        while True:
            page = self.data_store.extract_max_priority_page()
            if page is None:
                break
            if self.data_store.crawled_similar(page.signature):
                self.data_store.reduce_priority_link_to_crawl(page.url)
            else:
                self.crawl_page(page)
```

### Gestion des doublons

Nous devons être prudents pour que le robot d'exploration ne se retrouve pas dans une boucle infinie, ce qui se produit lorsque le graphe contient un cycle.

**Clarifiez avec votre intervieweur la quantité de code que vous êtes censé écrire**.

Nous voudrons supprimer les URLs en double :

* Pour des listes plus petites, nous pourrions utiliser quelque chose comme `sort | unique`
* Avec 1 milliard de liens à explorer, nous pourrions utiliser **MapReduce** pour ne produire que les entrées qui ont une fréquence de 1

```python
class RemoveDuplicateUrls(MRJob):

    def mapper(self, _, line):
        yield line, 1

    def reducer(self, key, values):
        total = sum(values)
        if total == 1:
            yield key, total
```

Détecter le contenu en double est plus complexe. Nous pourrions générer une signature basée sur le contenu de la page et comparer ces deux signatures pour leur similarité. Certains algorithmes potentiels sont [l'indice de Jaccard](https://en.wikipedia.org/wiki/Jaccard_index) et la [similarité cosinus](https://en.wikipedia.org/wiki/Cosine_similarity).

### Déterminer quand mettre à jour les résultats d'exploration

Les pages doivent être explorées régulièrement pour assurer leur fraîcheur. Les résultats d'exploration pourraient avoir un champ `timestamp` qui indique la dernière fois qu'une page a été explorée. Après une période par défaut, disons une semaine, toutes les pages devraient être rafraîchies. Les sites fréquemment mis à jour ou plus populaires pourraient être rafraîchis à des intervalles plus courts.

Bien que nous n'entrions pas dans les détails sur l'analytique, nous pourrions faire de l'exploration de données pour déterminer le temps moyen avant qu'une page particulière ne soit mise à jour, et utiliser cette statistique pour déterminer à quelle fréquence ré-explorer la page.

Nous pourrions également choisir de prendre en charge un fichier `Robots.txt` qui donne aux webmasters le contrôle de la fréquence d'exploration.

### Cas d'utilisation : L'utilisateur saisit un terme de recherche et voit une liste de pages pertinentes avec titres et extraits

* Le **Client** envoie une requête au **Serveur Web**, fonctionnant comme un [proxy inverse](https://github.com/donnemartin/system-design-primer#reverse-proxy-web-server)
* Le **Serveur Web** transmet la requête au serveur **API de Requête**
* Le serveur **API de Requête** fait ce qui suit :
    * Analyse la requête
        * Supprime le balisage
        * Décompose le texte en termes
        * Corrige les fautes de frappe
        * Normalise la capitalisation
        * Convertit la requête pour utiliser des opérations booléennes
    * Utilise le **Service d'Index Inversé** pour trouver les documents correspondant à la requête
        * Le **Service d'Index Inversé** classe les résultats correspondants et renvoie les meilleurs
    * Utilise le **Service de Document** pour renvoyer les titres et extraits

Nous utiliserons une [**API REST**](https://github.com/donnemartin/system-design-primer#representational-state-transfer-rest) publique :

```
$ curl https://search.com/api/v1/search?query=hello+world
```

Réponse :

```
{
    "title": "titre de foo",
    "snippet": "extrait de foo",
    "link": "https://foo.com",
},
{
    "title": "titre de bar",
    "snippet": "extrait de bar",
    "link": "https://bar.com",
},
{
    "title": "titre de baz",
    "snippet": "extrait de baz",
    "link": "https://baz.com",
},
```

Pour les communications internes, nous pourrions utiliser [Remote Procedure Calls](https://github.com/donnemartin/system-design-primer#remote-procedure-call-rpc).

## Étape 4 : Mettre à l'échelle la conception

> Identifier et traiter les goulots d'étranglement, compte tenu des contraintes.

![Imgur](http://i.imgur.com/bWxPtQA.png)

**Important : Ne passez pas directement de la conception initiale à la conception finale !**

Indiquez que vous 1) **Benchmarkeriez/Testeriez en charge**, 2) **Profileriez** pour identifier les goulots d'étranglement 3) traiteriez les goulots d'étranglement tout en évaluant les alternatives et les compromis, et 4) répéteriez. Voir [Concevoir un système qui s'adapte à des millions d'utilisateurs sur AWS](../scaling_aws/README.md) comme exemple sur la façon de mettre à l'échelle de manière itérative la conception initiale.

Il est important de discuter des goulots d'étranglement que vous pourriez rencontrer avec la conception initiale et comment vous pourriez les traiter. Par exemple, quels problèmes sont résolus en ajoutant un **Équilibreur de charge** avec plusieurs **Serveurs Web** ? **CDN** ? **Répliques Maître-Esclave** ? Quelles sont les alternatives et les **Compromis** pour chacun ?

Nous introduirons certains composants pour compléter la conception et traiter les problèmes de scalabilité. Les équilibreurs de charge internes ne sont pas montrés pour réduire l'encombrement.

*Pour éviter de répéter les discussions*, référez-vous aux [sujets de conception de systèmes](https://github.com/donnemartin/system-design-primer#index-of-system-design-topics) suivants pour les principaux points de discussion, les compromis et les alternatives :

* [DNS](https://github.com/donnemartin/system-design-primer#domain-name-system)
* [Équilibreur de charge](https://github.com/donnemartin/system-design-primer#load-balancer)
* [Mise à l'échelle horizontale](https://github.com/donnemartin/system-design-primer#horizontal-scaling)
* [Serveur web (proxy inverse)](https://github.com/donnemartin/system-design-primer#reverse-proxy-web-server)
* [Serveur API (couche application)](https://github.com/donnemartin/system-design-primer#application-layer)
* [Cache](https://github.com/donnemartin/system-design-primer#cache)
* [NoSQL](https://github.com/donnemartin/system-design-primer#nosql)
* [Modèles de cohérence](https://github.com/donnemartin/system-design-primer#consistency-patterns)
* [Modèles de disponibilité](https://github.com/donnemartin/system-design-primer#availability-patterns)

Certaines recherches sont très populaires, tandis que d'autres ne sont exécutées qu'une seule fois. Les requêtes populaires peuvent être servies depuis une **Mémoire Cache** comme Redis ou Memcached pour réduire les temps de réponse et pour éviter de surcharger le **Service d'Index Inversé** et le **Service de Document**. La **Mémoire Cache** est également utile pour gérer le trafic inégalement distribué et les pics de trafic. La lecture séquentielle de 1 Mo depuis la mémoire prend environ 250 microsecondes, tandis que la lecture depuis un SSD prend 4 fois plus de temps et depuis un disque dur 80 fois plus longtemps.<sup><a href=https://github.com/donnemartin/system-design-primer#latency-numbers-every-programmer-should-know>1</a></sup>

Voici quelques autres optimisations pour le **Service de Robot d'Exploration** :

* Pour gérer la taille des données et la charge des requêtes, le **Service d'Index Inversé** et le **Service de Document** auront probablement besoin de faire un usage intensif du partitionnement et de la fédération.
* La recherche DNS peut être un goulot d'étranglement, le **Service de Robot d'Exploration** peut maintenir sa propre recherche DNS qui est rafraîchie périodiquement.
* Le **Service de Robot d'Exploration** peut améliorer les performances et réduire l'utilisation de la mémoire en gardant de nombreuses connexions ouvertes en même temps, ce qu'on appelle le [pool de connexions](https://en.wikipedia.org/wiki/Connection_pool)
    * Passer à [UDP](https://github.com/donnemartin/system-design-primer#user-datagram-protocol-udp) pourrait également améliorer les performances
* L'exploration web est intensive en bande passante, assurez-vous qu'il y a suffisamment de bande passante pour maintenir un débit élevé

## Points de discussion supplémentaires

> Sujets supplémentaires à approfondir, selon la portée du problème et le temps restant.

### Modèles de mise à l'échelle SQL

* [Répliques de lecture](https://github.com/donnemartin/system-design-primer#master-slave-replication)
* [Fédération](https://github.com/donnemartin/system-design-primer#federation)
* [Partitionnement](https://github.com/donnemartin/system-design-primer#sharding)
* [Dénormalisation](https://github.com/donnemartin/system-design-primer#denormalization)
* [Optimisation SQL](https://github.com/donnemartin/system-design-primer#sql-tuning)

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
* Que mettre en cache
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