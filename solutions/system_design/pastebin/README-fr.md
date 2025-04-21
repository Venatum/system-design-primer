# Conception de Pastebin.com (ou Bit.ly)

*Remarque : Ce document renvoie directement aux domaines pertinents des [sujets de conception de systèmes](https://github.com/donnemartin/system-design-primer#index-of-system-design-topics) pour éviter les duplications. Référez-vous au contenu lié pour les points de discussion généraux, les compromis et les alternatives.*

**Conception de Bit.ly** - est une question similaire, sauf que pastebin nécessite de stocker le contenu du collage au lieu de l'URL non raccourcie originale.

## Étape 1 : Définir les cas d'utilisation et les contraintes

> Recueillir les exigences et délimiter le problème.
> Poser des questions pour clarifier les cas d'utilisation et les contraintes.
> Discuter des hypothèses.

Sans un intervieweur pour répondre aux questions de clarification, nous allons définir quelques cas d'utilisation et contraintes.

### Cas d'utilisation

#### Nous allons limiter le problème pour traiter uniquement les cas d'utilisation suivants

* **Utilisateur** saisit un bloc de texte et obtient un lien généré aléatoirement
    * Expiration
        * Le paramètre par défaut n'expire pas
        * Peut optionnellement définir une expiration temporisée
* **Utilisateur** saisit l'URL d'un collage et visualise son contenu
* **Utilisateur** est anonyme
* **Service** suit les analyses des pages
    * Statistiques de visites mensuelles
* **Service** supprime les collages expirés
* **Service** a une haute disponibilité

#### Hors du champ d'application

* **Utilisateur** s'inscrit pour un compte
    * **Utilisateur** vérifie son email
* **Utilisateur** se connecte à un compte enregistré
    * **Utilisateur** modifie le document
* **Utilisateur** peut définir la visibilité
* **Utilisateur** peut définir le lien court

### Contraintes et hypothèses

#### Hypothèses

* Le trafic n'est pas uniformément distribué
* Suivre un lien court doit être rapide
* Les collages sont uniquement du texte
* Les analyses de vues de page n'ont pas besoin d'être en temps réel
* 10 millions d'utilisateurs
* 10 millions d'écritures de collages par mois
* 100 millions de lectures de collages par mois
* Ratio lecture/écriture de 10:1

#### Calcul d'utilisation

**Clarifiez avec votre intervieweur si vous devez effectuer des calculs d'utilisation approximatifs.**

* Taille par collage
    * 1 Ko de contenu par collage
    * `shortlink` - 7 octets
    * `expiration_length_in_minutes` - 4 octets
    * `created_at` - 5 octets
    * `paste_path` - 255 octets
    * total = ~1,27 Ko
* 12,7 Go de nouveau contenu de collage par mois
    * 1,27 Ko par collage * 10 millions de collages par mois
    * ~450 Go de nouveau contenu de collage en 3 ans
    * 360 millions de liens courts en 3 ans
    * Supposons que la plupart sont de nouveaux collages plutôt que des mises à jour de collages existants
* 4 écritures de collage par seconde en moyenne
* 40 requêtes de lecture par seconde en moyenne

Guide de conversion pratique :

* 2,5 millions de secondes par mois
* 1 requête par seconde = 2,5 millions de requêtes par mois
* 40 requêtes par seconde = 100 millions de requêtes par mois
* 400 requêtes par seconde = 1 milliard de requêtes par mois

## Étape 2 : Créer une conception de haut niveau

> Esquisser une conception de haut niveau avec tous les composants importants.

![Imgur](http://i.imgur.com/BKsBnmG.png)

## Étape 3 : Concevoir les composants principaux

> Approfondir les détails de chaque composant principal.

### Cas d'utilisation : L'utilisateur saisit un bloc de texte et obtient un lien généré aléatoirement

Nous pourrions utiliser une [base de données relationnelle](https://github.com/donnemartin/system-design-primer#relational-database-management-system-rdbms) comme une grande table de hachage, mappant l'URL générée à un serveur de fichiers et un chemin contenant le fichier de collage.

Au lieu de gérer un serveur de fichiers, nous pourrions utiliser un **Stockage d'Objets** géré comme Amazon S3 ou un [stockage de documents NoSQL](https://github.com/donnemartin/system-design-primer#document-store).

Une alternative à une base de données relationnelle agissant comme une grande table de hachage, nous pourrions utiliser un [stockage clé-valeur NoSQL](https://github.com/donnemartin/system-design-primer#key-value-store). Nous devrions discuter des [compromis entre le choix de SQL ou NoSQL](https://github.com/donnemartin/system-design-primer#sql-or-nosql). La discussion suivante utilise l'approche de la base de données relationnelle.

* Le **Client** envoie une requête de création de collage au **Serveur Web**, fonctionnant comme un [proxy inverse](https://github.com/donnemartin/system-design-primer#reverse-proxy-web-server)
* Le **Serveur Web** transmet la requête au serveur **API d'Écriture**
* Le serveur **API d'Écriture** fait ce qui suit :
    * Génère une URL unique
        * Vérifie si l'URL est unique en cherchant un doublon dans la **Base de données SQL**
        * Si l'URL n'est pas unique, il en génère une autre
        * Si nous supportions une URL personnalisée, nous pourrions utiliser celle fournie par l'utilisateur (vérifier également les doublons)
    * Sauvegarde dans la table `pastes` de la **Base de données SQL**
    * Sauvegarde les données du collage dans le **Stockage d'Objets**
    * Renvoie l'URL

**Clarifiez avec votre intervieweur la quantité de code que vous êtes censé écrire**.

La table `pastes` pourrait avoir la structure suivante :

```
shortlink                     char(7)       NOT NULL
expiration_length_in_minutes  int           NOT NULL
created_at                    datetime      NOT NULL
paste_path                    varchar(255)  NOT NULL
PRIMARY KEY(shortlink)
```

Définir la clé primaire basée sur la colonne `shortlink` crée un [index](https://github.com/donnemartin/system-design-primer#use-good-indices) que la base de données utilise pour garantir l'unicité. Nous créerons un index supplémentaire sur `created_at` pour accélérer les recherches (temps logarithmique au lieu de parcourir toute la table) et pour garder les données en mémoire. La lecture séquentielle de 1 Mo depuis la mémoire prend environ 250 microsecondes, tandis que la lecture depuis un SSD prend 4 fois plus de temps et depuis un disque dur 80 fois plus longtemps.<sup><a href=https://github.com/donnemartin/system-design-primer#latency-numbers-every-programmer-should-know>1</a></sup>

Pour générer l'URL unique, nous pourrions :

* Prendre le hachage [**MD5**](https://en.wikipedia.org/wiki/MD5) de l'adresse IP de l'utilisateur + horodatage
    * MD5 est une fonction de hachage largement utilisée qui produit une valeur de hachage de 128 bits
    * MD5 est uniformément distribué
    * Alternativement, nous pourrions également prendre le hachage MD5 de données générées aléatoirement
* Encoder le hachage MD5 en [**Base 62**](https://www.kerstner.at/2012/07/shortening-strings-using-base-62-encoding/)
    * Base 62 encode en `[a-zA-Z0-9]` ce qui fonctionne bien pour les URLs, éliminant le besoin d'échapper des caractères spéciaux
    * Il n'y a qu'un seul résultat de hachage pour l'entrée originale et Base 62 est déterministe (pas d'aléatoire impliqué)
    * Base 64 est un autre encodage populaire, mais pose des problèmes pour les URLs en raison des caractères supplémentaires `+` et `/`
    * Le [pseudocode Base 62](http://stackoverflow.com/questions/742013/how-to-code-a-url-shortener) suivant s'exécute en temps O(k) où k est le nombre de chiffres = 7 :

```python
def base_encode(num, base=62):
    digits = []
    while num > 0
      remainder = modulo(num, base)
      digits.push(remainder)
      num = divide(num, base)
    digits = digits.reverse
```

* Prendre les 7 premiers caractères de la sortie, ce qui donne 62^7 valeurs possibles et devrait être suffisant pour gérer notre contrainte de 360 millions de liens courts en 3 ans :

```python
url = base_encode(md5(ip_address+timestamp))[:URL_LENGTH]
```

Nous utiliserons une [**API REST**](https://github.com/donnemartin/system-design-primer#representational-state-transfer-rest) publique :

```
$ curl -X POST --data '{ "expiration_length_in_minutes": "60", \
    "paste_contents": "Hello World!" }' https://pastebin.com/api/v1/paste
```

Réponse :

```
{
    "shortlink": "foobar"
}
```

Pour les communications internes, nous pourrions utiliser [Remote Procedure Calls](https://github.com/donnemartin/system-design-primer#remote-procedure-call-rpc).

### Cas d'utilisation : L'utilisateur saisit l'URL d'un collage et visualise son contenu

* Le **Client** envoie une requête de récupération de collage au **Serveur Web**
* Le **Serveur Web** transmet la requête au serveur **API de Lecture**
* Le serveur **API de Lecture** fait ce qui suit :
    * Vérifie la **Base de données SQL** pour l'URL générée
        * Si l'URL est dans la **Base de données SQL**, récupère le contenu du collage depuis le **Stockage d'Objets**
        * Sinon, renvoie un message d'erreur à l'utilisateur

API REST :

```
$ curl https://pastebin.com/api/v1/paste?shortlink=foobar
```

Réponse :

```
{
    "paste_contents": "Hello World"
    "created_at": "YYYY-MM-DD HH:MM:SS"
    "expiration_length_in_minutes": "60"
}
```

### Cas d'utilisation : Le service suit les analyses des pages

Puisque les analyses en temps réel ne sont pas une exigence, nous pourrions simplement utiliser **MapReduce** sur les journaux du **Serveur Web** pour générer des compteurs de visites.

**Clarifiez avec votre intervieweur la quantité de code que vous êtes censé écrire**.

```python
class HitCounts(MRJob):

    def extract_url(self, line):
        """Extrait l'URL générée de la ligne de journal."""
        ...

    def extract_year_month(self, line):
        """Renvoie les parties année et mois de l'horodatage."""
        ...

    def mapper(self, _, line):
        """Analyse chaque ligne de journal, extrait et transforme les lignes pertinentes.

        Émet des paires clé-valeur de la forme :

        (2016-01, url0), 1
        (2016-01, url0), 1
        (2016-01, url1), 1
        """
        url = self.extract_url(line)
        period = self.extract_year_month(line)
        yield (period, url), 1

    def reducer(self, key, values):
        """Somme les valeurs pour chaque clé.

        (2016-01, url0), 2
        (2016-01, url1), 1
        """
        yield key, sum(values)
```

### Cas d'utilisation : Le service supprime les collages expirés

Pour supprimer les collages expirés, nous pourrions simplement scanner la **Base de données SQL** pour toutes les entrées dont l'horodatage d'expiration est plus ancien que l'horodatage actuel. Toutes les entrées expirées seraient alors supprimées (ou marquées comme expirées) de la table.

## Étape 4 : Mettre à l'échelle la conception

> Identifier et traiter les goulots d'étranglement, compte tenu des contraintes.

![Imgur](http://i.imgur.com/4edXG0T.png)

**Important : Ne passez pas directement de la conception initiale à la conception finale !**

Indiquez que vous feriez cela de manière itérative :
1. **Benchmarkeriez/Testeriez en charge**
2. **Profileriez** pour identifier les goulots d'étranglement
3. Traiteriez les goulots d'étranglement tout en évaluant les alternatives et les compromis
4. Répéteriez ces actions

Voir [Concevoir un système qui s'adapte à des millions d'utilisateurs sur AWS](../scaling_aws/README.md) comme exemple sur la façon de mettre à l'échelle de manière itérative la conception initiale.

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

Un **Stockage d'Objets** comme Amazon S3 peut facilement gérer la contrainte de 12,7 Go de nouveau contenue par mois.

Pour gérer les 40 requêtes de lecture *moyennes* par seconde (plus élevées en période de pointe), le trafic pour le contenu populaire devrait être géré par le **Cache Mémoire** plutôt que par la base de données. Le **Cache Mémoire** est également utile pour gérer le trafic inégalement distribué et les pics de trafic. Les **Répliques de Lecture SQL** devraient pouvoir gérer les échecs de cache, tant que les répliques ne sont pas surchargées par la réplication des écritures.

4 écritures de collage *moyennes* par seconde (avec des pics plus élevés) devraient être faisables pour un seul **Maître-Esclave SQL d'Écriture**. Sinon, nous devrons employer des modèles de mise à l'échelle SQL supplémentaires :

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