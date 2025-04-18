# Conception d'un cache clé-valeur pour sauvegarder les résultats des requêtes les plus récentes du serveur web

*Remarque : Ce document renvoie directement aux domaines pertinents des [sujets de conception de systèmes](https://github.com/donnemartin/system-design-primer#index-of-system-design-topics) pour éviter les duplications. Référez-vous au contenu lié pour les points de discussion généraux, les compromis et les alternatives.*

## Étape 1 : Définir les cas d'utilisation et les contraintes

> Recueillir les exigences et délimiter le problème.
> Poser des questions pour clarifier les cas d'utilisation et les contraintes.
> Discuter des hypothèses.

Sans un intervieweur pour répondre aux questions de clarification, nous allons définir quelques cas d'utilisation et contraintes.

### Cas d'utilisation

#### Nous allons limiter le problème pour traiter uniquement les cas d'utilisation suivants

* **Utilisateur** envoie une requête de recherche résultant en un succès de cache (cache hit)
* **Utilisateur** envoie une requête de recherche résultant en un échec de cache (cache miss)
* **Service** a haute disponibilité

### Contraintes et hypothèses

#### Hypothèses

* Le trafic n'est pas uniformément distribué
    * Les requêtes populaires devraient presque toujours être dans le cache
    * Besoin de déterminer comment expirer/rafraîchir
* Servir depuis le cache nécessite des recherches rapides
* Faible latence entre les machines
* Mémoire limitée dans le cache
    * Besoin de déterminer que garder/supprimer
    * Besoin de mettre en cache des millions de requêtes
* 10 millions d'utilisateurs
* 10 milliards de requêtes par mois

#### Calcul d'utilisation

**Clarifiez avec votre intervieweur si vous devez effectuer des calculs d'utilisation approximatifs.**

* Le cache stocke une liste ordonnée de clé : requête, valeur : résultats
    * `query` - 50 octets
    * `title` - 20 octets
    * `snippet` - 200 octets
    * Total: 270 octets
* 2,7 To de données de cache par mois si toutes les 10 milliards de requêtes sont uniques et toutes sont stockées
    * 270 octets par recherche * 10 milliards de recherches par mois
    * Les hypothèses indiquent une mémoire limitée, besoin de déterminer comment expirer le contenu
* 4 000 requêtes par seconde

Guide de conversion pratique :

* 2,5 millions de secondes par mois
* 1 requête par seconde = 2,5 millions de requêtes par mois
* 40 requêtes par seconde = 100 millions de requêtes par mois
* 400 requêtes par seconde = 1 milliard de requêtes par mois

## Étape 2 : Créer une conception de haut niveau

> Esquisser une conception de haut niveau avec tous les composants importants.

![Imgur](http://i.imgur.com/KqZ3dSx.png)

## Étape 3 : Concevoir les composants principaux

> Approfondir les détails de chaque composant principal.

### Cas d'utilisation : L'utilisateur envoie une requête résultant en un succès de cache

Les requêtes populaires peuvent être servies depuis un **Cache Mémoire** comme Redis ou Memcached pour réduire la latence de lecture et pour éviter de surcharger le **Service d'Index Inversé** et le **Service de Documents**. La lecture séquentielle de 1 Mo depuis la mémoire prend environ 250 microsecondes, tandis que la lecture depuis un SSD prend 4 fois plus de temps et depuis un disque dur 80 fois plus longtemps.<sup><a href=https://github.com/donnemartin/system-design-primer#latency-numbers-every-programmer-should-know>1</a></sup>

Puisque le cache a une capacité limitée, nous utiliserons une approche du moins récemment utilisée (LRU) pour faire expirer les entrées plus anciennes.

* Le **Client** envoie une requête au **Serveur Web**, fonctionnant comme un [proxy inverse](https://github.com/donnemartin/system-design-primer#reverse-proxy-web-server)
* Le **Serveur Web** transmet la requête au serveur **API de Requête**
* Le serveur **API de Requête** fait ce qui suit :
    * Analyse la requête
        * Supprime le balisage
        * Décompose le texte en termes
        * Corrige les fautes de frappe
        * Normalise la capitalisation
        * Convertit la requête pour utiliser des opérations booléennes
    * Vérifie le **Cache Mémoire** pour le contenu correspondant à la requête
        * S'il y a un succès dans le **Cache Mémoire**, le **Cache Mémoire** fait ce qui suit :
            * Met à jour la position de l'entrée mise en cache au début de la liste LRU
            * Renvoie le contenu mis en cache
        * Sinon, l'**API de Requête** fait ce qui suit :
            * Utilise le **Service d'Index Inversé** pour trouver les documents correspondant à la requête
                * Le **Service d'Index Inversé** classe les résultats correspondants et renvoie les meilleurs
            * Utilise le **Service de Documents** pour renvoyer les titres et les extraits
            * Met à jour le **Cache Mémoire** avec le contenu, plaçant l'entrée au début de la liste LRU

#### Implémentation du cache

Le cache peut utiliser une liste doublement chaînée : les nouveaux éléments seront ajoutés à la tête tandis que les éléments à expirer seront supprimés de la queue. Nous utiliserons une table de hachage pour des recherches rapides vers chaque nœud de la liste chaînée.

**Clarifiez avec votre intervieweur la quantité de code que vous êtes censé écrire**.

Implémentation du **Serveur API de Requête** :

```python
class QueryApi(object):

    def __init__(self, memory_cache, reverse_index_service):
        self.memory_cache = memory_cache
        self.reverse_index_service = reverse_index_service

    def parse_query(self, query):
        """Supprime le balisage, décompose le texte en termes, traite les fautes de frappe,
        normalise la capitalisation, convertit pour utiliser des opérations booléennes.
        """
        ...

    def process_query(self, query):
        query = self.parse_query(query)
        results = self.memory_cache.get(query)
        if results is None:
            results = self.reverse_index_service.process_search(query)
            self.memory_cache.set(query, results)
        return results
```

Implémentation du **Nœud** :

```python
class Node(object):

    def __init__(self, query, results):
        self.query = query
        self.results = results
```

Implémentation de la **Liste Chaînée** :

```python
class LinkedList(object):

    def __init__(self):
        self.head = None
        self.tail = None

    def move_to_front(self, node):
        ...

    def append_to_front(self, node):
        ...

    def remove_from_tail(self):
        ...
```

Implémentation du **Cache** :

```python
class Cache(object):

    def __init__(self, MAX_SIZE):
        self.MAX_SIZE = MAX_SIZE
        self.size = 0
        self.lookup = {}  # clé : requête, valeur : nœud
        self.linked_list = LinkedList()

    def get(self, query)
        """Obtient le résultat de requête stocké du cache.

        Accéder à un nœud met à jour sa position au début de la liste LRU.
        """
        node = self.lookup[query]
        if node is None:
            return None
        self.linked_list.move_to_front(node)
        return node.results

    def set(self, results, query):
        """Définit le résultat pour la clé de requête donnée dans le cache.

        Lors de la mise à jour d'une entrée, met à jour sa position au début de la liste LRU.
        Si l'entrée est nouvelle et que le cache est à capacité, supprime l'entrée la plus ancienne
        avant que la nouvelle entrée ne soit ajoutée.
        """
        node = self.lookup[query]
        if node is not None:
            # La clé existe dans le cache, mettre à jour la valeur
            node.results = results
            self.linked_list.move_to_front(node)
        else:
            # La clé n'existe pas dans le cache
            if self.size == self.MAX_SIZE:
                # Supprimer l'entrée la plus ancienne de la liste chaînée et de la recherche
                self.lookup.pop(self.linked_list.tail.query, None)
                self.linked_list.remove_from_tail()
            else:
                self.size += 1
            # Ajouter la nouvelle clé et valeur
            new_node = Node(query, results)
            self.linked_list.append_to_front(new_node)
            self.lookup[query] = new_node
```

#### Quand mettre à jour le cache

Le cache devrait être mis à jour quand :

* Le contenu de la page change
* La page est supprimée ou une nouvelle page est ajoutée
* Le classement de la page change

La façon la plus simple de gérer ces cas est de simplement définir un temps maximum pendant lequel une entrée mise en cache peut rester dans le cache avant d'être mise à jour, généralement appelé durée de vie (TTL).

Référez-vous à [Quand mettre à jour le cache](https://github.com/donnemartin/system-design-primer#when-to-update-the-cache) pour les compromis et les alternatives. L'approche ci-dessus décrit [cache aside](https://github.com/donnemartin/system-design-primer#cache-aside).

## Étape 4 : Mettre à l'échelle la conception

> Identifier et traiter les goulots d'étranglement, compte tenu des contraintes.

![Imgur](http://i.imgur.com/4j99mhe.png)

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
* [Modèles de cohérence](https://github.com/donnemartin/system-design-primer#consistency-patterns)
* [Modèles de disponibilité](https://github.com/donnemartin/system-design-primer#availability-patterns)

### Extension du Cache Mémoire à plusieurs machines

Pour gérer la charge de requêtes importante et la grande quantité de mémoire nécessaire, nous allons mettre à l'échelle horizontalement. Nous avons trois options principales sur la façon de stocker les données sur notre cluster de **Cache Mémoire** :

* **Chaque machine du cluster de cache a son propre cache** - Simple, bien que cela entraînera probablement un faible taux de succès du cache.
* **Chaque machine du cluster de cache a une copie du cache** - Simple, bien que ce soit une utilisation inefficace de la mémoire.
* **Le cache est [partitionné](https://github.com/donnemartin/system-design-primer#sharding) sur toutes les machines du cluster de cache** - Plus complexe, bien que ce soit probablement la meilleure option. Nous pourrions utiliser le hachage pour déterminer quelle machine pourrait avoir les résultats mis en cache d'une requête en utilisant `machine = hash(query)`. Nous voudrons sûrement utiliser le [hachage cohérent](https://github.com/donnemartin/system-design-primer#under-development).

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