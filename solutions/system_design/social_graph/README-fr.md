# Conception des structures de données pour un réseau social

*Remarque : Ce document renvoie directement aux domaines pertinents des [sujets de conception de systèmes](https://github.com/donnemartin/system-design-primer#index-of-system-design-topics) pour éviter les duplications. Référez-vous au contenu lié pour les points de discussion généraux, les compromis et les alternatives.*

## Étape 1 : Définir les cas d'utilisation et les contraintes

> Recueillir les exigences et délimiter le problème.
> Poser des questions pour clarifier les cas d'utilisation et les contraintes.
> Discuter des hypothèses.

Sans un intervieweur pour répondre aux questions de clarification, nous allons définir quelques cas d'utilisation et contraintes.

### Cas d'utilisation

#### Nous allons limiter le problème pour traiter uniquement les cas d'utilisation suivants

* **Utilisateur** recherche quelqu'un et voit le chemin le plus court vers la personne recherchée
* **Service** à haute disponibilité

### Contraintes et hypothèses

#### Hypothèses

* Le trafic n'est pas uniformément distribué
    * Certaines recherches sont plus populaires que d'autres, tandis que d'autres ne sont exécutées qu'une seule fois
* Les données du graphe ne tiendront pas sur une seule machine
* Les arêtes du graphe ne sont pas pondérées
* 100 millions d'utilisateurs
* 50 amis par utilisateur en moyenne
* 1 milliard de recherches d'amis par mois

Exercer l'utilisation de systèmes plus traditionnels — ne pas utiliser de solutions spécifiques aux graphes comme [GraphQL](http://graphql.org/) ou une base de données de graphes comme [Neo4j](https://neo4j.com/)

#### Calcul d'utilisation

**Clarifiez avec votre intervieweur si vous devez effectuer des calculs d'utilisation approximatifs.**

* 5 milliards de relations d'amitié
    * 100 millions d'utilisateurs * 50 amis par utilisateur en moyenne
* 400 requêtes de recherche par seconde

Guide de conversion pratique :

* 2,5 millions de secondes par mois
* 1 requête par seconde = 2,5 millions de requêtes par mois
* 40 requêtes par seconde = 100 millions de requêtes par mois
* 400 requêtes par seconde = 1 milliard de requêtes par mois

## Étape 2 : Créer une conception de haut niveau

> Esquisser une conception de haut niveau avec tous les composants importants.

![Imgur](http://i.imgur.com/wxXyq2J.png)

## Étape 3 : Concevoir les composants principaux

> Approfondir les détails de chaque composant principal.

### Cas d'utilisation : L'utilisateur recherche quelqu'un et voit le chemin le plus court vers la personne recherchée

**Clarifiez avec votre intervieweur la quantité de code que vous êtes censé écrire**.

Sans la contrainte de millions d'utilisateurs (sommets) et de milliards de relations d'amitié (arêtes), nous pourrions résoudre cette tâche de chemin le plus court non pondéré avec une approche BFS générale :

```python
class Graph(Graph):

    def shortest_path(self, source, dest):
        if source is None or dest is None:
            return None
        if source is dest:
            return [source.key]
        prev_node_keys = self._shortest_path(source, dest)
        if prev_node_keys is None:
            return None
        else:
            path_ids = [dest.key]
            prev_node_key = prev_node_keys[dest.key]
            while prev_node_key is not None:
                path_ids.append(prev_node_key)
                prev_node_key = prev_node_keys[prev_node_key]
            return path_ids[::-1]

    def _shortest_path(self, source, dest):
        queue = deque()
        queue.append(source)
        prev_node_keys = {source.key: None}
        source.visit_state = State.visited
        while queue:
            node = queue.popleft()
            if node is dest:
                return prev_node_keys
            prev_node = node
            for adj_node in node.adj_nodes.values():
                if adj_node.visit_state == State.unvisited:
                    queue.append(adj_node)
                    prev_node_keys[adj_node.key] = prev_node.key
                    adj_node.visit_state = State.visited
        return None
```

Nous ne pourrons pas mettre tous les utilisateurs sur la même machine, nous devrons [partitionner](https://github.com/donnemartin/system-design-primer#sharding) les utilisateurs sur plusieurs **Serveurs de Personnes** et y accéder avec un **Service de Recherche**.

* Le **Client** envoie une requête au **Serveur Web**, fonctionnant comme un [proxy inverse](https://github.com/donnemartin/system-design-primer#reverse-proxy-web-server)
* Le **Serveur Web** transmet la requête au serveur **API de Recherche**
* Le serveur **API de Recherche** transmet la requête au **Service de Graphe d'Utilisateurs**
* Le **Service de Graphe d'Utilisateurs** fait ce qui suit :
    * Utilise le **Service de Recherche** pour trouver le **Serveur de Personnes** où les informations de l'utilisateur actuel sont stockées
    * Trouve le **Serveur de Personnes** approprié pour récupérer la liste des `friend_ids` de l'utilisateur actuel
    * Exécute une recherche BFS en utilisant l'utilisateur actuel comme `source` et les `friend_ids` de l'utilisateur actuel comme identifiants pour chaque `adjacent_node`
    * Pour obtenir l'`adjacent_node` à partir d'un identifiant donné :
        * Le **Service de Graphe d'Utilisateurs** devra *à nouveau* communiquer avec le **Service de Recherche** pour déterminer quel **Serveur de Personnes** stocke l'`adjacent_node` correspondant à l'identifiant donné (potentiel d'optimisation)

**Clarifiez avec votre intervieweur la quantité de code que vous devriez écrire**.

**Remarque** : La gestion des erreurs est exclue ci-dessous pour plus de simplicité. Demandez si vous devez coder une gestion correcte des erreurs.

Implémentation du **Service de Recherche** :

```python
class LookupService(object):

    def __init__(self):
        self.lookup = self._init_lookup()  # clé : person_id, valeur : person_server

    def _init_lookup(self):
        ...

    def lookup_person_server(self, person_id):
        return self.lookup[person_id]
```

Implémentation du **Serveur de Personnes** :

```python
class PersonServer(object):

    def __init__(self):
        self.people = {}  # clé : person_id, valeur : person

    def add_person(self, person):
        ...

    def people(self, ids):
        results = []
        for id in ids:
            if id in self.people:
                results.append(self.people[id])
        return results
```

Implémentation de **Personne** :

```python
class Person(object):

    def __init__(self, id, name, friend_ids):
        self.id = id
        self.name = name
        self.friend_ids = friend_ids
```

Implémentation du **Service de Graphe d'Utilisateurs** :

```python
class UserGraphService(object):

    def __init__(self, lookup_service):
        self.lookup_service = lookup_service

    def person(self, person_id):
        person_server = self.lookup_service.lookup_person_server(person_id)
        return person_server.people([person_id])

    def shortest_path(self, source_key, dest_key):
        if source_key is None or dest_key is None:
            return None
        if source_key is dest_key:
            return [source_key]
        prev_node_keys = self._shortest_path(source_key, dest_key)
        if prev_node_keys is None:
            return None
        else:
            # Parcourir les path_ids à l'envers, en commençant par dest_key
            path_ids = [dest_key]
            prev_node_key = prev_node_keys[dest_key]
            while prev_node_key is not None:
                path_ids.append(prev_node_key)
                prev_node_key = prev_node_keys[prev_node_key]
            # Inverser la liste puisque nous avons parcouru à l'envers
            return path_ids[::-1]

    def _shortest_path(self, source_key, dest_key, path):
        # Utiliser l'id pour obtenir la Personne
        source = self.person(source_key)
        # Mettre à jour notre file bfs
        queue = deque()
        queue.append(source)
        # prev_node_keys garde une trace de chaque saut de
        # source_key à dest_key
        prev_node_keys = {source_key: None}
        # Nous utiliserons visited_ids pour garder une trace des nœuds que nous avons
        # visités, ce qui peut être différent d'un bfs typique où
        # cela peut être stocké dans le nœud lui-même
        visited_ids = set()
        visited_ids.add(source.id)
        while queue:
            node = queue.popleft()
            if node.key is dest_key:
                return prev_node_keys
            prev_node = node
            for friend_id in node.friend_ids:
                if friend_id not in visited_ids:
                    friend_node = self.person(friend_id)
                    queue.append(friend_node)
                    prev_node_keys[friend_id] = prev_node.key
                    visited_ids.add(friend_id)
        return None
```

Nous utiliserons une [**API REST**](https://github.com/donnemartin/system-design-primer#representational-state-transfer-rest) publique :

```
$ curl https://social.com/api/v1/friend_search?person_id=1234
```

Réponse :

```
{
    "person_id": "100",
    "name": "foo",
    "link": "https://social.com/foo",
},
{
    "person_id": "53",
    "name": "bar",
    "link": "https://social.com/bar",
},
{
    "person_id": "1234",
    "name": "baz",
    "link": "https://social.com/baz",
},
```

Pour les communications internes, nous pourrions utiliser [Remote Procedure Calls](https://github.com/donnemartin/system-design-primer#remote-procedure-call-rpc).

## Étape 4 : Mettre à l'échelle la conception

> Identifier et traiter les goulots d'étranglement, compte tenu des contraintes.

![Imgur](http://i.imgur.com/cdCv5g7.png)

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

Pour répondre à la contrainte de 400 requêtes de lecture *moyennes* par seconde (plus élevées en période de pointe), les données des personnes peuvent être servies depuis un **Cache Mémoire** comme Redis ou Memcached pour réduire les temps de réponse et pour réduire le trafic vers les services en aval. Cela pourrait être particulièrement utile pour les personnes qui font plusieurs recherches successives et pour les personnes qui sont bien connectées. La lecture séquentielle de 1 Mo depuis la mémoire prend environ 250 microsecondes, tandis que la lecture depuis un SSD prend 4 fois plus de temps et depuis un disque dur 80 fois plus longtemps.<sup><a href=https://github.com/donnemartin/system-design-primer#latency-numbers-every-programmer-should-know>1</a></sup>

Voici d'autres optimisations :

- **Stockez des parcours BFS complets ou partiels** afin d'accélérer les recherches suivantes dans la **mémoire cache**.
- **Effectuez des calculs en batch hors ligne**, puis stockez des parcours BFS complets ou partiels pour accélérer les recherches suivantes dans une **base de données NoSQL**.
- **Réduisez les sauts machine** en regroupant les recherches d'amis hébergées sur le même **serveur de personnes**.  
  - [Fragmenter](https://github.com/donnemartin/system-design-primer#sharding) les **serveurs de personnes** par emplacement pour améliorer cela, car les amis vivent généralement à proximité les uns des autres.
- **Effectuez deux recherches BFS en même temps** : une partant de la source et une de la destination, puis fusionnez les deux chemins.
- **Lancez la recherche BFS** en partant des personnes ayant un grand nombre d'amis, car elles peuvent davantage réduire le nombre de [degrés de séparation](https://en.wikipedia.org/wiki/Six_degrees_of_separation) entre l'utilisateur actuel et la cible de recherche.
- **Définissez une limite basée sur le temps ou le nombre d'étapes** avant de demander à l'utilisateur s'il souhaite continuer la recherche, car celle-ci pourrait prendre beaucoup de temps dans certains cas.
- **Utilisez une base de données de graphes**, comme [Neo4j](https://neo4j.com/), ou un langage de requête spécifique aux graphes, comme [GraphQL](http://graphql.org/) (s'il n'y avait pas de contrainte empêchant l'utilisation de **bases de données de graphes**).


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