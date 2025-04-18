# Conception d'un système qui s'adapte à des millions d'utilisateurs sur AWS

*Remarque : Ce document renvoie directement aux domaines pertinents des [sujets de conception de systèmes](https://github.com/donnemartin/system-design-primer#index-of-system-design-topics) pour éviter les duplications. Référez-vous au contenu lié pour les points de discussion généraux, les compromis et les alternatives.*

## Étape 1 : Définir les cas d'utilisation et les contraintes

> Recueillir les exigences et délimiter le problème.
> Poser des questions pour clarifier les cas d'utilisation et les contraintes.
> Discuter des hypothèses.

Sans un intervieweur pour répondre aux questions de clarification, nous allons définir quelques cas d'utilisation et contraintes.

### Cas d'utilisation

La résolution de ce problème nécessite une approche itérative : 1) **Benchmarker/Tester en charge**, 2) **Profiler** pour identifier les goulots d'étranglement 3) traiter les goulots d'étranglement tout en évaluant les alternatives et les compromis, et 4) répéter, ce qui est un bon modèle pour faire évoluer des conceptions de base vers des conceptions évolutives.

À moins que vous n'ayez une expérience avec AWS ou que vous postuliez pour un poste qui nécessite des connaissances AWS, les détails spécifiques à AWS ne sont pas une exigence. Cependant, **la plupart des principes discutés dans cet exercice peuvent s'appliquer plus généralement en dehors de l'écosystème AWS.**

#### Nous allons limiter le problème pour traiter uniquement les cas d'utilisation suivants

* **Utilisateur** fait une requête de lecture ou d'écriture
    * **Service** effectue un traitement, stocke les données utilisateur, puis renvoie les résultats
* **Service** doit évoluer pour passer d'un petit nombre d'utilisateurs à des millions d'utilisateurs
    * Discuter des modèles généraux de mise à l'échelle à mesure que nous faisons évoluer une architecture pour gérer un grand nombre d'utilisateurs et de requêtes
* **Service** à haute disponibilité

### Contraintes et hypothèses

#### Hypothèses

* Le trafic n'est pas uniformément distribué
* Besoin de données relationnelles
* Mise à l'échelle de 1 utilisateur à des dizaines de millions d'utilisateurs
    * Dénoter l'augmentation des utilisateurs comme :
        * Utilisateurs+
        * Utilisateurs++
        * Utilisateurs+++
        * ...
    * 10 millions d'utilisateurs
    * 1 milliard d'écritures par mois
    * 100 milliards de lectures par mois
    * Ratio lecture/écriture de 100:1
    * 1 Ko de contenu par écriture

#### Calcul d'utilisation

**Clarifiez avec votre intervieweur si vous devez effectuer des calculs d'utilisation approximatifs.**

* 1 To de nouveau contenu par mois
    * 1 Ko par écriture * 1 milliard d'écritures par mois
    * 36 To de nouveau contenu en 3 ans
    * Supposons que la plupart des écritures concernent du nouveau contenu plutôt que des mises à jour de contenu existant
* 400 écritures par seconde en moyenne
* 40 000 lectures par seconde en moyenne

Guide de conversion pratique :

* 2,5 millions de secondes par mois
* 1 requête par seconde = 2,5 millions de requêtes par mois
* 40 requêtes par seconde = 100 millions de requêtes par mois
* 400 requêtes par seconde = 1 milliard de requêtes par mois

## Étape 2 : Créer une conception de haut niveau

> Esquisser une conception de haut niveau avec tous les composants importants.

