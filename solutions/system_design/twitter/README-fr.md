# Conception de la timeline et de la recherche Twitter

*Remarque : Ce document renvoie directement aux domaines pertinents des [sujets de conception de systèmes](https://github.com/donnemartin/system-design-primer#index-of-system-design-topics) pour éviter les duplications. Référez-vous au contenu lié pour les points de discussion généraux, les compromis et les alternatives.*

**Concevoir le fil d'actualité Facebook** et **Concevoir la recherche Facebook** sont des questions similaires.

## Étape 1 : Définir les cas d'utilisation et les contraintes

> Recueillir les exigences et délimiter le problème.
> Poser des questions pour clarifier les cas d'utilisation et les contraintes.
> Discuter des hypothèses.

Sans un intervieweur pour répondre aux questions de clarification, nous allons définir quelques cas d'utilisation et contraintes.

### Cas d'utilisation

#### Nous allons limiter le problème pour traiter uniquement les cas d'utilisation suivants

* **Utilisateur** publie un tweet
    * **Service** pousse les tweets aux abonnés, envoyant des notifications push et des emails
* **Utilisateur** consulte la timeline utilisateur (activité de l'utilisateur)
* **Utilisateur** consulte la timeline d'accueil (activité des personnes que l'utilisateur suit)
* **Utilisateur** recherche des mots-clés
* **Service** à haute disponibilité

#### Hors du champ d'application

* **Service** pousse les tweets vers Twitter Firehose et autres flux
* **Service** filtre les tweets en fonction des paramètres de visibilité des utilisateurs
    * Masquer les @réponses si l'utilisateur ne suit pas également la personne à qui on répond
    * Respecter le paramètre 'masquer les retweets'
* Analytique

### Contraintes et hypothèses

#### Hypothèses

Général

* Le trafic n'est pas uniformément distribué
* Publier un tweet devrait être rapide
    * La diffusion d'un tweet à tous vos abonnés devrait être rapide, sauf si vous avez des millions d'abonnés
* 100 millions d'utilisateurs actifs
* 500 millions de tweets par jour ou 15 milliards de tweets par mois
    * Chaque tweet a en moyenne une diffusion de 10 livraisons
    * 5 milliards de tweets totaux livrés par diffusion par jour
    * 150 milliards de tweets livrés par diffusion par mois
* 250 milliards de requêtes de lecture par mois
* 10 milliards de recherches par mois

Timeline

* Consulter la timeline devrait être rapide
* Twitter est plus orienté lecture qu'écriture
    * Optimiser pour des lectures rapides des tweets
* L'ingestion des tweets est intensive en écriture

Recherche

* La recherche devrait être rapide
* La recherche est intensive en lecture

#### Calcul d'utilisation

**Clarifiez avec votre intervieweur si vous devez effectuer des calculs d'utilisation approximatifs.**

* Taille par tweet :
    * `tweet_id` - 8 octets
    * `user_id` - 32 octets
    * `text` - 140 octets
    * `media` - 10 Ko en moyenne
    * Total: ~10 Ko
* 150 To de nouveau contenu de tweet par mois
    * 10 Ko par tweet * 500 millions de tweets par jour * 30 jours par mois
    * 5,4 Po de nouveau contenu de tweet en 3 ans
* 100 mille requêtes de lecture par seconde
    * 250 milliards de requêtes de lecture par mois * (400 requêtes par seconde / 1 milliard de requêtes par mois)
* 6 000 tweets par seconde
    * 15 milliards de tweets par mois * (400 requêtes par seconde / 1 milliard de requêtes par mois)
* 60 mille tweets livrés par diffusion par seconde
    * 150 milliards de tweets livrés par diffusion par mois * (400 requêtes par seconde / 1 milliard de requêtes par mois)
* 4 000 requêtes de recherche par seconde
    * 10 milliards de recherches par mois * (400 requêtes par seconde / 1 milliard de requêtes par mois)

Guide de conversion pratique :

* 2,5 millions de secondes par mois
* 1 requête par seconde = 2,5 millions de requêtes par mois
* 40 requêtes par seconde = 100 millions de requêtes par mois
* 400 requêtes par seconde = 1 milliard de requêtes par mois

## Étape 2 : Créer une conception de haut niveau

> Esquisser une conception de haut niveau avec tous les composants importants.

![Imgur](http://i.imgur.com/48tEA2j.png)

## Étape 3 : Concevoir les composants principaux

> Approfondir les détails de chaque composant principal.

### Cas d'utilisation : L'utilisateur publie un tweet

Nous pourrions stocker les propres tweets de l'utilisateur pour alimenter la timeline utilisateur (activité de l'utilisateur) dans une [base de données relationnelle](https://github.com/donnemartin/system-design-primer#relational-database-management-system-rdbms). Nous devrions discuter des [cas d'utilisation et des compromis entre le choix de SQL ou NoSQL](https://github.com/donnemartin/system-design-primer#sql-or-nosql).

Livrer des tweets et construire la timeline d'accueil (activité des personnes que l'utilisateur suit) est plus délicat. La diffusion des tweets à tous les abonnés (60 mille tweets livrés par diffusion par seconde) surchargera une [base de données relationnelle](https://github.com/donnemartin/system-design-primer#relational-database-management-system-rdbms) traditionnelle. Nous voudrons probablement choisir un stockage de données avec des écritures rapides comme une **base de données NoSQL** ou un **Cache Mémoire**. La lecture séquentielle de 1 Mo depuis la mémoire prend environ 250 microsecondes, tandis que la lecture depuis un SSD prend 4 fois plus de temps et depuis un disque dur 80 fois plus longtemps.<sup><a href=https://github.com/donnemartin/system-design-primer#latency-numbers-every-programmer-should-know>1</a></sup>

Nous pourrions stocker les médias tels que les photos ou les vidéos sur un **Stockage d'Objets**.

* Le **Client** publie un tweet au **Serveur Web**, fonctionnant comme un [proxy inverse](https://github.com/donnemartin/system-design-primer#reverse-proxy-web-server)
* Le **Serveur Web** transmet la requête au serveur **API d'Écriture**
* Le serveur **API d'Écriture** stocke le tweet dans la table `tweets` de la **Base de données SQL**
    * La requête de stockage du tweet pourrait être mise en file d'attente dans un **Message Queue** pour être traitée de manière asynchrone
* Le serveur **API d'Écriture** envoie le tweet au **Service de Diffusion**, qui fait ce qui suit :
    * Consulte la table `followers` de la **Base de données SQL** pour trouver les abonnés de l'utilisateur
    * Stocke le tweet dans la **Mémoire Cache** de chaque abonné
    * Stocke le tweet dans la **Mémoire Cache** de la timeline utilisateur
    * Utilise le **Service de Notification** pour envoyer des notifications push et des emails aux abonnés

**Clarifiez avec votre intervieweur la quantité de code que vous êtes censé écrire**.

La table `tweets` pourrait avoir la structure suivante :

```
tweet_id    int             NOT NULL
user_id     int             NOT NULL
text        varchar(140)    NOT NULL
media_id    int
timestamp   datetime        NOT NULL
PRIMARY KEY(tweet_id)
FOREIGN KEY(user_id)    REFERENCES users(user_id)
FOREIGN KEY(media_id)   REFERENCES media(media_id)
```

Nous devrions créer un [index](https://github.com/donnemartin/system-design-primer#use-good-indices) sur `tweet_id` et `user_id` pour accélérer les recherches (temps logarithmique au lieu de parcourir toute la table) et pour garder les données en mémoire. La lecture séquentielle de 1 Mo depuis la mémoire prend environ 250 microsecondes, tandis que la lecture depuis un SSD prend 4 fois plus de temps et depuis un disque dur 80 fois plus longtemps.<sup><a href=https://github.com/donnemartin/system-design-primer#latency-numbers-every-programmer-should-know>1</a></sup>

La table `users` pourrait avoir la structure suivante :

```
user_id     int         NOT NULL
name        varchar(32) NOT NULL
email       varchar(32) NOT NULL
last_login  datetime    NOT NULL
PRIMARY KEY(user_id)
```

La table `followers` pourrait avoir la structure suivante :

```
user_id         int     NOT NULL
follower_id     int     NOT NULL
PRIMARY KEY (user_id, follower_id)
FOREIGN KEY(user_id)        REFERENCES users(user_id)
FOREIGN KEY(follower_id)    REFERENCES users(user_id)
```

Nous utiliserons une [**API REST**](https://github.com/donnemartin/system-design-primer#representational-state-transfer-rest) publique :

```
$ curl -X POST --data '{ "user_id": "123", "text": "hello world!", "media_id": "789" }' \
    https://twitter.com/api/v1/tweet
```

Pour les communications internes, nous pourrions utiliser [Remote Procedure Calls](https://github.com/donnemartin/system-design-primer#remote-procedure-call-rpc).

### Cas d'utilisation : L'utilisateur consulte la timeline d'accueil

* Le **Client** envoie une requête au **Serveur Web**
* Le **Serveur Web** transmet la requête au serveur **API de Lecture**
* Le serveur **API de Lecture** fait ce qui suit :
    * Vérifie la **Mémoire Cache** pour la timeline d'accueil
    * Si la timeline n'est pas dans la **Mémoire Cache**, il récupère la timeline d'accueil depuis la **Base de données SQL**
        * Consulte la table `followers` pour trouver les personnes que l'utilisateur suit
        * Récupère les tweets récents pour chaque personne suivie
        * Stocke la timeline d'accueil dans la **Mémoire Cache**
    * Renvoie la timeline d'accueil

### Cas d'utilisation : L'utilisateur consulte la timeline utilisateur

* Le **Client** envoie une requête au **Serveur Web**
* Le **Serveur Web** transmet la requête au serveur **API de Lecture**
* Le serveur **API de Lecture** fait ce qui suit :
    * Vérifie la **Mémoire Cache** pour la timeline utilisateur
    * Si la timeline n'est pas dans la **Mémoire Cache**, il récupère la timeline utilisateur depuis la **Base de données SQL**
        * Récupère les tweets récents de l'utilisateur
        * Stocke la timeline utilisateur dans la **Mémoire Cache**
    * Renvoie la timeline utilisateur

### Cas d'utilisation : L'utilisateur recherche des mots-clés

* Le **Client** envoie une requête au **Serveur Web**
* Le **Serveur Web** transmet la requête au serveur **API de Recherche**
* Le serveur **API de Recherche** fait ce qui suit :
    * Transmet la requête au **Cluster de Recherche**, qui fait ce qui suit :
        * Analyse la requête
        * Exécute la requête sur un [Index Inversé](https://en.wikipedia.org/wiki/Inverted_index)
        * Renvoie les résultats
    * Renvoie les résultats

## Étape 4 : Mettre à l'échelle la conception

> Identifier et traiter les goulots d'étranglement, compte tenu des contraintes.

![Imgur](http://i.imgur.com/jrUBAF7.png)

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
* [Cohérence](https://github.com/donnemartin/system-design-primer#consistency)
* [Disponibilité](https://github.com/donnemartin/system-design-primer#availability)

Nous devrons traiter les goulots d'étranglement suivants :

* La **Mémoire Cache** pour la timeline d'accueil pourrait être surchargée, entraînant des temps de réponse élevés ou des pannes
    * Nous pourrions mettre à l'échelle horizontalement en ajoutant plus de machines
* Le **Service de Diffusion** pourrait être surchargé, entraînant des retards dans la livraison des tweets aux abonnés
    * Nous pourrions mettre à l'échelle horizontalement en ajoutant plus de machines
    * Nous pourrions utiliser un **Message Queue** pour tamponner les tweets et les traiter de manière asynchrone
* La **Base de données SQL** pourrait être surchargée, entraînant des temps de réponse élevés ou des pannes
    * Nous pourrions mettre à l'échelle horizontalement en ajoutant des **Répliques de Lecture**
    * Nous pourrions [partitionner](https://github.com/donnemartin/system-design-primer#sharding) la base de données
    * Nous pourrions déplacer certaines données vers une **Base de données NoSQL**

#### Diffusion des tweets

Pour gérer la diffusion des tweets à des millions d'abonnés, nous pourrions explorer les approches suivantes :

* Approche par push
    * Diffuser les tweets à tous les abonnés lorsqu'ils sont publiés
    * Avantages :
        * La timeline d'accueil est toujours à jour
        * Les requêtes de lecture sont rapides
    * Inconvénients :
        * Coûteux pour les utilisateurs avec des millions d'abonnés
        * La plupart des tweets ne sont jamais lus
* Approche par pull
    * Récupérer les tweets des personnes suivies lorsque l'utilisateur consulte sa timeline d'accueil
    * Avantages :
        * Moins coûteux pour les utilisateurs avec des millions d'abonnés
        * Évite de diffuser des tweets qui ne seront jamais lus
    * Inconvénients :
        * La timeline d'accueil pourrait ne pas être à jour
        * Les requêtes de lecture sont plus lentes
* Approche hybride
    * Utiliser l'approche par push pour les utilisateurs avec peu d'abonnés
    * Utiliser l'approche par pull pour les utilisateurs avec des millions d'abonnés
    * Avantages :
        * Combine les avantages des deux approches
    * Inconvénients :
        * Complexité accrue

#### Recherche

Pour gérer des milliards de tweets et des milliers de requêtes de recherche par seconde, nous pourrions utiliser un **Cluster de Recherche** avec un [Index Inversé](https://en.wikipedia.org/wiki/Inverted_index).

* Un index inversé est une structure de données qui stocke une correspondance entre les mots et les documents qui les contiennent
* Chaque tweet serait traité et indexé lorsqu'il est publié
* Les requêtes de recherche seraient exécutées sur l'index inversé
* Les résultats seraient classés par pertinence, date, etc.

## Points de discussion supplémentaires

> Sujets supplémentaires à approfondir, selon la portée du problème et le temps restant.

### NoSQL

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