![Imgur](http://i.imgur.com/B8LDKD7.png)

## Étape 3 : Concevoir les composants principaux

> Approfondir les détails de chaque composant principal.

### Cas d'utilisation : L'utilisateur fait une requête de lecture ou d'écriture

#### Objectifs

* Avec seulement 1-2 utilisateurs, vous n'avez besoin que d'une configuration de base
    * Une seule machine pour la simplicité
    * Mise à l'échelle verticale si nécessaire
    * Surveillance pour déterminer les goulots d'étranglement

#### Commencer avec une seule machine

* **Serveur web** sur EC2
    * Stockage pour les données utilisateur
    * [**Base de données MySQL**](https://github.com/donnemartin/system-design-primer#relational-database-management-system-rdbms)

Utiliser la **Mise à l'échelle verticale** :

* Simplement choisir une machine plus puissante
* Surveiller les métriques pour déterminer comment augmenter la capacité
    * Utiliser une surveillance de base pour déterminer les goulots d'étranglement : CPU, mémoire, E/S, réseau, etc.
    * CloudWatch, top, nagios, statsd, graphite, etc.
* La mise à l'échelle verticale peut devenir très coûteuse
* Pas de redondance/basculement

*Compromis, alternatives et détails supplémentaires :*

* L'alternative à la **Mise à l'échelle verticale** est la [**Mise à l'échelle horizontale**](https://github.com/donnemartin/system-design-primer#horizontal-scaling)

#### Commencer avec SQL, envisager NoSQL

Les contraintes supposent qu'il y a un besoin de données relationnelles. Nous pouvons commencer par utiliser une **Base de données MySQL** sur la machine unique.

*Compromis, alternatives et détails supplémentaires :*

* Voir la section [Système de gestion de base de données relationnelle (SGBDR)](https://github.com/donnemartin/system-design-primer#relational-database-management-system-rdbms)
* Discuter des raisons d'utiliser [SQL ou NoSQL](https://github.com/donnemartin/system-design-primer#sql-or-nosql)

#### Attribuer une IP statique publique

* Les IP élastiques fournissent un point de terminaison public dont l'IP ne change pas au redémarrage
* Aide au basculement, il suffit de pointer le domaine vers une nouvelle IP

#### Utiliser un DNS

Ajouter un **DNS** comme Route 53 pour mapper le domaine à l'IP publique de l'instance.

*Compromis, alternatives et détails supplémentaires :*

* Voir la section [Système de noms de domaine](https://github.com/donnemartin/system-design-primer#domain-name-system)

#### Sécuriser le serveur web

* N'ouvrir que les ports nécessaires
    * Permettre au serveur web de répondre aux requêtes entrantes de :
        * `80` pour HTTP
        * `443` pour HTTPS
        * `22` pour SSH uniquement depuis des IP autorisées
    * Empêcher le serveur web d'initier des connexions sortantes

*Compromis, alternatives et détails supplémentaires :*

* Voir la section [Sécurité](https://github.com/donnemartin/system-design-primer#security)

## Étape 4 : Mettre à l'échelle la conception

> Identifier et traiter les goulots d'étranglement, compte tenu des contraintes.

### Utilisateurs+

![Imgur](http://i.imgur.com/rrfjMXB.png)

#### Hypothèses

Notre nombre d'utilisateurs commence à augmenter et la charge augmente sur notre machine unique. Nos **Benchmarks/Tests de charge** et **Profilage** indiquent que la **Base de données MySQL** utilise de plus en plus de mémoire et de ressources CPU, tandis que le contenu utilisateur remplit l'espace disque.

Nous avons pu résoudre ces problèmes avec la **Mise à l'échelle verticale** jusqu'à présent. Malheureusement, cela est devenu assez coûteux et ne permet pas une mise à l'échelle indépendante de la **Base de données MySQL** et du **Serveur Web**.

#### Objectifs

* Alléger la charge sur la machine unique et permettre une mise à l'échelle indépendante
    * Stocker le contenu statique séparément dans un **Stockage d'Objets**
    * Déplacer la **Base de données MySQL** vers une machine séparée
* Inconvénients
    * Ces changements augmenteraient la complexité et nécessiteraient des modifications du **Serveur Web** pour pointer vers le **Stockage d'Objets** et la **Base de données MySQL**
    * Des mesures de sécurité supplémentaires doivent être prises pour sécuriser les nouveaux composants
    * Les coûts AWS pourraient également augmenter, mais devraient être comparés aux coûts de gestion de systèmes similaires par vous-même

#### Stocker le contenu statique séparément

* Envisager d'utiliser un **Stockage d'Objets** géré comme S3 pour stocker le contenu statique
    * Hautement évolutif et fiable
    * Chiffrement côté serveur
* Déplacer le contenu statique vers S3
    * Fichiers utilisateur
    * JS
    * CSS
    * Images
    * Vidéos

#### Déplacer la base de données MySQL vers une machine séparée

* Envisager d'utiliser un service comme RDS pour gérer la **Base de données MySQL**
    * Simple à administrer, à mettre à l'échelle
    * Zones de disponibilité multiples
    * Chiffrement au repos

#### Sécuriser le système

* Chiffrer les données en transit et au repos
* Utiliser un Cloud Privé Virtuel
    * Créer un sous-réseau public pour le **Serveur Web** unique afin qu'il puisse envoyer et recevoir du trafic depuis Internet
    * Créer un sous-réseau privé pour tout le reste, empêchant l'accès extérieur
    * N'ouvrir que les ports depuis des IP autorisées pour chaque composant
* Ces mêmes modèles devraient être mis en œuvre pour les nouveaux composants dans le reste de l'exercice

*Compromis, alternatives et détails supplémentaires :*

* Voir la section [Sécurité](https://github.com/donnemartin/system-design-primer#security)

### Utilisateurs++

![Imgur](http://i.imgur.com/raoFTXM.png)

#### Hypothèses

Nos **Benchmarks/Tests de charge** et **Profilage** montrent que notre **Serveur Web** unique devient un goulot d'étranglement pendant les heures de pointe, entraînant des réponses lentes et, dans certains cas, des temps d'arrêt. À mesure que le service mûrit, nous aimerions également évoluer vers une plus grande disponibilité et redondance.

#### Objectifs

* Les objectifs suivants tentent de résoudre les problèmes de mise à l'échelle avec le **Serveur Web**
    * En fonction des **Benchmarks/Tests de charge** et du **Profilage**, vous pourriez n'avoir besoin de mettre en œuvre qu'une ou deux de ces techniques
* Utiliser la [**Mise à l'échelle horizontale**](https://github.com/donnemartin/system-design-primer#horizontal-scaling) pour gérer des charges croissantes et traiter les points uniques de défaillance
    * Ajouter un [**Équilibreur de charge**](https://github.com/donnemartin/system-design-primer#load-balancer) comme l'ELB d'Amazon ou HAProxy
        * L'ELB est hautement disponible
        * Si vous configurez votre propre **Équilibreur de charge**, la mise en place de plusieurs serveurs en [actif-actif](https://github.com/donnemartin/system-design-primer#active-active) ou [actif-passif](https://github.com/donnemartin/system-design-primer#active-passive) dans plusieurs zones de disponibilité améliorera la disponibilité
        * Terminer SSL sur l'**Équilibreur de charge** pour réduire la charge de calcul sur les serveurs backend et simplifier l'administration des certificats
    * Utiliser plusieurs **Serveurs Web** répartis sur plusieurs zones de disponibilité
    * Utiliser plusieurs instances **MySQL** en mode [**Basculement Maître-Esclave**](https://github.com/donnemartin/system-design-primer#master-slave-replication) à travers plusieurs zones de disponibilité pour améliorer la redondance
* Séparer les **Serveurs Web** des [**Serveurs d'Application**](https://github.com/donnemartin/system-design-primer#application-layer)
    * Mettre à l'échelle et configurer les deux couches indépendamment
    * Les **Serveurs Web** peuvent fonctionner comme un [**Proxy inverse**](https://github.com/donnemartin/system-design-primer#reverse-proxy-web-server)
    * Par exemple, vous pouvez ajouter des **Serveurs d'Application** gérant les **API de Lecture** tandis que d'autres gèrent les **API d'Écriture**
* Déplacer le contenu statique (et certains contenus dynamiques) vers un [**Réseau de Diffusion de Contenu (CDN)**](https://github.com/donnemartin/system-design-primer#content-delivery-network) comme CloudFront pour réduire la charge et la latence

*Compromis, alternatives et détails supplémentaires :*

* Voir le contenu lié ci-dessus pour les détails

### Utilisateurs+++

![Imgur](http://i.imgur.com/OZCxJr0.png)

**Remarque :** Les **Équilibreurs de charge internes** ne sont pas montrés pour réduire l'encombrement

#### Hypothèses

Nos **Benchmarks/Tests de charge** et **Profilage** montrent que nous avons une forte charge de lecture (100:1 avec les écritures) et notre base de données souffre de mauvaises performances en raison des nombreuses requêtes de lecture.

#### Objectifs

* Les objectifs suivants tentent de résoudre les problèmes de mise à l'échelle avec la **Base de données MySQL**
    * En fonction des **Benchmarks/Tests de charge** et du **Profilage**, vous pourriez n'avoir besoin de mettre en œuvre qu'une ou deux de ces techniques
* Déplacer les données suivantes vers un [**Cache Mémoire**](https://github.com/donnemartin/system-design-primer#cache) comme Elasticache pour réduire la charge et la latence :
    * Contenu fréquemment accédé depuis **MySQL**
        * D'abord, essayez de configurer le cache de la **Base de données MySQL** pour voir si cela suffit à soulager le goulot d'étranglement avant de mettre en œuvre un **Cache Mémoire**
    * Données de session des **Serveurs Web**
        * Les **Serveurs Web** deviennent sans état, permettant l'**Autoscaling**
    * La lecture séquentielle de 1 Mo depuis la mémoire prend environ 250 microsecondes, tandis que la lecture depuis un SSD prend 4 fois plus de temps et depuis un disque dur 80 fois plus longtemps.<sup><a href=https://github.com/donnemartin/system-design-primer#latency-numbers-every-programmer-should-know>1</a></sup>
* Ajouter des [**Répliques de lecture MySQL**](https://github.com/donnemartin/system-design-primer#master-slave-replication) pour réduire la charge sur le maître d'écriture
* Ajouter plus de **Serveurs Web** et **Serveurs d'Application** pour améliorer la réactivité

*Compromis, alternatives et détails supplémentaires :*

* Voir le contenu lié ci-dessus pour les détails

#### Ajouter des répliques de lecture MySQL

* En plus d'ajouter et de mettre à l'échelle un **Cache Mémoire**, les **Répliques de lecture MySQL** peuvent également aider à soulager la charge sur le **Maître d'écriture MySQL**
* Ajouter une logique au **Serveur Web** pour séparer les écritures et les lectures
* Ajouter des **Équilibreurs de charge** devant les **Répliques de lecture MySQL** (non illustrés pour réduire l'encombrement)
* La plupart des services ont une charge de lecture plus importante que d'écriture

*Compromis, alternatives et détails supplémentaires :*

* Voir la section [Système de gestion de base de données relationnelle (SGBDR)](https://github.com/donnemartin/system-design-primer#relational-database-management-system-rdbms)

### Utilisateurs++++

![Imgur](http://i.imgur.com/3X8nmdL.png)

#### Hypothèses

Nos **Benchmarks/Tests de charge** et **Profilage** montrent que notre trafic augmente pendant les heures de bureau normales aux États-Unis et diminue considérablement lorsque les utilisateurs quittent le bureau. Nous pensons pouvoir réduire les coûts en augmentant et diminuant automatiquement le nombre de serveurs en fonction de la charge réelle. Nous sommes une petite équipe, donc nous aimerions automatiser autant que possible les opérations DevOps pour l'**Autoscaling** et pour les opérations générales.

#### Objectifs

* Ajouter l'**Autoscaling** pour provisionner la capacité selon les besoins
    * Suivre les pics de trafic
    * Réduire les coûts en éteignant les instances inutilisées
* Automatiser DevOps
    * Chef, Puppet, Ansible, etc.
* Continuer à surveiller les métriques pour traiter les goulots d'étranglement
    * **Niveau hôte** - Examiner une seule instance EC2
    * **Niveau agrégé** - Examiner les statistiques de l'équilibreur de charge
    * **Analyse des journaux** - CloudWatch, CloudTrail, Loggly, Splunk, Sumo
    * **Performance du site externe** - Pingdom ou New Relic
    * **Gérer les notifications et les incidents** - PagerDuty
    * **Rapport d'erreurs** - Sentry

#### Ajouter l'autoscaling

* Envisager un service géré comme l'**Autoscaling** AWS
    * Créer un groupe pour chaque type de **Serveur Web** et un pour chaque type de **Serveur d'Application**, placer chaque groupe dans plusieurs zones de disponibilité
    * Définir un nombre minimum et maximum d'instances
    * Déclencher la mise à l'échelle via CloudWatch
        * Simple métrique d'heure de la journée pour des charges prévisibles ou
        * Métriques sur une période de temps :
            * Charge CPU
            * Latence
            * Trafic réseau
            * Métrique personnalisée
    * Inconvénients
        * L'autoscaling peut introduire de la complexité
        * Il pourrait falloir un certain temps avant qu'un système ne s'adapte correctement pour répondre à une demande accrue, ou pour réduire lorsque la demande diminue

### Utilisateurs+++++

![Imgur](http://i.imgur.com/jj3A5N8.png)

**Remarque :** Les groupes d'**Autoscaling** ne sont pas montrés pour réduire l'encombrement

#### Hypothèses

À mesure que le service continue de croître vers les chiffres décrits dans les contraintes, nous exécutons itérativement des **Benchmarks/Tests de charge** et du **Profilage** pour découvrir et traiter les nouveaux goulots d'étranglement.

#### Objectifs

Nous continuerons à traiter les problèmes de mise à l'échelle dus aux contraintes du problème :

* Si notre **Base de données MySQL** commence à devenir trop volumineuse, nous pourrions envisager de ne stocker qu'une période limitée de données dans la base de données, tout en stockant le reste dans un entrepôt de données comme Redshift
    * Un entrepôt de données comme Redshift peut facilement gérer la contrainte de 1 To de nouveau contenu par mois
* Avec 40 000 requêtes de lecture moyennes par seconde, le trafic de lecture pour le contenu populaire peut être traité en mettant à l'échelle le **Cache Mémoire**, qui est également utile pour gérer le trafic inégalement distribué et les pics de trafic
    * Les **Répliques de lecture SQL** pourraient avoir du mal à gérer les échecs de cache, nous aurons probablement besoin d'employer des modèles de mise à l'échelle SQL supplémentaires
* 400 écritures moyennes par seconde (avec des pics probablement beaucoup plus élevés) pourraient être difficiles pour un seul **Maître-Esclave SQL d'écriture**, indiquant également un besoin de techniques de mise à l'échelle supplémentaires

Les modèles de mise à l'échelle SQL comprennent :

* [Fédération](https://github.com/donnemartin/system-design-primer#federation)
* [Partitionnement](https://github.com/donnemartin/system-design-primer#sharding)
* [Dénormalisation](https://github.com/donnemartin/system-design-primer#denormalization)
* [Optimisation SQL](https://github.com/donnemartin/system-design-primer#sql-tuning)

Pour traiter davantage les requêtes de lecture et d'écriture élevées, nous devrions également envisager de déplacer les données appropriées vers une [**Base de données NoSQL**](https://github.com/donnemartin/system-design-primer#nosql) comme DynamoDB.

Nous pouvons davantage séparer nos [**Serveurs d'Application**](https://github.com/donnemartin/system-design-primer#application-layer) pour permettre une mise à l'échelle indépendante. Les processus par lots ou les calculs qui n'ont pas besoin d'être effectués en temps réel peuvent être effectués de manière [**Asynchrone**](https://github.com/donnemartin/system-design-primer#asynchronism) avec des **Files d'attente** et des **Workers** :

* Par exemple, dans un service de photos, le téléchargement de la photo et la création de la vignette peuvent être séparés :
    * Le **Client** télécharge la photo
    * Le **Serveur d'Application** place une tâche dans une **File d'attente** comme SQS
    * Le **Service Worker** sur EC2 ou Lambda extrait le travail de la **File d'attente** puis :
        * Crée une vignette
        * Met à jour une **Base de données**
        * Stocke la vignette dans le **Stockage d'Objets**

*Compromis, alternatives et détails supplémentaires :*

* Voir le contenu lié ci-dessus pour les détails

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