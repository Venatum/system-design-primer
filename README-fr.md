*[Français](README-fr.md) ∙ [English](README.md) ∙ [日本語](README-ja.md) ∙ [简体中文](README-zh-Hans.md) ∙ [繁體中文](README-zh-TW.md) | [العَرَبِيَّة‎](https://github.com/donnemartin/system-design-primer/issues/170) ∙ [বাংলা](https://github.com/donnemartin/system-design-primer/issues/220) ∙ [Português do Brasil](https://github.com/donnemartin/system-design-primer/issues/40) ∙ [Deutsch](https://github.com/donnemartin/system-design-primer/issues/186) ∙ [ελληνικά](https://github.com/donnemartin/system-design-primer/issues/130) ∙ [עברית](https://github.com/donnemartin/system-design-primer/issues/272) ∙ [Italiano](https://github.com/donnemartin/system-design-primer/issues/104) ∙ [한국어](https://github.com/donnemartin/system-design-primer/issues/102) ∙ [فارسی](https://github.com/donnemartin/system-design-primer/issues/110) ∙ [Polski](https://github.com/donnemartin/system-design-primer/issues/68) ∙ [русский язык](https://github.com/donnemartin/system-design-primer/issues/87) ∙ [Español](https://github.com/donnemartin/system-design-primer/issues/136) ∙ [ภาษาไทย](https://github.com/donnemartin/system-design-primer/issues/187) ∙ [Türkçe](https://github.com/donnemartin/system-design-primer/issues/39) ∙ [tiếng Việt](https://github.com/donnemartin/system-design-primer/issues/127) | [Ajouter une traduction](https://github.com/donnemartin/system-design-primer/issues/28)*

**Aidez à [traduire](TRANSLATIONS.md) ce guide !**

# Introduction à la Conception de Systèmes

<p align="center">
  <img src="images/jj3A5N8.png">
  <br/>
</p>

## Motivation

> Apprenez à concevoir des systèmes à grande échelle.
>
> Préparez-vous aux entretiens de conception de systèmes.

### Apprenez à concevoir des systèmes à grande échelle

Apprendre à concevoir des systèmes évolutifs vous aidera à devenir un meilleur ingénieur.

La conception de systèmes est un sujet vaste. Il existe **une immense quantité de ressources éparpillées sur le web**
sur les principes de conception des systèmes.

Ce dépôt est une **collection organisée** de ressources pour vous aider à apprendre comment concevoir des systèmes à
grande échelle.

### Apprenez de la communauté open source

Ceci est un projet open source mis à jour en continu.

Les [contributions](#contributing) sont les bienvenues !

### Préparez-vous aux entretiens de conception de systèmes

En plus des entretiens de codage, la conception de systèmes est une **composante incontournable** du **processus
d'entretien technique** dans de nombreuses entreprises technologiques.

**Entraînez-vous sur des questions courantes d'entretien de conception de systèmes** et **comparez** vos résultats avec
**des solutions types** : discussions, code et diagrammes.

Sujets supplémentaires pour vous préparer aux entretiens :

* [Guide d'étude](#study-guide)
* [Comment aborder une question d'entretien de conception de système](#how-to-approach-a-system-design-interview-question)
* [Questions d'entretien en conception système, **avec solutions**](#system-design-interview-questions-with-solutions)
* [Questions d'entretien en conception orientée objet, **avec solutions
  **](#object-oriented-design-interview-questions-with-solutions)
* [Questions supplémentaires d'entretien en conception de systèmes](#additional-system-design-interview-questions)

## Cartes mémoire Anki

<p align="center">
  <img src="images/zdCAkB3.png">
  <br/>
</p>

Les [paquets de cartes Anki](https://apps.ankiweb.net/) proposés utilisent la répétition espacée pour vous aider à
retenir les concepts clés de la conception de systèmes.

* [Paquet "Conception de systèmes"](https://github.com/donnemartin/system-design-primer/tree/master/resources/flash_cards/System%20Design.apkg)
* [Paquet "Exercices de conception de systèmes"](https://github.com/donnemartin/system-design-primer/tree/master/resources/flash_cards/System%20Design%20Exercises.apkg)
* [Paquet "Exercices de conception orientée objet"](https://github.com/donnemartin/system-design-primer/tree/master/resources/flash_cards/OO%20Design.apkg)

Idéal pour une utilisation en déplacement.

### Ressource de programmation : Défis interactifs

Vous cherchez des ressources pour vous préparer à l'[**Entretien de programmation**](https://github.com/donnemartin/interactive-coding-challenges) ?

<p align="center">
  <img src="images/b4YtAEN.png">
  <br/>
</p>

Consultez le dépôt compagnon [**Défis interactifs de programmation**](https://github.com/donnemartin/interactive-coding-challenges), contenant un autre paquet Anki :

* [Paquet "Programmation"](https://github.com/donnemartin/interactive-coding-challenges/tree/master/anki_cards/Coding.apkg)

## Contribuer

> Apprenez de la communauté.

N'hésitez pas à soumettre des demandes de fusion pour :

* Corriger des erreurs
* Améliorer des sections
* Ajouter de nouvelles sections
* [Traduire](https://github.com/donnemartin/system-design-primer/issues/28)

Le contenu qui nécessite d'être peaufiné est placé [sous développement](#under-development).

Consultez les [directives de contribution](CONTRIBUTING.md).

## Index des sujets de conception de systèmes

> Résumés de divers sujets sur la conception de systèmes, avec avantages et inconvénients.  **Tout est un compromis**.
>
> Chaque section contient des liens vers des ressources plus approfondies.

<p align="center">
  <img src="images/jrUBAF7.png">
  <br/>
</p>

* [Sujets de conception système : commencez ici](#system-design-topics-start-here)
    * [Étape 1 : Revoir la vidéo sur l'évolutivité](#step-1-review-the-scalability-video-lecture)
    * [Étape 2 : Revoir l'article sur l'évolutivité](#step-2-review-the-scalability-article)
    * [Étapes suivantes](#next-steps)
* [Performance vs Évolutivité](#performance-vs-scalability)
* [Latence vs Débit](#latency-vs-throughput)
* [Disponibilité vs Cohérence](#availability-vs-consistency)
    * [Théorème CAP](#cap-theorem)
        * [CP - Cohérence et Tolérance aux partitions](#cp---consistency-and-partition-tolerance)
        * [AP - Disponibilité et Tolérance aux partitions](#ap---availability-and-partition-tolerance)
* [Modèles de cohérence](#consistency-patterns)
    * [Cohérence faible](#weak-consistency)
    * [Cohérence éventuelle](#eventual-consistency)
    * [Cohérence forte](#strong-consistency)
* [Modèles de disponibilité](#availability-patterns)
    * [Basculement (Fail-over)](#fail-over)
    * [Réplication](#replication)
    * [Disponibilité en chiffres](#availability-in-numbers)
* [Système de noms de domaine (DNS)](#domain-name-system)
* [Réseau de diffusion de contenu (CDN)](#content-delivery-network)
    * [CDNs en mode push](#push-cdns)
    * [CDNs en mode pull](#pull-cdns)
* [Équilibrage de charge (Load balancer)](#load-balancer)
    * [Actif-passif](#active-passive)
    * [Actif-actif](#active-active)
    * [Équilibrage de charge de niveau 4](#layer-4-load-balancing)
    * [Équilibrage de charge de niveau 7](#layer-7-load-balancing)
    * [Mise à l'échelle horizontale](#horizontal-scaling)
* [Proxy inverse (serveur web)](#reverse-proxy-web-server)
    * [Équilibrage de charge vs Proxy inverse](#load-balancer-vs-reverse-proxy)
* [Couches applicatives](#application-layer)
    * [Microservices](#microservices)
    * [Découverte de services](#service-discovery)
* [Bases de données](#database)
    * [Système de gestion de base de données relationnelle (SGBDR)](#relational-database-management-system-rdbms)
        * [Réplication maître-esclave](#master-slave-replication)
        * [Réplication maître-maître](#master-master-replication)
        * [Fédération](#federation)
        * [Partage (Sharding)](#sharding)
        * [Dénormalisation](#denormalization)
        * [Optimisation SQL](#sql-tuning)
    * [NoSQL](#nosql)
        * [Stockage clé-valeur](#key-value-store)
        * [Base de documents](#document-store)
        * [Stockage en colonnes larges](#wide-column-store)
        * [Base de données graphe](#graph-database)
    * [SQL ou NoSQL](#sql-or-nosql)
* [Cache](#cache)
    * [Cache client](#client-caching)
    * [Cache CDN](#cdn-caching)
    * [Cache serveur web](#web-server-caching)
    * [Cache base de données](#database-caching)
    * [Cache applicatif](#application-caching)
    * [Mise en cache au niveau des requêtes de base de données](#caching-at-the-database-query-level)
    * [Mise en cache au niveau des objets](#caching-at-the-object-level)
    * [Quand mettre à jour le cache](#when-to-update-the-cache)
        * [Cache-aside](#cache-aside)
        * [Write-through](#write-through)
        * [Write-behind (ou Write-back)](#write-behind-write-back)
        * [Refresh-ahead](#refresh-ahead)
* [Asynchronisme](#asynchronism)
    * [Files de messages](#message-queues)
    * [Files de tâches](#task-queues)
    * [Contre-pression (Back pressure)](#back-pressure)
* [Communication](#communication)
    * [Protocole de contrôle de transmission (TCP)](#transmission-control-protocol-tcp)
    * [Protocole de datagramme utilisateur (UDP)](#user-datagram-protocol-udp)
    * [Appel de procédure distante (RPC)](#remote-procedure-call-rpc)
    * [Représentation par transfert d'état (REST)](#representational-state-transfer-rest)
* [Sécurité](#security)
* [Annexes](#appendix)
    * [Tableau des puissances de deux](#powers-of-two-table)
    * [Chiffres de latence que chaque programmeur devrait connaître](#latency-numbers-every-programmer-should-know)
    * [Questions supplémentaires d'entretien en conception système](#additional-system-design-interview-questions)
    * [Architectures du monde réel](#real-world-architectures)
    * [Architectures d'entreprises](#company-architectures)
    * [Blogs d'ingénierie d'entreprises](#company-engineering-blogs)
* [En développement](#under-development)
* [Crédits](#credits)
* [Informations de contact](#contact-info)
* [Licence](#license)

## Guide d'étude

> Sujets suggérés à réviser en fonction de votre calendrier d'entretien (court, moyen, long).

![Imgur](images/OfVllex.png)

**Q : Pour les entretiens, dois-je tout savoir ici ?**

**R : Non, vous n'avez pas besoin de tout savoir pour vous préparer à l'entretien.**

Ce qui est demandé lors d'un entretien dépend de plusieurs variables telles que :

* Votre niveau d'expérience
* Votre formation technique
* Les postes pour lesquels vous passez un entretien
* Les entreprises avec lesquelles vous passez un entretien
* La chance

Les candidats plus expérimentés sont généralement censés en savoir plus sur la conception de systèmes. Les architectes
ou les chefs d'équipe peuvent être obligés d'en savoir plus que les contributeurs individuels. Les grandes entreprises
technologiques sont susceptibles d'avoir une ou plusieurs phases d'entretien de conception.

Commencez par une approche large, puis approfondissez certains sujets. Il est utile de connaître un peu divers sujets
clés de conception de systèmes. Ajustez le guide suivant en fonction de votre calendrier, de votre expérience, des
postes pour lesquels vous êtes candidat, et des entreprises concernées.

* **Calendrier court** - Ciblez **l'étendue** avec les sujets de conception système. Pratiquez en résolvant **quelques**
  questions d'entretien.
* **Calendrier moyen** - Ciblez **l'étendue** et **une certaine profondeur** avec les sujets de conception système.
  Pratiquez en résolvant **de nombreuses** questions d'entretien.
* **Calendrier long** - Ciblez **l'étendue** et **plus de profondeur** avec les sujets de conception système. Pratiquez
  en résolvant **la plupart** des questions d'entretien.

|                                                                                                                                                           | Court terme | Moyen terme | Long terme |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------|-------------|-------------|------------|
| Lire les [Sujets de conception système](#index-of-system-design-topics) pour mieux comprendre le fonctionnement global des systèmes                       | :+1:        | :+1:        | :+1:       |
| Lire quelques articles dans les [blogs d'ingénierie des entreprises](#company-engineering-blogs) des entreprises avec lesquelles vous passez un entretien | :+1:        | :+1:        | :+1:       |
| Lire quelques [Architectures du monde réel](#real-world-architectures)                                                                                    | :+1:        | :+1:        | :+1:       |
| Revoir [Comment aborder une question d'entretien en conception de systèmes](#how-to-approach-a-system-design-interview-question)                          | :+1:        | :+1:        | :+1:       |
| Travailler sur les [Questions d'entretien en conception de systèmes avec solutions](#system-design-interview-questions-with-solutions)                    | Un peu      | Beaucoup    | La plupart |
| Travailler sur les [Questions d'entretien en conception orientée objet avec solutions](#object-oriented-design-interview-questions-with-solutions)        | Un peu      | Beaucoup    | La plupart |
| Revoir les [Questions supplémentaires en conception de systèmes](#additional-system-design-interview-questions)                                           | Un peu      | Beaucoup    | La plupart |

## Comment aborder une question d'entretien en conception de système

> Comment traiter une question d'entretien en conception de système.

L'entretien de conception de système est une **conversation ouverte**. Vous êtes censé la diriger.

Vous pouvez utiliser les étapes suivantes pour guider la discussion. Pour maîtriser ce processus, entraînez-vous avec
des exemples dans la
section [Questions d'entretien en conception de systèmes avec solutions](#system-design-interview-questions-with-solutions).

### Étape 1 : Enoncer les cas d'utilisation, contraintes et hypothèses

Recueillez les exigences et définissez l'étendue du problème. Posez des questions pour clarifier les cas d'utilisation
et les contraintes. Discutez des hypothèses.

* Qui va l'utiliser ?
* Comment vont-ils l'utiliser ?
* Combien d'utilisateurs y aura-t-il ?
* Que fait le système ?
* Quelles sont les entrées et sorties du système ?
* Combien de données prévoyez-vous de traiter ?
* Combien de requêtes par seconde prévoyez-vous ?
* Quel est le ratio lecture/écriture attendu ?

### Étape 2 : Créer une conception au niveau supérieur

Faites un schéma global comprenant tous les composants importants.

* Dessinez les principaux composants et leurs connexions.
* Justifiez vos idées.

### Étape 3 : Concevoir les composants principaux

Entrez dans les détails de chaque composant principal. Par exemple, si on vous demande
de [concevoir un service de raccourcissement d'URL](solutions/system_design/pastebin/README.md), discutez de :

* Générer et stocker un hachage de l'URL complète :
    * [MD5](solutions/system_design/pastebin/README.md) et [Base62](solutions/system_design/pastebin/README.md)
    * Collisions de hachage
    * SQL ou NoSQL
    * Schéma de base de données
* Traduire une URL hachée vers l'URL complète :
    * Recherche dans une base de données
* API et conception orientée objet

### Étape 4 : Évoluer la conception

Identifiez et résolvez les goulets d'étranglement en fonction des contraintes. Par exemple, avez-vous besoin des
éléments suivants pour résoudre des problèmes d'évolutivité ?

* Équilibrage de charge
* Mise à l'échelle horizontale
* Mise en cache
* Partitionnement de base de données

Discutez des solutions potentielles et des compromis. Tout est une affaire de compromis. Abordez les goulets
d'étranglement en utilisant les [principes de conception de systèmes évolutifs](#index-of-system-design-topics).

### Calculs approximatifs

Il se peut qu'on vous demande de faire des estimations manuellement. Reportez-vous à [l'annexe](#appendix) pour les
ressources suivantes :

* [Utilisez les calculs approximatifs](http://highscalability.com/blog/2011/1/26/google-pro-tip-use-back-of-the-envelope-calculations-to-choo.html)
* [Tableau des puissances de deux](#powers-of-two-table)
* [Chiffres de latence que chaque programmeur devrait connaître](#latency-numbers-every-programmer-should-know)

### Sources et lectures complémentaires

Consultez les liens suivants pour avoir une meilleure idée de ce qui vous attend :

* [Comment réussir un entretien en conception de systèmes](https://www.palantir.com/2011/10/how-to-rock-a-systems-design-interview/)
* [L'entretien en conception de système](http://www.hiredintech.com/system-design)
* [Introduction à la conception d'architecture et aux entretiens en conception système](https://www.youtube.com/watch?v=ZgdS0EUmn70)
* [Modèle de conception système](https://leetcode.com/discuss/career/229177/My-System-Design-Template)

## Questions d'entretien en conception de système avec solutions

> Questions d'entretien courantes en conception de système avec des discussions, du code et des diagrammes d'exemple.
>
> Les solutions sont liées au contenu du dossier `solutions/`.

| Question                                                                                            |                                                               |
|-----------------------------------------------------------------------------------------------------|---------------------------------------------------------------|
| Concevez Pastebin.com (ou Bit.ly)                                                                   | [Solution](solutions/system_design/pastebin/README-fr.md)     |
| Concevez la timeline et la recherche de Twitter (ou le fil d'actualité et la recherche de Facebook) | [Solution](solutions/system_design/twitter/README-fr.md)      |
| Concevez un crawler web                                                                             | [Solution](solutions/system_design/web_crawler/README-fr.md)  |
| Concevez Mint.com                                                                                   | [Solution](solutions/system_design/mint/README-fr.md)         |
| Concevez les structures de données pour un réseau social                                            | [Solution](solutions/system_design/social_graph/README-fr.md) |
| Concevez un magasin clé-valeur pour un moteur de recherche                                          | [Solution](solutions/system_design/query_cache/README-fr.md)  |
| Concevez la fonctionnalité de classement par catégorie des ventes d'Amazon                          | [Solution](solutions/system_design/sales_rank/README-fr.md)   |
| Concevez un système pouvant évoluer pour atteindre des millions d'utilisateurs sur AWS              | [Solution](solutions/system_design/scaling_aws/README-fr.md)  |
| Ajouter une question de conception système                                                          | [Contribuer](#contributing)                                   |

### Concevoir Pastebin.com (ou Bit.ly)

[Voir exercice et solution](solutions/system_design/pastebin/README-fr.md)

![Imgur](images/4edXG0T.png)

### Concevez la timeline et la recherche de Twitter (ou le fil d'actualité et la recherche de Facebook)

[Voir exercice et solution](solutions/system_design/twitter/README-fr.md)

![Imgur](images/jrUBAF7.png)

### Concevez un crawler web

[Voir exercice et solution](solutions/system_design/web_crawler/README-fr.md)

![Imgur](images/bWxPtQA.png)

### Concevez Mint.com

[Voir exercice et solution](solutions/system_design/mint/README-fr.md)

![Imgur](images/V5q57vU.png)

### Concevez les structures de données pour un réseau social

[Voir exercice et solution](solutions/system_design/social_graph/README-fr.md)

![Imgur](images/cdCv5g7.png)

### Concevez un magasin clé-valeur pour un moteur de recherche

[Voir exercice et solution](solutions/system_design/query_cache/README-fr.md)

![Imgur](images/4j99mhe.png)

### Concevez la fonctionnalité de classement par catégorie des ventes d'Amazon

[Voir exercice et solution](solutions/system_design/sales_rank/README-fr.md)

![Imgur](images/MzExP06.png)

### Concevez un système pouvant évoluer pour atteindre des millions d'utilisateurs sur AWS

[Voir exercice et solution](solutions/system_design/scaling_aws/README-fr.md)

![Imgur](images/jj3A5N8.png)

## Questions d'entretien en conception orientée objet avec solutions

> Questions d'entretien courantes en conception orientée objet avec des discussions, du code et des diagrammes
> d'exemple.
>
> Les solutions sont liées au contenu du dossier `solutions/`.

> **Remarque : Cette section est en cours de développement**

| Question                                                     |                                                                                |
|--------------------------------------------------------------|--------------------------------------------------------------------------------|
| Concevez une table de hachage (hash map)                     | [Solution](solutions/object_oriented_design/hash_table/hash_map.ipynb)         |
| Concevez un cache à utilisation récente minimale (LRU cache) | [Solution](solutions/object_oriented_design/lru_cache/lru_cache.ipynb)         |
| Concevez un centre d'appels (call center)                    | [Solution](solutions/object_oriented_design/call_center/call_center.ipynb)     |
| Concevez un jeu de cartes                                    | [Solution](solutions/object_oriented_design/deck_of_cards/deck_of_cards.ipynb) |
| Concevez un parking                                          | [Solution](solutions/object_oriented_design/parking_lot/parking_lot.ipynb)     |
| Concevez un serveur de chat                                  | [Solution](solutions/object_oriented_design/online_chat/online_chat.ipynb)     |
| Concevez un tableau circulaire                               | [Contribuer](#contributing)                                                    |
| Ajouter une question en conception orientée objet            | [Contribuer](#contributing)                                                    |

## Sujets de conception de système : commencez ici

Nouveau en conception de systèmes ?

Tout d'abord, vous aurez besoin d'une compréhension de base des principes courants, en apprenant ce qu'ils sont, comment
ils sont utilisés, ainsi que leurs avantages et inconvénients.

### Étape 1 : Regardez la vidéo sur l'évolutivité

[Conférence sur l'évolutivité à Harvard](https://www.youtube.com/watch?v=-W9F__D3oY4)

* Sujets abordés :
    * Mise à l'échelle verticale
    * Mise à l'échelle horizontale
    * Mise en cache
    * Répartition de charge
    * Réplication de base de données
    * Partitionnement de base de données

### Étape 2 : Lisez l'article sur l'évolutivité

[Évolutivité](https://web.archive.org/web/20221030091841/http://www.lecloud.net/tagged/scalability/chrono)

* Sujets abordés :
    * [Clones](https://web.archive.org/web/20220530193911/https://www.lecloud.net/post/7295452622/scalability-for-dummies-part-1-clones)
    * [Bases de données](https://web.archive.org/web/20220602114024/https://www.lecloud.net/post/7994751381/scalability-for-dummies-part-2-database)
    * [Caches](https://web.archive.org/web/20230126233752/https://www.lecloud.net/post/9246290032/scalability-for-dummies-part-3-cache)
    * [Asynchronisme](https://web.archive.org/web/20220926171507/https://www.lecloud.net/post/9699762917/scalability-for-dummies-part-4-asynchronism)

### Prochaines étapes

Ensuite, nous examinerons les compromis à haut niveau suivants :

* **Performance** vs **évolutivité**
* **Latence** vs **débit**
* **Disponibilité** vs **cohérence**

Gardez à l'esprit que **tout est une question de compromis**.

Ensuite, nous plongerons dans des sujets plus spécifiques tels que le DNS, les CDNs et les répartiteurs de charge.

## Performance vs Scalabilité

Un service est **scalable** (évolutif) s'il permet une augmentation des **performances** de manière proportionnelle aux
ressources ajoutées. En général, augmenter les performances signifie traiter davantage d'unités de travail, mais cela
peut également consister à traiter des unités de travail plus importantes, par exemple lorsque les ensembles de données
augmentent.<sup><a href=http://www.allthingsdistributed.com/2006/03/a_word_on_scalability.html>1</a></sup>

Une autre façon de comparer performances et scalabilité :

* Si vous avez un problème de **performance**, votre système est lent pour un seul utilisateur.
* Si vous avez un problème de **scalabilité**, votre système est rapide pour un seul utilisateur, mais lent sous une
  charge importante.

### Sources et lectures complémentaires

* [Un mot sur la scalabilité](http://www.allthingsdistributed.com/2006/03/a_word_on_scalability.html)
* [Scalabilité, disponibilité, stabilité, modèles](http://www.slideshare.net/jboner/scalability-availability-stability-patterns/)

## Latence vs Débit

**Latence** : le temps nécessaire pour effectuer une action ou produire un résultat.

**Débit** : le nombre d'actions ou de résultats par unité de temps.

En général, vous devriez viser un **débit maximal** tout en maintenant une **latence acceptable**.

### Sources et lectures complémentaires

* [Comprendre la latence et le débit](https://community.cadence.com/cadence_blogs_8/b/fv/posts/understanding-latency-vs-throughput)

## Disponibilité vs Cohérence

### Théorème CAP

<p align="center">
  <img src="images/bgLMI2u.png">
  <br/>
  <i><a href=http://robertgreiner.com/2014/08/cap-theorem-revisited>Source : Théorème CAP revisité</a></i>
</p>

Dans un système informatique distribué, vous ne pouvez garantir que deux des trois propriétés suivantes :

* **Cohérence** - Chaque lecture reçoit la dernière écriture ou une erreur.
* **Disponibilité** - Chaque requête reçoit une réponse, sans garantie qu'elle contienne la version la plus récente de
  l'information.
* **Tolérance aux partitions** - Le système continue de fonctionner malgré une partition du réseau en raison de pannes.

*Les réseaux ne sont pas fiables, donc vous devez supporter la tolérance aux partitions. Vous devrez ainsi faire un
compromis logiciel entre cohérence et disponibilité.*

#### CP - Cohérence et tolérance aux partitions

Attendre une réponse du nœud partitionné peut entraîner un délai d'expiration. CP est un bon choix si les besoins métier
nécessitent des lectures et écritures atomiques.

#### AP - Disponibilité et tolérance aux partitions

Les réponses retournent la version la plus disponible des données sur n'importe quel nœud, qui peut ne pas être la plus
récente. Les écritures peuvent prendre du temps à être propagées lorsque la partition est résolue.

AP est un bon choix si les besoins métier permettent une [cohérence éventuelle](#eventual-consistency) ou si le système
doit continuer à fonctionner malgré des erreurs externes.

### Sources et lectures complémentaires

* [Théorème CAP revisité](http://robertgreiner.com/2014/08/cap-theorem-revisited/)
* [Une introduction simple au théorème CAP](http://ksat.me/a-plain-english-introduction-to-cap-theorem)
* [CAP FAQ](https://github.com/henryr/cap-faq)
* [Le théorème CAP](https://www.youtube.com/watch?v=k-Yaq8AHlFA)

## Modèles de cohérence

Avec plusieurs copies des mêmes données, nous sommes confrontés à des options sur la manière de les synchroniser afin
que les clients aient une vue cohérente des données. Rappelez-vous la définition de la cohérence selon
le [théorème CAP](#cap-theorem) : chaque lecture reçoit la dernière écriture ou une erreur.

### Cohérence faible

Après une écriture, les lectures peuvent ou non la voir. Une approche de « meilleur effort » est adoptée.

Cette approche est utilisée dans des systèmes tels que memcached. La cohérence faible fonctionne bien pour des cas en
temps réel comme la VoIP, les chats vidéo et les jeux multijoueurs en temps réel. Par exemple, si vous perdez la
connexion lors d'un appel téléphonique, lorsque vous la récupérez, vous n'entendez pas ce qui a été dit pendant l'interruption.

### Cohérence éventuelle

Après une écriture, les lectures la verront éventuellement (généralement en millisecondes). Les données sont répliquées
de manière asynchrone.

Cette approche est utilisée dans des systèmes tels que le DNS et les emails. La cohérence éventuelle fonctionne bien
dans les systèmes à haute disponibilité.

### Cohérence forte

Après une écriture, les lectures la verront immédiatement. Les données sont répliquées de manière synchrone.

Cette approche est utilisée dans les systèmes de fichiers et les SGBDR (Systèmes de Gestion de Bases de Données
Relationnelles). La cohérence forte convient aux systèmes ayant besoin de transactions.

### Sources et lectures complémentaires

* [Transactions entre centres de données](http://snarfed.org/transactions_across_datacenters_io.html)

## Modèles de disponibilité

Il existe deux schémas complémentaires pour garantir une haute disponibilité : **failover** et **réplication**.

### Failover (basculement)

#### Actif-passif

Avec un basculement actif-passif, des signaux (heartbeats) sont envoyés entre le serveur actif et le serveur passif en
veille. Si le signal est interrompu, le serveur passif prend l'adresse IP de l'actif et reprend le service.

La durée de l'interruption dépend du fait que le serveur passif fonctionne déjà en veille « active » (hot standby) ou
s'il doit démarrer à partir d'une veille « froide » (cold standby). Seul le serveur actif gère le trafic.

Le basculement actif-passif peut également être nommé basculement maître-esclave.

#### Actif-actif

Dans une configuration actif-actif, les deux serveurs gèrent le trafic, répartissant la charge entre eux.

Si les serveurs sont accessibles publiquement, le DNS doit connaître les IP publiques des deux serveurs. Si les serveurs
sont internes, la logique de l'application doit gérer les deux serveurs.

Le basculement actif-actif peut également être appelé basculement maître-maître.

### Inconvénients : failover

* Le failover nécessite plus de matériel et ajoute de la complexité.
* Il existe un risque de perte de données si le système actif échoue avant que les données récemment écrites puissent
  être répliquées vers le système passif.

### Réplication

#### Maître-esclave et maître-maître

Ce sujet est davantage discuté dans la section [Base de données](#database) :

* [Réplication maître-esclave](#master-slave-replication)
* [Réplication maître-maître](#master-master-replication)

### Disponibilité en chiffres

La disponibilité est souvent quantifiée en pourcentage d'uptime (ou de downtime). La disponibilité est généralement
mesurée par le nombre de 9 — un service avec 99,99 % de disponibilité est décrit comme ayant quatre 9.

#### Disponibilité à 99,9% - trois 9

| Période     | Temps d'interruption acceptable |
|-------------|---------------------------------|
| Par an      | 8h 45min 57s                    |
| Par mois    | 43min 49,7s                     |
| Par semaine | 10min 4,8s                      |
| Par jour    | 1min 26,4s                      |

#### Disponibilité à 99,99% - quatre 9

| Période     | Temps d'interruption acceptable |
|-------------|---------------------------------|
| Par an      | 52min 35,7s                     |
| Par mois    | 4min 23s                        |
| Par semaine | 1min 5s                         |
| Par jour    | 8,6s                            |

#### Disponibilité en parallèle et en séquence

Si un service se compose de plusieurs composants susceptibles de tomber en panne, la disponibilité globale dépend du
fait que les composants soient en séquence ou en parallèle.

###### En séquence

La disponibilité globale diminue lorsque deux composants ayant une disponibilité < 100 % sont en séquence :

```
Availability (Total) = Availability (Foo) * Availability (Bar)
```

Si `Foo` et `Bar` ont chacun une disponibilité de 99,9%, leur disponibilité totale en séquence sera de 99,8%.

###### En parallèle

La disponibilité globale augmente lorsque deux composants ayant une disponibilité < 100 % sont en parallèle   :

```
Availability (Total) = 1 - (1 - Availability (Foo)) * (1 - Availability (Bar))
```

Si `Foo` et `Bar` ont chacun une disponibilité de 99,9%, leur disponibilité totale en parallèle sera de 99,9999%.

## Système de noms de domaine (DNS)

<p align="center">
  <img src="images/IOyLj4i.jpg">
  <br/>
  <i><a href=http://www.slideshare.net/srikrupa5/dns-security-presentation-issa>Source   : Présentation sur la sécurité DNS</a></i>
</p>

Un Système de Noms de Domaine (DNS) traduit un nom de domaine tel que www.example.com en une adresse IP.

Le DNS est hiérarchique, avec quelques serveurs autoritaires au niveau supérieur. Votre routeur ou votre FAI fournit des
informations sur le(s) serveur(s) DNS à contacter lors d'une requête. Les serveurs DNS de niveaux inférieurs mettent en
cache les correspondances, qui peuvent devenir obsolètes en raison des délais de propagation DNS. Les résultats DNS
peuvent également être mis en cache par votre navigateur ou votre système d'exploitation pour une durée déterminée par
le [time to live (TTL)](https://en.wikipedia.org/wiki/Time_to_live).

* **Enregistrement NS (name server)**   : Spécifie les serveurs DNS pour votre domaine/sous-domaine.
* **Enregistrement MX (mail exchange)**   : Spécifie les serveurs de messagerie qui acceptent les messages.
* **Enregistrement A (adresse)**   : Pointe un nom vers une adresse IP.
* **CNAME (canonique)**   : Pointe un nom vers un autre nom ou un `CNAME` (example.com vers www.example.com) ou vers un
  enregistrement `A`.

Des services comme [CloudFlare](https://www.cloudflare.com/dns/) et [Route 53](https://aws.amazon.com/route53/)
proposent des services DNS gérés. Certains services DNS peuvent router le trafic via différentes méthodes   :

* [Round Robin pondéré](https://www.jscape.com/blog/load-balancing-algorithms)   :
    * Empêche le trafic d'atteindre les serveurs en maintenance
    * Répartition entre des clusters de tailles différentes
    * Tests A/B
* [Basé sur la latence](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/routing-policy-latency.html)
* [Basé sur la géolocalisation](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/routing-policy-geo.html)

### Inconvénients   : DNS

* Accéder à un serveur DNS introduit un léger retard, bien que cela soit atténué par la mise en cache évoquée ci-dessus.
* La gestion des serveurs DNS peut s'avérer complexe et est généralement assurée
  par [les gouvernements, fournisseurs d'accès à Internet et grandes entreprises](http://superuser.com/questions/472695/who-controls-the-dns-servers/472729).
* Les services DNS ont récemment été la cible
  d'[attaques DDoS](http://dyn.com/blog/dyn-analysis-summary-of-friday-october-21-attack/), empêchant les utilisateurs
  d'accéder à des sites comme Twitter sans connaître l'(ou les) adresse(s) IP correspondantes.

### Sources et lectures complémentaires

* [Architecture du DNS](https://technet.microsoft.com/en-us/library/dd197427(v=ws.10).aspx)
* [Wikipedia](https://en.wikipedia.org/wiki/Domain_Name_System)
* [Articles sur le DNS](https://support.dnsimple.com/categories/dns/)

## Réseau de diffusion de contenu (CDN)

<p align="center">
  <img src="images/h9TAuGI.jpg">
  <br/>
  <i><a href=https://www.creative-artworks.eu/why-use-a-content-delivery-network-cdn/>Source : Pourquoi utiliser un CDN</a></i>
</p>

Un réseau de diffusion de contenu (CDN) est un réseau mondial distribué de serveurs proxy, servant du contenu depuis des
emplacements plus proches de l'utilisateur. Généralement, des fichiers statiques tels que HTML, CSS, JS, des photos et
des vidéos sont diffusés à partir d'un CDN, bien que certains CDN comme Amazon CloudFront prennent en charge du contenu
dynamique. La résolution DNS du site indique aux clients quel serveur contacter.

Diffuser du contenu via un CDN peut considérablement améliorer les performances de deux manières :

* Les utilisateurs reçoivent du contenu depuis des centres de données proches d'eux.
* Vos serveurs n'ont pas besoin de traiter les requêtes que le CDN satisfait.

### Push CDN

Les push CDN reçoivent du nouveau contenu à chaque fois qu'il y a un changement sur votre serveur. Vous êtes entièrement
responsable de fournir le contenu, en téléchargeant directement vers le CDN et en réécrivant les URL pour pointer vers
le CDN. Vous pouvez configurer la date d'expiration du contenu et quand il est mis à jour. Le contenu est téléchargé
uniquement lorsqu'il est nouveau ou modifié, minimisant ainsi le trafic, mais maximisant l'espace de stockage.

Les sites avec peu de trafic ou du contenu peu souvent mis à jour fonctionnent bien avec les push CDN. Le contenu est
placé une seule fois sur le CDN, au lieu d'être récupéré régulièrement.

### Pull CDN

Les pull CDN récupèrent le nouveau contenu depuis votre serveur lorsque le premier utilisateur en fait la demande. Vous
laissez le contenu sur votre serveur et réécrivez les URL pour pointer vers le CDN. Cela entraîne une réponse plus lente
jusqu'à ce que le contenu soit mis en cache sur le CDN.

Un [time-to-live (TTL)](https://en.wikipedia.org/wiki/Time_to_live) détermine combien de temps le contenu reste en
cache. Les pull CDN minimisent l'espace de stockage nécessaire sur le CDN, mais peuvent créer du trafic redondant si des
fichiers expirent et sont récupérés avant qu'ils ne soient réellement modifiés.

Les sites avec un trafic élevé fonctionnent bien avec les pull CDN, car le trafic est mieux réparti et seuls les
contenus demandés récemment restent sur le CDN.

### Inconvénients des CDN

* Le coût des CDN peut être important en fonction du trafic, bien que cela doive être comparé aux coûts additionnels
  encourus sans utiliser de CDN.
* Le contenu peut devenir obsolète si mis à jour avant l'expiration du TTL.
* Les CDN exigent la modification des URL pour pointer vers le CDN.

### Sources et lectures complémentaires

* [Diffusion de contenu à l'échelle mondiale](https://figshare.com/articles/Globally_distributed_content_delivery/6605972)
* [Les différences entre push et pull CDN](http://www.travelblogadvice.com/technical/the-differences-between-push-and-pull-cdns/)
* [Wikipedia](https://en.wikipedia.org/wiki/Content_delivery_network)

## Répartiteur de charge (Load balancer)

<p align="center">
  <img src="images/h81n9iK.png">
  <br/>
  <i><a href=http://horicky.blogspot.com/2010/10/scalable-system-design-patterns.html>Source : Modèles de conception de systèmes évolutifs</a></i>
</p>

Les répartiteurs de charge distribuent les requêtes entrantes des clients vers les ressources de calcul comme les
serveurs d'application et les bases de données. Dans chaque cas, le répartiteur de charge renvoie la réponse de la
ressource de calcul appropriée au client. Les répartiteurs de charge sont efficaces pour :

* Empêcher les requêtes d'atteindre des serveurs non sains
* Empêcher la surcharge des ressources
* Aider à éliminer un point de défaillance unique

Les répartiteurs de charge peuvent être mis en œuvre avec du matériel (coûteux) ou avec des logiciels comme HAProxy.

Bénéfices additionnels :

* **Terminaison SSL** - Décrypte les requêtes entrantes et crypte les réponses des serveurs pour que les serveurs
  backend n'aient pas à effectuer ces opérations coûteuses
    * Élimine le besoin d'installer des [certificats X.509](https://en.wikipedia.org/wiki/X.509) sur chaque serveur
* **Persistance de session** - Émettre des cookies et router les requêtes spécifiques d'un client vers la même instance
  si les applications web ne conservent pas les sessions

Pour se protéger contre les pannes, il est courant d'installer plusieurs répartiteurs, en
mode [actif-passif](#active-passive) ou [actif-actif](#active-active).

Les répartiteurs de charge peuvent router le trafic selon divers critères, incluant :

* Aléatoire
* Moins chargé
* Session/cookies
* [Round robin ou round robin pondéré](https://www.g33kinfo.com/info/round-robin-vs-weighted-round-robin-lb)
* [Niveau 4](#layer-4-load-balancing)
* [Niveau 7](#layer-7-load-balancing)

### Répartition au niveau 4

Les répartiteurs de charge du niveau 4 examinent les informations au niveau de la [couche transport](#communication)
pour décider comment distribuer les requêtes. Cela implique généralement les adresses IP source, destination et les
ports dans l'en-tête, mais pas le contenu du paquet. Les répartiteurs de niveau 4 transfèrent les paquets réseau vers et
depuis le serveur en amont en effectuant
une [traduction d'adresse réseau (NAT)](https://www.nginx.com/resources/glossary/layer-4-load-balancing/).

### Répartition au niveau 7

Les répartiteurs de charge du niveau 7 examinent la [couche application](#communication) pour décider comment distribuer
les requêtes. Cela peut inclure le contenu des en-têtes, des messages et des cookies. Les répartiteurs de niveau 7
terminent le trafic réseau, lisent les messages, prennent une décision de répartition de charge, puis ouvrent une
connexion au serveur sélectionné. Par exemple, un répartiteur de charge de niveau 7 peut diriger le trafic vidéo vers
des serveurs dédiés aux vidéos tout en envoyant le trafic de facturation sécurisée vers des serveurs renforcés.

Bien que leur flexibilité soit limitée, les répartiteurs de niveau 4 demandent moins de ressources et de temps que ceux
de niveau 7, même si l'impact sur les performances est minime avec le matériel moderne.

### Mise à l'échelle horizontale

Les répartiteurs de charge peuvent également faciliter la mise à l'échelle horizontale, améliorant les performances et
la disponibilité. Élargir l'infrastructure avec des machines standards est plus rentable et offre une meilleure
disponibilité que de surdimensionner un serveur sur du matériel spécialisé, une approche appelée **mise à l'échelle
verticale**. C'est aussi plus facile de recruter pour des technologies standards que pour des systèmes spécialisés.

#### Inconvénients : mise à l'échelle horizontale

* Augmente la complexité et nécessite la duplication de serveurs
    * Les serveurs doivent être sans état : ils ne doivent pas contenir de données spécifiques aux utilisateurs
    * Les sessions peuvent être stockées dans une datastore centralisée comme une [base de données](#database) (SQL,
      NoSQL) ou un [cache persistant](#cache) (Redis, Memcached)
* Les systèmes en aval (caches, bases de données) doivent gérer plus de connexions simultanées lorsque les serveurs en
  amont s'étendent.

### Inconvénients : répartiteur de charge

* Le répartiteur de charge peut devenir un goulot d'étranglement s'il n'est pas correctement configuré ou ne dispose pas
  de ressources suffisantes.
* Ajouter un répartiteur de charge pour éliminer un point de défaillance unique augmente la complexité.
* Un répartiteur de charge unique est un point de défaillance ; configurer plusieurs répartiteurs complique encore
  davantage.

### Sources et lectures complémentaires

* [Architecture NGINX](https://www.nginx.com/blog/inside-nginx-how-we-designed-for-performance-scale/)
* [Guide d'architecture HAProxy](http://www.haproxy.org/download/1.2/doc/architecture.txt)
* [Scalabilité](http://www.lecloud.net/post/7295452622/scalability-for-dummies-part-1-clones)
* [Wikipédia](https://en.wikipedia.org/wiki/Load_balancing_(computing))
* [Répartition de charge au niveau 4](https://www.nginx.com/resources/glossary/layer-4-load-balancing/)
* [Répartition de charge au niveau 7](https://www.nginx.com/resources/glossary/layer-7-load-balancing/)
* [Configuration des écouteurs ELB](http://docs.aws.amazon.com/elasticloadbalancing/latest/classic/elb-listener-config.html)

## Proxy inverse (serveur Web)

<p align="center">
  <img src="images/n41Azff.png">
  <br/>
  <i><a href=https://upload.wikimedia.org/wikipedia/commons/6/67/Reverse_proxy_h2g2bob.svg>Source : Wikipédia</a></i>
  <br/>
</p>

Un proxy inverse est un serveur web qui centralise les services internes et fournit des interfaces unifiées au public.
Les requêtes des clients sont transmises au serveur capable de les traiter avant que le proxy inverse ne retourne la
réponse du serveur au client.

Les avantages supplémentaires incluent :

* **Sécurité accrue** - Cache les informations sur les serveurs backend, bloque les IP indésirables, limite le nombre de
  connexions par client.
* **Évolutivité et flexibilité accrues** - Les clients voient uniquement l'IP du proxy inverse, ce qui vous permet de
  faire évoluer les serveurs ou de modifier leur configuration.
* **Terminaison SSL** - Décrypte les requêtes entrantes et crypte les réponses des serveurs backend, évitant à ces
  derniers de réaliser ces opérations coûteuses.
    * Supprime la nécessité d'installer des [certificats X.509](https://en.wikipedia.org/wiki/X.509) sur chaque serveur.
* **Compression** - Compresse les réponses des serveurs.
* **Mise en cache** - Renvoie la réponse des requêtes mises en cache.
* **Contenu statique** - Sert directement le contenu statique, comme :
    * HTML/CSS/JS
    * Photos
    * Vidéos
    * Etc.

### Répartiteur de charge vs proxy inverse

* Déployer un répartiteur de charge est utile lorsque vous disposez de plusieurs serveurs. Souvent, les répartiteurs de
  charge routent le trafic vers un ensemble de serveurs ayant les mêmes fonctions.
* Les proxys inverses peuvent être utiles même avec un seul serveur web ou une application, en offrant les avantages
  mentionnés ci-dessus.
* Des solutions comme NGINX et HAProxy peuvent gérer à la fois le proxy inverse de niveau 7 et le répartition de charge.

### Inconvénients : proxy inverse

* L'introduction d'un proxy inverse augmente la complexité.
* Un proxy inverse unique est un point de défaillance ; configurer plusieurs proxys inverses (par ex. via
  un [basculement](https://en.wikipedia.org/wiki/Failover)) augmente aussi la complexité.

### Sources et lectures complémentaires

* [Proxy inverse vs répartiteur de charge](https://www.nginx.com/resources/glossary/reverse-proxy-vs-load-balancer/)
* [Architecture NGINX](https://www.nginx.com/blog/inside-nginx-how-we-designed-for-performance-scale/)
* [Guide architecture HAProxy](http://www.haproxy.org/download/1.2/doc/architecture.txt)
* [Wikipédia](https://en.wikipedia.org/wiki/Reverse_proxy)

## Couche d'application

<p align="center">
  <img src="images/yB5SYwm.png">
  <br/>
  <i><a href=http://lethain.com/introduction-to-architecting-systems-for-scale/#platform_layer>Source : Introduction à l'architecture des systèmes à l'échelle</a></i>
</p>

L'isolation de la couche Web par rapport à la couche d'application (également appelée la couche plateforme) permet de
dimensionner et de configurer ces deux couches indépendamment. Ajouter une nouvelle API se traduit par l'ajout de
serveurs applicatifs sans nécessairement augmenter le nombre de serveurs web. Le **principe de responsabilité unique**
préconise de petits services autonomes qui fonctionnent ensemble. De petites équipes avec de petits services peuvent
planifier de manière plus agressive pour une croissance rapide.

Les tâches asynchrones au sein de la couche application permettent également de bénéficier
de [l'asynchronisme](#asynchronism).

### Microservices

En lien avec cette discussion, [les microservices](https://en.wikipedia.org/wiki/Microservices) peuvent être décrits
comme un ensemble de services petits, modulaires et déployables indépendamment. Chaque service exécute un processus
unique et communique via un mécanisme léger et bien défini pour atteindre un objectif
métier. <sup><a href=https://smartbear.com/learn/api-design/what-are-microservices>1</a></sup>

Par exemple, Pinterest pourrait avoir les microservices suivants : profil utilisateur, abonnés, flux, recherche,
téléchargement de photo, etc.

### Découverte de services

Des systèmes tels que [Consul](https://www.consul.io/docs/index.html), [Etcd](https://coreos.com/etcd/docs/latest)
et [Zookeeper](http://www.slideshare.net/sauravhaloi/introduction-to-apache-zookeeper) peuvent aider les services à se
localiser mutuellement en tenant à jour les noms enregistrés, les adresses et les ports.
Les [vérifications de l'état](https://www.consul.io/intro/getting-started/checks.html) permettent de vérifier
l'intégrité des services, souvent via une requête [HTTP](#hypertext-transfer-protocol-http). Consul et Etcd intègrent
un [magasin clé-valeur](#key-value-store) utile pour stocker les valeurs de configuration et d'autres données partagées.

### Inconvénients : couche application

* Ajouter une couche application avec des services faiblement couplés exige une approche différente en termes
  d'architecture, d'opérations et de processus (par rapport à un système monolithique).
* Les microservices peuvent ajouter de la complexité lors des déploiements et des opérations.

### Sources et lectures complémentaires

* [Introduction à l'architecture des systèmes évolutifs](http://lethain.com/introduction-to-architecting-systems-for-scale)
* [Crack the system design interview](http://www.puncsky.com/blog/2016-02-13-crack-the-system-design-interview)
* [Architecture orientée service](https://en.wikipedia.org/wiki/Service-oriented_architecture)
* [Introduction à Zookeeper](http://www.slideshare.net/sauravhaloi/introduction-to-apache-zookeeper)
* [Tout ce que vous devez savoir sur la création de microservices](https://cloudncode.wordpress.com/2016/07/22/msa-getting-started/)

## Base de données

<p align="center">
  <img src="images/Xkm5CXz.png">
  <br/>
  <i><a href=https://www.youtube.com/watch?v=kKjm4ehYiMs>Source : Scaling up to your first 10 million users</a></i>
</p>

### Système de gestion de bases de données relationnelles (SGBDR)

Une base de données relationnelle comme SQL est une collection de données organisées en tables.

**ACID** est un ensemble de propriétés des [transactions](https://en.wikipedia.org/wiki/Database_transaction) d'une base
de données relationnelle.

* **Atomicité** - Chaque transaction est soit totalement exécutée, soit non exécutée.
* **Cohérence** - Toute transaction amène la base de données d'un état valide à un autre.
* **Isolation** - L'exécution de transactions en parallèle donne les mêmes résultats que leur exécution en série.
* **Durabilité** - Une fois qu'une transaction est validée, elle reste ainsi.

De nombreuses techniques permettent de faire évoluer une base relationnelle : **réplication maître-esclave**, *
*réplication maître-maître**, **fédération**, **sharding**, **dénormalisation**, et **optimisation SQL**.

#### Réplication maître-esclave

Le maître gère les lectures et écritures, répliquant les écritures vers un ou plusieurs esclaves, qui ne servent que les
lectures. Les esclaves peuvent également répliquer à d'autres esclaves de manière arborescente. Si le maître devient
hors ligne, le système peut continuer à fonctionner en mode lecture seule jusqu'à ce qu'un esclave soit promu maître ou
qu'un nouveau maître soit configuré.

<p align="center">
  <img src="images/C9ioGtn.png">
  <br/>
  <i><a href=http://www.slideshare.net/jboner/scalability-availability-stability-patterns/>Source : Scalability, availability, stability, patterns</a></i>
</p>

##### Inconvénients : réplication maître-esclave

* Une logique supplémentaire est nécessaire pour promouvoir un esclave en maître.
* Voir [Inconvénients : réplication](#inconvenients-replication) pour les points liés à **la réplication maître-esclave
  et maître-maître**.

#### Réplication maître-maître

Deux maîtres gèrent les lectures et les écritures, se coordonnant mutuellement sur les écritures. Si l'un des maîtres
tombe en panne, le système peut continuer à fonctionner pour les lectures et les écritures.

<p align="center">
  <img src="images/krAHLGg.png">
  <br/>
  <i><a href=http://www.slideshare.net/jboner/scalability-availability-stability-patterns/>Source : Scalability, availability, stability, patterns</a></i>
</p>

##### Inconvénients : réplication maître-maître

* Un répartiteur de charge ou des modifications dans la logique de l'application sont nécessaires pour déterminer où
  écrire.
* La plupart des systèmes maître-maître sont soit faiblement cohérents (violant ACID), soit augmentent la latence
  d'écriture en raison de la synchronisation.
* La résolution de conflits devient plus importante à mesure que davantage de nœuds d'écriture sont ajoutés ou que la
  latence augmente.
* Voir [Inconvénients : réplication](#inconvenients-replication) pour les points liés à **la réplication maître-esclave
  et maître-maître**.

##### Inconvénients : réplication

* Risque potentiel de perte de données si le maître échoue avant que les données nouvellement écrites ne soient
  répliquées.
* Les écritures sont rejouées sur les répliques de lecture. S'il y a beaucoup d'écritures, les répliques peuvent être
  surchargées en les rejouant, réduisant le nombre de lectures possibles.
* Plus il y a d'esclaves pour la lecture, plus la réplication est importante, exacerbant les retards de réplication.
* Sur certains systèmes, l'écriture sur le maître peut engendrer des threads multiples en parallèle, alors que les
  répliques ne supportent que des écritures séquentielles avec un thread unique.
* La réplication nécessite plus de matériel et ajoute de la complexité.

##### Sources et lectures complémentaires : réplication

* [Scalability, availability, stability, patterns](http://www.slideshare.net/jboner/scalability-availability-stability-patterns/)
* [Multi-master replication](https://en.wikipedia.org/wiki/Multi-master_replication)

#### Fédération

<p align="center">
  <img src="images/U3qV33e.png">
  <br/>
  <i><a href=https://www.youtube.com/watch?v=kKjm4ehYiMs>Source : Scaling up to your first 10 million users</a></i>
</p>

La fédération (ou partitionnement fonctionnel) segmente les bases de données par fonction. Par exemple, à la place d'une
base de données monolithique unique, vous pouvez avoir trois bases de données: **forums**, **utilisateurs**, et *
*produits**, ce qui réduit le trafic de lecture et d'écriture sur chaque base et donc les retards de réplication. Des
bases de données plus petites permettent à plus de données de tenir en mémoire, ce qui augmente les taux de cache grâce
à une meilleure localité. Avec aucun maître central unique pour sérialiser les écritures, vous pouvez écrire en
parallèle, augmentant le débit.

##### Inconvénients : fédération

* La fédération est inefficace si votre schéma nécessite de grandes fonctions ou tables.
* Vous devrez modifier la logique de votre application pour déterminer quelle base de données lire et écrire.
* Effectuer des jointures entre deux bases de données devient plus complexe avec
  un [lien entre serveurs](http://stackoverflow.com/questions/5145637/querying-data-by-joining-two-tables-in-two-database-on-different-servers).
* La fédération nécessite davantage de matériel et accroît la complexité.

##### Sources et lectures complémentaires : fédération

* [Scaling up to your first 10 million users](https://www.youtube.com/watch?v=kKjm4ehYiMs)

#### Sharding (fragmentation)

<p align="center">
  <img src="images/wU8x5Id.png">
  <br/>
  <i><a href=http://www.slideshare.net/jboner/scalability-availability-stability-patterns/>Source : Scalability, availability, stability, patterns</a></i>
</p>

La fragmentation distribue les données entre plusieurs bases de données de sorte que chacune gère uniquement un
sous-ensemble des données. Prenons une base de données utilisateurs : à mesure que leur nombre augmente, davantage de
fragments sont ajoutés au cluster.

Similaire aux avantages de la [fédération](#federation), la fragmentation entraîne moins de trafic en lecture et
écriture, moins de réplications et plus de cache hits. La taille des index est également réduite, ce qui améliore
généralement les performances avec des requêtes plus rapides. Si un fragment tombe en panne, les autres continuent de
fonctionner, bien qu'il soit recommandé d'ajouter une forme de réplication pour éviter une perte de données. Tout comme
la fédération, il n'y a pas de maître central unique, ce qui permet des écritures parallèles avec un débit accru.

Parmi les approches courantes pour fragmenter une table d'utilisateurs, on peut citer l'initiale du nom de famille ou la
localisation géographique.

##### Inconvénients : fragmentation

* Vous devrez mettre à jour la logique de votre application pour travailler avec des fragments, ce qui pourrait
  entraîner des requêtes SQL complexes.
* La répartition des données peut devenir déséquilibrée dans un fragment. Par exemple, un groupe d'utilisateurs très
  actifs dans un fragment pourrait entraîner une charge accrue sur celui-ci par rapport aux autres.
    * L'équilibrage de charge ajoute de la complexité. Une fonction de fragmentation basée sur
      le [hachage cohérent](http://www.paperplanes.de/2011/12/9/the-magic-of-consistent-hashing.html) peut réduire la
      quantité de données transférées.
* Effectuer des jointures entre plusieurs fragments est plus complexe.
* La fragmentation nécessite plus de matériel et accroît la complexité.

##### Sources et lectures complémentaires : fragmentation

* [The coming of the shard](http://highscalability.com/blog/2009/8/6/an-unorthodox-approach-to-database-design-the-coming-of-the.html)
* [Architecture des bases fragmentées](https://en.wikipedia.org/wiki/Shard_(database_architecture))
* [Hachage cohérent](http://www.paperplanes.de/2011/12/9/the-magic-of-consistent-hashing.html)

#### Dénormalisation

La dénormalisation vise à améliorer les performances de lecture au détriment des performances d'écriture. Des copies
redondantes des données sont écrites dans plusieurs tables pour éviter des jointures coûteuses. Certains SGBDR tels
que [PostgreSQL](https://en.wikipedia.org/wiki/PostgreSQL) et Oracle prennent en charge
des [vues matérialisées](https://en.wikipedia.org/wiki/Materialized_view) qui gèrent le stockage des informations
redondantes et la cohérence des copies redondantes.

Une fois les données distribuées par des techniques telles que la [fédération](#federation) ou
la [fragmentation](#sharding), la gestion des jointures entre centres de données augmente encore la complexité. La
dénormalisation peut contourner le besoin de telles jointures complexes.

Dans la plupart des systèmes, les lectures surpassent largement les écritures, parfois par des proportions de 100:1 ou
même 1000:1. Une lecture avec une jointure complexe de bases peut être très coûteuse, nécessitant beaucoup d'opérations
sur disque.

##### Inconvénients : dénormalisation

* Les données sont dupliquées.
* Les contraintes nécessaires pour garantir la cohérence des copies redondantes augmentent la complexité de la
  conception de la base de données.
* Une base dénormalisée soumise à une forte charge d'écriture peut avoir de moins bonnes performances qu'une base
  normalisée.

###### Sources et lectures complémentaires : dénormalisation

* [Dénormalisation](https://en.wikipedia.org/wiki/Denormalization)

#### Optimisation SQL

L'optimisation SQL est un sujet vaste et de
nombreux [livres](https://www.amazon.com/s/ref=nb_sb_noss_2?url=search-alias%3Daps&field-keywords=sql+tuning) y font
référence.

Il est essentiel de **faire des benchmarks** et de **profiler** pour simuler et identifier les goulots d'étranglement.

* **Benchmark** - Simulez des charges importantes avec des outils
  comme [ab](http://httpd.apache.org/docs/2.2/programs/ab.html).
* **Profiler** - Utilisez des outils comme
  le [slow query log](http://dev.mysql.com/doc/refman/5.7/en/slow-query-log.html) pour détecter des problèmes de
  performance.

Les benchmarks et le profiling peuvent suggérer les optimisations suivantes.

##### Optimisation du schéma

* MySQL enregistre sur le disque en blocs contigus pour des accès rapides.
* Utilisez `CHAR` au lieu de `VARCHAR` pour les champs à longueur fixe.
    * `CHAR` permet un accès rapide et aléatoire, tandis qu'avec `VARCHAR`, vous devez trouver la fin d'une chaîne avant
      de passer à la suivante.
* Utilisez `TEXT` pour de grands blocs de texte comme des articles de blog. `TEXT` permet également des recherches
  booléennes.
* Utilisez `INT` pour des nombres jusqu'à 2^32 ou 4 milliards.
* Utilisez `DECIMAL` pour les valeurs monétaires afin d'éviter les erreurs de représentation des nombres à virgule
  flottante.
* Évitez de stocker de grands `BLOBS`. Indiquez plutôt leur emplacement.
* `VARCHAR(255)` représente la limite maximale que peut compter un octet dans un champ à 8 bits sur certains SGBDR.
* Ajoutez la contrainte `NOT NULL` si possible
  pour [améliorer les performances de recherche](http://stackoverflow.com/questions/1017239/how-do-null-values-affect-performance-in-a-database-search).

##### Utilisez des index efficaces

* Les colonnes interrogées (`SELECT`, `GROUP BY`, `ORDER BY`, `JOIN`) peuvent être optimisées grâce à des index.
* Un index est souvent représenté comme un [B-tree auto-équilibré](https://en.wikipedia.org/wiki/B-tree) qui conserve
  les données triées tout en permettant des insertions, suppressions et recherches en temps logarithmique.
* Ajouter un index peut nécessiter plus d'espace en mémoire pour conserver les données triées.
* Les écritures peuvent être plus lentes, car l'index doit également être mis à jour.
* Lors de chargements massifs de données, il peut être plus efficace de désactiver les index, charger les données, puis
  de les reconstruire.

##### Évitez les jointures coûteuses

* [Dénormalisez](#denormalization) si nécessaire pour améliorer les performances.

##### Partitionnez les tables

* Scindez une table en déplaçant les points chauds dans une table séparée pour maintenir les données en mémoire.

##### Optimisez le cache des requêtes

* Dans certains cas, le [cache des requêtes](https://dev.mysql.com/doc/refman/5.7/en/query-cache.html) peut entraîner
  des [problèmes de performance](https://www.percona.com/blog/2016/10/12/mysql-5-7-performance-tuning-immediately-after-installation/).

##### Sources et lectures complémentaires : optimisation SQL

* [Conseils pour optimiser les requêtes MySQL](http://aiddroid.com/10-tips-optimizing-mysql-queries-dont-suck/)
* [Explication sur VARCHAR(255)](http://stackoverflow.com/questions/1217466/is-there-a-good-reason-i-see-varchar255-used-so-often-as-opposed-to-another-l)
* [Impact des valeurs NULL](http://stackoverflow.com/questions/1017239/how-do-null-values-affect-performance-in-a-database-search)
* [Slow query log](http://dev.mysql.com/doc/refman/5.7/en/slow-query-log.html)

### NoSQL

NoSQL est un ensemble d'éléments de données représentés dans un **magasin clé-valeur**, un **magasin de documents**, un
**magasin par colonnes larges** ou une **base de données orientée graphe**. Les données sont dénormalisées et les
jointures sont généralement effectuées au niveau du code de l'application. La plupart des magasins NoSQL ne respectent
pas complètement les transactions ACID et privilégient la [consistance éventuelle](#eventual-consistency).

**BASE** est souvent utilisé pour décrire les propriétés des bases de données NoSQL. Contrairement
au [théorème CAP](#cap-theorem), BASE privilégie la disponibilité à la consistance.

* **Basically available (disponibilité essentielle)** - Le système garantit la disponibilité.
* **Soft state (état malléable)** - L'état du système peut évoluer au fil du temps, même sans entrée.
* **Eventual consistency (consistance éventuelle)** - Le système deviendra cohérent après une période de temps, tant
  qu'il ne reçoit pas de nouvelles entrées pendant cette période.

En plus de choisir entre [SQL ou NoSQL](#sql-or-nosql), il est utile de comprendre quel type de base de données NoSQL
convient le mieux à vos cas d'utilisation. Nous examinerons les **magasins clé-valeur**, les **magasins de documents**,
les **magasins par colonnes larges**, et les **bases de données orientées graphe** dans la section suivante.

#### Magasin clé-valeur

> Abstraction : table de hachage

Un magasin clé-valeur permet généralement des opérations de lecture et d'écriture en O(1) et est souvent basé sur de la
mémoire ou des disques SSD. Ces magasins peuvent maintenir les clés dans
un [ordre lexicographique](https://en.wikipedia.org/wiki/Lexicographical_order), ce qui permet une récupération efficace
de plages de clés. Les magasins clé-valeur peuvent également stocker des métadonnées avec une valeur.

Les magasins clé-valeur offrent de hautes performances et sont souvent utilisés pour des modèles de données simples ou
pour des données changeant rapidement, comme une couche de cache en mémoire. Étant donné qu'ils n'offrent qu'un ensemble
limité d'opérations, la complexité est reportée sur la couche applicative si des opérations supplémentaires sont
nécessaires.

Un magasin clé-valeur constitue la base de systèmes plus complexes tels qu'un magasin de documents, et dans certains
cas, une base de données graphe.

##### Sources et lectures complémentaires : magasin clé-valeur

* [Base de données clé-valeur](https://en.wikipedia.org/wiki/Key-value_database)
* [Inconvénients des magasins clé-valeur](http://stackoverflow.com/questions/4056093/what-are-the-disadvantages-of-using-a-key-value-table-over-nullable-columns-or)
* [Architecture de Redis](http://qnimate.com/overview-of-redis-architecture/)
* [Architecture de Memcached](https://adayinthelifeof.nl/2011/02/06/memcache-internals/)

#### Magasin de documents

> Abstraction : magasin clé-valeur avec des documents stockés sous forme de valeurs

Un magasin de documents est basé sur des documents (XML, JSON, binaire, etc.), où un document contient toutes les
informations pour un objet donné. Les magasins de documents proposent des API ou un langage de requête pour interroger
la structure interne des documents. *Notez que de nombreux magasins clé-valeur incluent des fonctionnalités pour
travailler avec les métadonnées d'une valeur, brouillant les lignes entre ces deux types de stockage.*

Selon l'implémentation sous-jacente, les documents sont organisés par collections, balises, métadonnées ou répertoires.
Bien que les documents puissent être organisés ou regroupés, ils peuvent contenir des champs complètement différents les
uns des autres.

Certains magasins de documents tels que [MongoDB](https://www.mongodb.com/mongodb-architecture)
et [CouchDB](https://blog.couchdb.org/2016/08/01/couchdb-2-0-architecture/) offrent un langage similaire au SQL pour
effectuer des requêtes
complexes. [DynamoDB](http://www.read.seas.harvard.edu/~kohler/class/cs239-w08/decandia07dynamo.pdf) prend en charge à
la fois les clés-valeurs et les documents.

Les magasins de documents offrent une grande flexibilité et sont souvent utilisés pour des données qui changent
occasionnellement.

##### Sources et lectures complémentaires : magasin de documents

* [Base de données orientée documents](https://en.wikipedia.org/wiki/Document-oriented_database)
* [Architecture de MongoDB](https://www.mongodb.com/mongodb-architecture)
* [Architecture de CouchDB](https://blog.couchdb.org/2016/08/01/couchdb-2-0-architecture/)
* [Architecture d'Elasticsearch](https://www.elastic.co/blog/found-elasticsearch-from-the-bottom-up)

#### Magasin par colonnes larges

<p align="center">
  <img src="images/n16iOGk.png">
  <br/>
  <i><a href=http://blog.grio.com/2015/11/sql-nosql-a-brief-history.html>Source : SQL & NoSQL, a brief history</a></i>
</p>

> Abstraction : map imbriquée `ColumnFamily<RowKey, Columns<ColKey, Value, Timestamp>>`

L'unité de base d'un magasin par colonnes larges est une colonne (paire nom/valeur). Une colonne peut être regroupée
dans des familles de colonnes (analogue à une table SQL). Les super-familles de colonnes regroupent davantage ces
familles. On peut accéder à chaque colonne indépendamment à l'aide d'une clé de ligne, et les colonnes ayant la même clé
de ligne forment une ligne. Chaque valeur contient un horodatage pour la gestion des versions et la résolution des
conflits.

Google a introduit [Bigtable](http://www.read.seas.harvard.edu/~kohler/class/cs239-w08/chang06bigtable.pdf) comme
premier magasin par colonnes larges, influençant le projet
open-source [HBase](https://www.edureka.co/blog/hbase-architecture/) souvent utilisé dans l'écosystème Hadoop
et [Cassandra](http://docs.datastax.com/en/cassandra/3.0/cassandra/architecture/archIntro.html) de Facebook. Des
magasins comme Bigtable, HBase et Cassandra maintiennent les clés dans un ordre lexicographique, permettant une
récupération efficace de plages de clés spécifiques.

Les magasins par colonnes larges offrent une haute disponibilité et une grande évolutivité. Ils sont souvent utilisés
pour des ensembles de données très volumineux.

##### Sources et lectures complémentaires : magasin par colonnes larges

* [SQL & NoSQL, a brief history](http://blog.grio.com/2015/11/sql-nosql-a-brief-history.html)
* [Architecture de Bigtable](http://www.read.seas.harvard.edu/~kohler/class/cs239-w08/chang06bigtable.pdf)
* [Architecture de HBase](https://www.edureka.co/blog/hbase-architecture/)
* [Architecture de Cassandra](http://docs.datastax.com/en/cassandra/3.0/cassandra/architecture/archIntro.html)

#### Base orientée graphe

<p align="center">
  <img src="images/fNcl65g.png">
  <br/>
  <i><a href=https://en.wikipedia.org/wiki/File:GraphDatabase_PropertyGraph.png>Source : Graph database</a></i>
</p>

> Abstraction: graphe

Dans une base de données orientée graphe, chaque nœud représente un enregistrement et chaque arc une relation entre deux
nœuds. Les bases orientées graphe sont optimisées pour représenter des relations complexes avec de nombreuses clés
étrangères ou des relations plusieurs-à-plusieurs.

Les bases orientées graphe offrent de hautes performances pour des modèles de données complexes, comme ceux d'un réseau
social. Elles sont relativement récentes et ne sont pas encore largement utilisées, ce qui peut rendre plus difficile la
recherche d'outils de développement et de ressources. De nombreux graphes ne peuvent être accessibles que via
des [APIs REST](#representational-state-transfer-rest).

##### Sources et lectures complémentaires : graphe

* [Base de données orientée graphe](https://en.wikipedia.org/wiki/Graph_database)
* [Neo4j](https://neo4j.com/)
* [FlockDB](https://blog.twitter.com/2010/introducing-flockdb)

#### Sources et lectures complémentaires : NoSQL

* [Explication du concept BASE](http://stackoverflow.com/questions/3342497/explanation-of-base-terminology)
* [Bases de données NoSQL: enquête et guides de décision](https://medium.com/baqend-blog/nosql-databases-a-survey-and-decision-guidance-ea7823a822d#.wskogqenq)
* [Évolutivité](http://www.lecloud.net/post/7994751381/scalability-for-dummies-part-2-database)
* [Introduction à NoSQL](https://www.youtube.com/watch?v=qI_g07C_Q5I)
* [Modèles NoSQL](http://horicky.blogspot.com/2009/11/nosql-patterns.html)

### SQL ou NoSQL

<p align="center">
  <img src="images/wXGqG5f.png">
  <br/>
  <i><a href=https://www.infoq.com/articles/Transition-RDBMS-NoSQL/>Source : Transitioning from RDBMS to NoSQL</a></i>
</p>

Raisons de choisir **SQL**:

* Données structurées
* Schéma rigide
* Données relationnelles
* Besoin de jointures complexes
* Transactions
* Modèles clairs pour la montée en charge
* Plus établi : développeurs, communauté, code, outils, etc.
* Les recherches par index sont très rapides

Raisons de choisir **NoSQL** :

* Données semi-structurées
* Schéma flexible ou dynamique
* Données non relationnelles
* Aucun besoin de jointures complexes
* Stockage de nombreux To (ou Po) de données
* Charges de travail très intensives en données
* Débit très élevé pour les IOPS

Exemples de données bien adaptées à NoSQL :

* Ingestion rapide de données clickstream et journaux
* Données de leaderboard ou de scores
* Données temporaires, comme un panier d'achat
* Tables fréquemment consultées ("chaudes")
* Métadonnées ou tables de recherche

##### Sources et lectures complémentaires : SQL ou NoSQL

* [Scaling up to your first 10 million users](https://www.youtube.com/watch?v=kKjm4ehYiMs)
* [SQL vs NoSQL differences](https://www.sitepoint.com/sql-vs-nosql-differences/)

## Cache

<p align="center">
  <img src="images/Q6z24La.png">
  <br/>
  <i><a href=http://horicky.blogspot.com/2010/10/scalable-system-design-patterns.html>Source : Scalable system design patterns</a></i>
</p>

Le cache améliore les temps de chargement des pages et peut réduire la charge sur vos serveurs et bases de données. Dans
ce modèle, le répartiteur vérifie d'abord si la requête a déjà été effectuée et tente de trouver le résultat précédent à
renvoyer, afin d'éviter l'exécution réelle.

Les bases de données bénéficient souvent d'une répartition uniforme des lectures et des écritures entre leurs
partitions. Les éléments populaires peuvent déséquilibrer cette répartition, causant des goulets d'étranglement. Placer
un cache devant une base de données peut aider à absorber des charges irrégulières et des pics de trafic.

### Cache côté client

Les caches peuvent se trouver côté client (système d'exploitation ou
navigateur), [côté serveur](#reverse-proxy-web-server) ou dans une couche distincte dédiée au cache.

### Cache CDN

Les [CDNs](#content-delivery-network) sont considérés comme un type de cache.

### Cache au niveau du serveur web

Des [proxies inverses](#reverse-proxy-web-server) et des caches comme [Varnish](https://www.varnish-cache.org/) peuvent
servir directement du contenu statique et dynamique. Les serveurs web peuvent également mettre en cache des requêtes,
renvoyant des réponses sans avoir besoin de contacter les serveurs applicatifs.

### Cache de base de données

Votre base de données inclut généralement un certain niveau de cache dans sa configuration par défaut, optimisée pour un
cas d'utilisation générique. Ajuster ces paramètres en fonction des modèles d'utilisation spécifiques peut améliorer
encore les performances.

### Cache d'application

Les caches en mémoire comme Memcached et Redis sont des magasins clé-valeur situés entre votre application et votre
système de stockage de données. Comme les données sont conservées en RAM, elles sont beaucoup plus rapides que dans des
bases classiques où les données résident sur disque. La RAM étant plus limitée que le disque, les algorithmes
d'[invalidation de cache](https://en.wikipedia.org/wiki/Cache_algorithms) comme
le [Least Recently Used (LRU)](https://en.wikipedia.org/wiki/Cache_replacement_policies#Least_recently_used_(LRU))
peuvent aider à invalider les entrées "froides" et garder les données "chaudes" en RAM.

Redis offre les fonctionnalités supplémentaires suivantes :

* Option de persistance
* Structures de données intégrées telles que les ensembles triés et les listes

Il existe plusieurs niveaux possibles de cache, répartis en deux catégories générales : **requêtes de base de données**
et **objets** :

* Au niveau des lignes
* Au niveau des requêtes
* Objets sérialisables entièrement formés
* HTML intégralement rendu

De manière générale, il est recommandé d'éviter le cache basé sur des fichiers, car cela complique le clonage et
l'auto-scalabilité.

### Mise en cache au niveau des requêtes de base de données

À chaque requête exécutée sur la base de données, il est possible de hacher la requête comme clé et d'enregistrer le
résultat dans le cache. Cependant, cette méthode souffre de certaines limitations liées à l'expiration des données :

* Difficile de supprimer un résultat mis en cache lors de requêtes complexes.
* Si un élément de données change (comme une cellule de tableau), toutes les requêtes mises en cache incluant cette
  cellule doivent être invalidées.

### Mise en cache au niveau objet

Traitez vos données comme des objets, similaire à ce que vous faites au niveau du code applicatif. Faites construire par
votre application le jeu de données à partir de la base dans une instance de classe ou une/des structure(s) de données :

* Supprimez l'objet du cache si ses données sous-jacentes ont changé.
* Permet un traitement asynchrone : des workers assemblent des objets en consommant l'objet mis en cache le plus récent.

Suggestions de ce qu'il convient de mettre en cache :

* Sessions utilisateur
* Pages web entièrement rendues
* Flux d'activité
* Données de graphe utilisateur

### Quand mettre à jour le cache

Étant donné que vous ne pouvez stocker qu'une quantité limitée de données dans le cache, vous devrez déterminer quelle
stratégie de mise à jour convient le mieux à votre cas d'utilisation.

#### Cache-aside

<p align="center">
  <img src="images/ONjORqk.png">
  <br/>
  <i><a href=http://www.slideshare.net/tmatyashovsky/from-cache-to-in-memory-data-grid-introduction-to-hazelcast>Source : From cache to in-memory data grid</a></i>
</p>

L'application est responsable de la lecture/écriture depuis le stockage. Le cache n'interagit pas directement avec le
stockage. L'application procède comme suit :

* Cherche une entrée dans le cache, entraînant un cache miss si elle n'existe pas.
* Charge l'entrée depuis la base de données.
* Ajoute l'entrée dans le cache.
* Retourne l'entrée.

```python
def get_user(self, user_id):
    user = cache.get("user.{0}", user_id)
    if user is None:
        user = db.query("SELECT * FROM users WHERE user_id = {0}", user_id)
        if user is not None:
            key = "user.{0}".format(user_id)
            cache.set(key, json.dumps(user))
    return user
```

[Memcached](https://memcached.org/) est généralement utilisé de cette manière.

Les lectures suivantes des données mises en cache sont rapides. Le cache-aside est aussi surnommé lazy loading. Seules
les données demandées sont mises en cache, évitant de remplir le cache avec des données non lues.

##### Inconvénients : cache-aside

* Chaque cache miss entraîne trois opérations, ce qui peut causer un retard notable.
* Les données peuvent devenir obsolètes si elles sont modifiées dans la base. Ce problème peut être atténué en
  définissant un délai d'expiration (TTL) ou en utilisant le write-through.
* En cas de défaillance d'un nœud, un nouveau nœud vide est ajouté, augmentant la latence.

#### Write-through (écriture directe)

<p align="center">
  <img src="images/0vBc0hN.png">
  <br/>
  <i><a href=http://www.slideshare.net/jboner/scalability-availability-stability-patterns/>Source : Scalability, availability, stability, patterns</a></i>
</p>

L'application utilise le cache comme principale source de données, en y lisant et écrivant. Le cache est alors
responsable des opérations de lecture et d'écriture dans la base de données :

* L'application ajoute/actualise une entrée dans le cache.
* Le cache écrit de manière synchrone l'entrée dans le stockage de données.
* Retour de la réponse.

Exemple dans le code applicatif :

```python
set_user(12345, {"foo": "bar"})
```

Cache code:

```python
def set_user(user_id, values):
    user = db.query("UPDATE Users WHERE id = {0}", user_id, values)
    cache.set(user_id, user)
```

Le write-through est une opération globalement plus lente, car elle implique une écriture dans le stockage, mais les
lectures suivantes des données récemment écrites sont rapides. Les utilisateurs tolèrent généralement mieux une latence
lors de l'écriture des données que lors de leur lecture. Les données dans le cache ne sont pas obsolètes.

##### Inconvénients : écriture directe

* Lorsqu'un nouveau nœud est créé (suite à une défaillance ou une montée en charge), ce dernier ne met pas en cache
  d'entrées tant que ces dernières n'ont pas été actualisées dans la base de données. Le cache-aside associé au
  write-through peut limiter ce problème.
* La plupart des données écrites pourraient ne jamais être lues, mais cela peut être minimisé grâce à l'utilisation d'un
  TTL.

#### Write-behind (écriture différée)

<p align="center">
  <img src="images/rgSrvjG.png">
  <br/>
  <i><a href=http://www.slideshare.net/jboner/scalability-availability-stability-patterns/>Source : Scalability, availability, stability, patterns</a></i>
</p>

Avec l'écriture différée, l'application procède comme suit :

* Ajouter/mettre à jour une entrée dans le cache.
* Écriture asynchrone de l'entrée dans le stockage de données, améliorant ainsi les performances en écriture.

##### Inconvénients : écriture différée

* Une perte de données est possible si le cache tombe en panne avant que son contenu ne soit synchronisé avec le
  stockage de données.
* Il est plus complexe de mettre en œuvre le write-behind que le cache-aside ou le write-through.

#### Refresh-ahead (rafraîchissement anticipé)

<p align="center">
  <img src="images/kxtjqgE.png">
  <br/>
  <i><a href=http://www.slideshare.net/tmatyashovsky/from-cache-to-in-memory-data-grid-introduction-to-hazelcast>Source : From cache to in-memory data grid</a></i>
</p>

Vous pouvez configurer le cache pour qu'il rafraîchisse automatiquement toute entrée récemment consultée avant son
expiration.

Le refresh-ahead peut réduire la latence par rapport au read-through si le cache peut prédire avec précision les
éléments susceptibles d'être nécessaires à l'avenir.

##### Inconvénients : rafraîchissement anticipé

* Une prédiction incorrecte des éléments susceptibles d'être nécessaires peut entraîner des performances inférieures à
  celles obtenues sans le refresh-ahead.

### Inconvénients : cache

* Besoin de maintenir la cohérence entre les caches et la source de vérité, comme une base de données, par un
  mécanisme [d'invalidation du cache](https://en.wikipedia.org/wiki/Cache_algorithms).
* L'invalidation du cache est un problème difficile. Une complexité supplémentaire est associée à la décision de quand
  mettre à jour le cache.
* Des modifications au niveau de l'application sont nécessaires, comme l'ajout de Redis ou Memcached.

### Sources et lectures complémentaires

* [From cache to in-memory data grid](http://www.slideshare.net/tmatyashovsky/from-cache-to-in-memory-data-grid-introduction-to-hazelcast)
* [Scalable system design patterns](http://horicky.blogspot.com/2010/10/scalable-system-design-patterns.html)
* [Introduction to architecting systems for scale](http://lethain.com/introduction-to-architecting-systems-for-scale/)
* [Scalability, availability, stability, patterns](http://www.slideshare.net/jboner/scalability-availability-stability-patterns/)
* [Scalability](http://www.lecloud.net/post/9246290032/scalability-for-dummies-part-3-cache)
* [AWS ElastiCache strategies](http://docs.aws.amazon.com/AmazonElastiCache/latest/UserGuide/Strategies.html)
* [Wikipedia](https://en.wikipedia.org/wiki/Cache_(computing))

## Asynchronisme

<p align="center">
  <img src="images/54GYsSx.png">
  <br/>
  <i><a href=http://lethain.com/introduction-to-architecting-systems-for-scale/#platform_layer>Source : Intro to architecting systems for scale</a></i>
</p>

Les workflows asynchrones permettent de réduire les temps de réponse pour des opérations coûteuses qui seraient
autrement effectuées en ligne. Ils peuvent également aider en réalisant à l'avance des travaux chronophages, comme
l'agrégation périodique de données.

### Files de messages

Les files de messages reçoivent, stockent et livrent des messages. Si une opération est trop lente pour être réalisée en
ligne, vous pouvez utiliser une file de messages avec le workflow suivant :

* Une application publie une tâche dans la file, puis informe l'utilisateur de l'état de la tâche.
* Un worker récupère la tâche dans la file, la traite, puis signale que la tâche est terminée.

L'utilisateur n'est pas bloqué et la tâche est traitée en arrière-plan. Pendant ce temps, le client peut éventuellement
effectuer une petite quantité de traitement pour donner l'impression que la tâche est terminée. Par exemple, lors de la
publication d'un tweet, le tweet pourrait être instantanément affiché dans votre timeline, mais il pourrait prendre un
certain temps avant d'être réellement diffusé à l'ensemble de vos abonnés.

**[Redis](https://redis.io/)** est utile en tant que courtier de messages simple, mais les messages peuvent être perdus.

**[RabbitMQ](https://www.rabbitmq.com/)** est populaire, mais nécessite une adaptation au protocole `AMQP` et la gestion
de vos propres nœuds.

**[Amazon SQS](https://aws.amazon.com/sqs/)** est hébergé, mais peut présenter une latence élevée et comporter un risque
de livraison de messages en double.

### Files de tâches

Les files de tâches reçoivent des tâches et leurs données associées, les exécutent, puis renvoient leurs résultats.
Elles peuvent supporter la planification et être utilisées pour exécuter en arrière-plan des tâches nécessitant une
charge computationnelle importante.

**[Celery](https://docs.celeryproject.org/en/stable/)** prend en charge la planification et est principalement
compatible avec Python.

### Back pressure (rétropression)

Lorsque les files commencent à croître de manière significative, leur taille peut dépasser la capacité mémoire,
entraînant des ratés de cache, des lectures disque et des performances encore plus lentes.
La [rétropression (back pressure)](http://mechanical-sympathy.blogspot.com/2012/05/apply-back-pressure-when-overloaded.html)
peut aider en limitant la taille de la file, maintenant ainsi un taux de traitement élevé et des temps de réponse
optimaux pour les tâches déjà en file. Une fois la file remplie, les clients reçoivent un message "serveur occupé" ou un
code de statut HTTP 503 pour réessayer plus tard. Les clients peuvent retenter la requête ultérieurement, peut-être avec
un [backoff exponentiel](https://en.wikipedia.org/wiki/Exponential_backoff).

### Inconvénients : asynchronisme

* Les cas d'utilisation tels que les calculs peu coûteux et les workflows en temps réel pourraient être mieux adaptés
  aux opérations synchrones, car l'introduction de files peut ajouter des retards et de la complexité.

### Sources et lectures complémentaires

* [It's all a number game](https://www.youtube.com/watch?v=1KRYH75wgy4)
* [Applying back pressure when overloaded](http://mechanical-sympathy.blogspot.com/2012/05/apply-back-pressure-when-overloaded.html)
* [Little's law](https://en.wikipedia.org/wiki/Little%27s_law)
* [Quelle est la différence entre une file de messages et une file de tâches ?](https://www.quora.com/What-is-the-difference-between-a-message-queue-and-a-task-queue-Why-would-a-task-queue-require-a-message-broker-like-RabbitMQ-Redis-Celery-or-IronMQ-to-function)

## Communication

<p align="center">
  <img src="images/5KeocQs.jpg">
  <br/>
  <i><a href=http://www.escotal.com/osilayer.html>Source : OSI 7 layer model</a></i>
</p>

### Hypertext Transfer Protocol (HTTP)

HTTP est une méthode pour encoder et transporter des données entre un client et un serveur. C'est un protocole de type
requête/réponse : les clients émettent des requêtes et les serveurs renvoient des réponses contenant du contenu pertinent
et des informations sur l'état d'achèvement de la requête. HTTP est autonome, permettant aux requêtes et réponses de
transiter par plusieurs routeurs et serveurs intermédiaires effectuant de l'équilibrage de charge, de la mise en cache,
du chiffrement et de la compression.

Une requête HTTP de base se compose d'un verbe (méthode) et d'une ressource (point de terminaison). Voici les verbes
HTTP les plus courants :

| Verbe  | Description                                                         | Idempotent* | Sûr | Mise en cache                                            |
|--------|---------------------------------------------------------------------|-------------|-----|----------------------------------------------------------|
| GET    | Lit une ressource                                                   | Oui         | Oui | Oui                                                      |
| POST   | Crée une ressource ou déclenche un processus qui traite des données | Non         | Non | Oui si la réponse contient des informations de fraîcheur |
| PUT    | Crée ou remplace une ressource                                      | Oui         | Non | Non                                                      |
| PATCH  | Met à jour partiellement une ressource                              | Non         | Non | Oui si la réponse contient des informations de fraîcheur |
| DELETE | Supprime une ressource                                              | Oui         | Non | Non                                                      |

*Peut être appelé plusieurs fois sans conséquences différentes.

HTTP est un protocole de couche application reposant sur des protocoles de niveau inférieur tels que **TCP** et **UDP**.

#### Sources et lectures complémentaires : HTTP

* [What is HTTP?](https://www.nginx.com/resources/glossary/http/)
* [Difference between HTTP and TCP](https://www.quora.com/What-is-the-difference-between-HTTP-protocol-and-TCP-protocol)
* [Difference between PUT and PATCH](https://laracasts.com/discuss/channels/general-discussion/whats-the-differences-between-put-and-patch?page=1)

### Transmission Control Protocol (TCP)

<p align="center">
  <img src="images/JdAsdvG.jpg">
  <br/>
  <i><a href=http://www.wildbunny.co.uk/blog/2012/10/09/how-to-make-a-multi-player-game-part-1/>Source : How to make a multiplayer game</a></i>
</p>

TCP est un protocole orienté connexion utilisé sur un [réseau IP](https://en.wikipedia.org/wiki/Internet_Protocol). La
connexion est établie et terminée via un [handshake](https://en.wikipedia.org/wiki/Handshaking). Tous les paquets
envoyés sont garantis d'atteindre leur destination dans l'ordre d'origine et sans corruption grâce à:

* des numéros de séquence et
  des [champs checksum](https://en.wikipedia.org/wiki/Transmission_Control_Protocol#Checksum_computation) pour chaque
  paquet ;
* des paquets d'[accusé de réception](https://en.wikipedia.org/wiki/Acknowledgement_(data_networks)) et des
  retransmissions automatiques.

Si l'expéditeur ne reçoit pas de réponse correcte, il réenvoie les paquets. Après plusieurs expirations, la connexion
est abandonnée. TCP met également en œuvre un [contrôle de flux](https://en.wikipedia.org/wiki/Flow_control_(data)) et
un [contrôle de congestion](https://en.wikipedia.org/wiki/Network_congestion#Congestion_control). Ces garanties
provoquent des délais et entraînent généralement une transmission moins efficace par rapport à UDP.

Pour garantir un débit élevé, les serveurs web peuvent maintenir un grand nombre de connexions TCP ouvertes, ce qui
entraîne une utilisation élevée de la mémoire. Cela peut être coûteux lorsqu'il y a un grand nombre de connexions
ouvertes entre des threads de serveurs web et, par exemple, un serveur [Memcached](https://memcached.org/).
Le [pooling des connexions](https://en.wikipedia.org/wiki/Connection_pool) peut être utilisé, en plus d'un basculement
vers UDP lorsque applicable.

TCP est utile pour les applications nécessitant une fiabilité élevée, mais qui ne sont pas critiques en termes de temps,
comme les serveurs web, les bases de données, les emails (SMTP), les transferts de fichiers (FTP) ou SSH.

Utilisez TCP au lieu d'UDP lorsque:

* Vous avez besoin que toutes les données arrivent intactes.
* Vous souhaitez bénéficier automatiquement du meilleur usage possible du débit réseau.

### User Datagram Protocol (UDP)

<p align="center">
  <img src="images/yzDrJtA.jpg">
  <br/>
  <i><a href=http://www.wildbunny.co.uk/blog/2012/10/09/how-to-make-a-multi-player-game-part-1/>Source : How to make a multiplayer game</a></i>
</p>

UDP est un protocole sans connexion. Les datagrammes (similaires aux paquets) sont garantis uniquement au niveau du
datagramme. Les datagrammes peuvent arriver à destination dans le désordre ou ne pas arriver du tout. UDP ne prend pas
en charge le contrôle de congestion. Sans les garanties apportées par TCP, UDP est généralement plus efficace.

UDP peut effectuer des diffusions (broadcast), envoyant des datagrammes à tous les dispositifs du sous-réseau. Cela est
utile avec le [DHCP](https://en.wikipedia.org/wiki/Dynamic_Host_Configuration_Protocol), car le client n'a pas encore
reçu d'adresse IP, ce qui empêche une transmission en continu via TCP.

UDP est moins fiable, mais fonctionne bien dans des cas d'utilisation en temps réel tels que les appels VoIP, la
visioconférence, le streaming, et les jeux multijoueurs en temps réel.

Utilisez UDP au lieu de TCP lorsque:

* Vous avez besoin de la latence la plus basse possible.
* Des données reçues en retard sont pires que la perte de données.
* Vous souhaitez mettre en œuvre votre propre système de correction d'erreurs.

#### Sources et lectures complémentaires : TCP et UDP

* [Networking for game programming](http://gafferongames.com/networking-for-game-programmers/udp-vs-tcp/)
* [Key differences between TCP and UDP protocols](http://www.cyberciti.biz/faq/key-differences-between-tcp-and-udp-protocols/)
* [Difference between TCP and UDP](http://stackoverflow.com/questions/5970383/difference-between-tcp-and-udp)
* [Transmission control protocol](https://en.wikipedia.org/wiki/Transmission_Control_Protocol)
* [User datagram protocol](https://en.wikipedia.org/wiki/User_Datagram_Protocol)
* [Scaling memcache at Facebook](http://www.cs.bu.edu/~jappavoo/jappavoo.github.com/451/papers/memcache-fb.pdf)

### Appel de procédure distante (RPC)

<p align="center">
  <img src="images/iF4Mkb5.png">
  <br/>
  <i><a href=http://www.puncsky.com/blog/2016-02-13-crack-the-system-design-interview>Source : Crack the system design interview</a></i>
</p>

Avec un RPC, un client provoque l'exécution d'une procédure dans un espace d'adressage différent, généralement sur un
serveur distant. La procédure est codée comme si elle était un appel de procédure locale, en cachant les détails de
communication avec le serveur au programme client. Les appels distants sont habituellement plus lents et moins fiables que
les appels locaux, il est donc utile de distinguer les appels RPC des appels locaux. Les frameworks RPC populaires
incluent [Protobuf](https://developers.google.com/protocol-buffers/), [Thrift](https://thrift.apache.org/)
et [Avro](https://avro.apache.org/docs/current/).

Le RPC est un protocole de type requête-réponse :

* **Programme client** : Appelle la procédure du stub client. Les paramètres sont empilés comme un appel de procédure
  local.
* **Procédure de stub client** : Sérialise (marshal) l'identifiant de la procédure et les arguments dans un message de
  demande.
* **Module de communication client** : L'OS envoie le message du client au serveur.
* **Module de communication serveur** : L'OS transmet les paquets entrants à la procédure stub serveur.
* **Procédure de stub serveur** : Désérialise (unmarshal) les résultats, appelle la procédure serveur correspondante à
  l'identifiant et passe les arguments donnés.
* La réponse du serveur répète les étapes ci-dessus, mais dans l'ordre inverse.

Exemples d'appels RPC:

```
GET /someoperation?data=anId

POST /anotheroperation
{
  "data":"anId";
  "anotherdata": "another value"
}
```

RPC met l'accent sur l'exposition des comportements. Les RPC sont souvent utilisés pour des raisons de performance dans
les communications internes, car vous pouvez concevoir des appels natifs pour mieux répondre à vos cas d'utilisation.

Choisissez une bibliothèque native (aussi appelée SDK) lorsque :

* Vous connaissez votre plateforme cible.
* Vous voulez contrôler la façon dont votre "logique" est accessible.
* Vous voulez contrôler le processus de gestion des erreurs depuis votre bibliothèque.
* La performance et l'expérience utilisateur sont vos principales préoccupations.

Les API HTTP suivant **REST** sont souvent utilisées pour des API publiques.

#### Inconvénients : RPC

* Les clients RPC deviennent fortement couplés à l'implémentation du service.
* Une nouvelle API doit être définie pour chaque nouvelle opération ou cas d'utilisation.
* Il peut être difficile de déboguer un RPC.
* Vous risquez de ne pas pouvoir exploiter facilement les technologies existantes. Par exemple, il peut être nécessaire
  de faire un effort supplémentaire pour s'assurer
  que [les appels RPC sont correctement mis en cache](https://web.archive.org/web/20170608193645/http://etherealbits.com/2012/12/debunking-the-myths-of-rpc-rest/)
  sur des serveurs de mise en cache tels que [Squid](http://www.squid-cache.org/).

### Representational State Transfer (REST)

REST est un style d'architecture qui impose un modèle client/serveur où le client agit sur un ensemble de ressources
gérées par le serveur. Le serveur fournit une représentation des ressources et des actions qui peuvent soit manipuler,
soit obtenir une nouvelle représentation des ressources. Toute communication doit être sans état (stateless) et mise en
cache.

Il existe quatre qualités d'une interface RESTful :

* **Identifier les ressources (URI dans HTTP)** - Utilisez le même URI quelle que soit l'opération.
* **Changer avec les représentations (Verbes dans HTTP)** - Utilisez des verbes, des en-têtes (headers) et le corps de
  la requête.
* **Messagerie d'erreurs auto-descriptive (codes de statut dans HTTP)** - Utilisez des codes de statut, inutile de
  réinventer la roue.
* **[HATEOAS](http://restcookbook.com/Basics/hateoas/) (interface HTML pour HTTP)** - Votre service web doit être
  entièrement accessible via un navigateur.

Exemples d'appels REST:

```
GET /someresources/anId

PUT /someresources/anId
{"anotherdata": "another value"}
```

REST met l'accent sur l'exposition des données. Il minimise le couplage entre client/serveur et est souvent utilisé pour
les API HTTP publiques. REST utilise une méthode plus générique et uniforme pour exposer des ressources via des URI,
des [représentations avec des en-têtes](https://github.com/for-GET/know-your-http-well/blob/master/headers.md), et des
actions via des verbes tels que GET, POST, PUT, DELETE et PATCH. Étant sans état, REST est idéal pour le scaling
horizontal et le partitionnement.

#### Inconvénients: REST

* REST étant centré sur l'exposition des données, il peut ne pas être adapté si les ressources ne sont pas naturellement
  organisées ou accessibles dans une hiérarchie simple. Par exemple, retourner tous les enregistrements mis à jour de la
  dernière heure correspondant à un ensemble d'événements spécifique peut ne pas être facilement exprimé comme un
  chemin. Cela peut nécessiter une combinaison de chemins URI, paramètres de requête, et éventuellement un corps de
  requête.
* REST s'appuie généralement sur quelques verbes (GET, POST, PUT, DELETE et PATCH), ce qui peut parfois ne pas
  correspondre à votre usage. Par exemple, déplacer des documents expirés dans un dossier d'archivage peut ne pas
  s'intégrer nettement dans ces verbes.
* La récupération de ressources complexes avec des hiérarchies imbriquées nécessite plusieurs allers-retours entre le
  client et le serveur pour rendre des vues uniques. Par exemple, récupérer le contenu d'un billet de blog et les
  commentaires sur cette publication. Cela peut être problématique pour les applications mobiles fonctionnant dans des
  conditions de réseau variables.
* Au fil du temps, de nouveaux champs peuvent être ajoutés à une réponse API, et les anciens clients recevront tous les
  nouveaux champs de données, même ceux dont ils n'ont pas besoin. Cela alourdit la taille de la charge utile et
  entraîne de plus grandes latences.

### Comparaison des appels RPC et REST

| Opération                                  | RPC                                                                                       | REST                                                         |
|--------------------------------------------|-------------------------------------------------------------------------------------------|--------------------------------------------------------------|
| Inscription                                | **POST** /signup                                                                          | **POST** /persons                                            |
| Désinscription                             | **POST** /resign<br/>{<br/>"personid": "1234"<br/>}                                       | **DELETE** /persons/1234                                     |
| Lire une personne                          | **GET** /readPerson?personid=1234                                                         | **GET** /persons/1234                                        |
| Lire la liste des objets d'une personne    | **GET** /readUsersItemsList?personid=1234                                                 | **GET** /persons/1234/items                                  |
| Ajouter un objet à la liste d'une personne | **POST** /addItemToUsersItemsList<br/>{<br/>"personid": "1234";<br/>"itemid": "456"<br/>} | **POST** /persons/1234/items<br/>{<br/>"itemid": "456"<br/>} |
| Mettre à jour un objet                     | **POST** /modifyItem<br/>{<br/>"itemid": "456";<br/>"key": "value"<br/>}                  | **PUT** /items/456<br/>{<br/>"key": "value"<br/>}            |
| Supprimer un objet                         | **POST** /removeItem<br/>{<br/>"itemid": "456"<br/>}                                      | **DELETE** /items/456                                        |

<p align="center">
  <i><a href=https://apihandyman.io/do-you-really-know-why-you-prefer-rest-over-rpc/>Source : Do you really know why you prefer REST over RPC</a></i>
</p>

#### Sources et lectures complémentaires : REST et RPC

* [Do you really know why you prefer REST over RPC?](https://apihandyman.io/do-you-really-know-why-you-prefer-rest-over-rpc/)
* [When are RPC-ish approaches more appropriate than REST ?](http://programmers.stackexchange.com/a/181186)
* [REST vs JSON-RPC](http://stackoverflow.com/questions/15056878/rest-vs-json-rpc)
* [Debunking the myths of RPC and REST](https://web.archive.org/web/20170608193645/http://etherealbits.com/2012/12/debunking-the-myths-of-rpc-rest/)
* [What are the drawbacks of using REST ?](https://www.quora.com/What-are-the-drawbacks-of-using-RESTful-APIs)
* [Crack the system design interview](http://www.puncsky.com/blog/2016-02-13-crack-the-system-design-interview)
* [Thrift](https://code.facebook.com/posts/1468950976659943/)
* [Why REST for internal use and not RPC](http://arstechnica.com/civis/viewtopic.php?t=1190508)

## Sécurité

Cette section pourrait être mise à jour. Pensez à [contribuer](#contributing) !

La sécurité est un sujet vaste. À moins d'avoir une expérience considérable, un bagage en sécurité, ou de postuler un poste nécessitant des connaissances en sécurité, vous n'aurez probablement besoin de connaître que les bases :

* Chiffrez les données en transit et au repos.
* Assainissez toutes les entrées utilisateur ou tous les paramètres d'entrée exposés à l'utilisateur pour éviter les [attaques XSS](https://en.wikipedia.org/wiki/Cross-site_scripting) et les [injections SQL](https://en.wikipedia.org/wiki/SQL_injection).
* Utilisez des requêtes paramétrées pour prévenir les injections SQL.
* Respectez le principe du [moindre privilège](https://en.wikipedia.org/wiki/Principle_of_least_privilege).

### Sources et lectures complémentaires

* [API security checklist](https://github.com/shieldfy/API-Security-Checklist)
* [Security guide for developers](https://github.com/FallibleInc/security-guide-for-developers)
* [OWASP top ten](https://www.owasp.org/index.php/OWASP_Top_Ten_Cheat_Sheet)

## Annexe

Il peut vous arriver de devoir effectuer des estimations rapides, souvent appelées "estimations de comptoir". Par exemple, vous pourriez devoir estimer combien de temps il faudra pour générer 100 vignettes d'images à partir du disque ou combien de mémoire une structure de données prendra. Les tableaux **Puissances de deux** et **Chiffres de latence que tout programmeur devrait connaître** sont des références utiles.

### Tableau des puissances de deux

| Puissance |     Valeur exacte | Valeur approximative | Octets |
|:----------|------------------:|:--------------------:|-------:|
| 7         |               128 |                      |        |
| 8         |               256 |                      |        |
| 10        |              1024 |      1 thousand      |   1 KB |
| 16        |            65,536 |                      |  64 KB |
| 20        |         1,048,576 |      1 million       |   1 MB |
| 30        |     1,073,741,824 |      1 billion       |   1 GB |
| 32        |     4,294,967,296 |                      |   4 GB |
| 40        | 1,099,511,627,776 |      1 trillion      |   1 TB |

#### Sources et lectures complémentaires

* [Puissances de deux](https://en.wikipedia.org/wiki/Power_of_two)

### Chiffres de latence que tout programmeur devrait connaître

```
Latency Comparison Numbers
--------------------------
L1 cache reference                           0.5 ns
Branch mispredict                            5   ns
L2 cache reference                           7   ns                      14x L1 cache
Mutex lock/unlock                           25   ns
Main memory reference                      100   ns                      20x L2 cache, 200x L1 cache
Compress 1K bytes with Zippy            10,000   ns       10 us
Send 1 KB bytes over 1 Gbps network     10,000   ns       10 us
Read 4 KB randomly from SSD*           150,000   ns      150 us          ~1GB/sec SSD
Read 1 MB sequentially from memory     250,000   ns      250 us
Round trip within same datacenter      500,000   ns      500 us
Read 1 MB sequentially from SSD*     1,000,000   ns    1,000 us    1 ms  ~1GB/sec SSD, 4X memory
HDD seek                            10,000,000   ns   10,000 us   10 ms  20x datacenter roundtrip
Read 1 MB sequentially from 1 Gbps  10,000,000   ns   10,000 us   10 ms  40x memory, 10X SSD
Read 1 MB sequentially from HDD     30,000,000   ns   30,000 us   30 ms 120x memory, 30X SSD
Send packet CA->Netherlands->CA    150,000,000   ns  150,000 us  150 ms

Notes
-----
1 ns = 10^-9 seconds
1 us = 10^-6 seconds = 1,000 ns
1 ms = 10^-3 seconds = 1,000 us = 1,000,000 ns
```

Quelques métriques utiles basées sur les chiffres ci-dessus :

* Lecture séquentielle depuis un HDD à 30 Mo/s
* Lecture séquentielle depuis un réseau Ethernet de 1 Gbps à 100 Mo/s
* Lecture séquentielle depuis un SSD à 1 Go/s
* Lecture séquentielle depuis la mémoire principale à 4 Go/s
* 6 à 7 allers-retours dans le monde par seconde
* 2 000 allers-retours par seconde dans un datacenter

#### Visualisation des chiffres de latence

![](https://camo.githubusercontent.com/77f72259e1eb58596b564d1ad823af1853bc60a3/687474703a2f2f692e696d6775722e636f6d2f6b307431652e706e67)

#### Sources et lectures complémentaires

* [Chiffres de latence que tout programmeur devrait connaître - 1](https://gist.github.com/jboner/2841832)
* [Chiffres de latence que tout programmeur devrait connaître - 2](https://gist.github.com/hellerbarde/2843375)
* [Conceptions, leçons et conseils pour concevoir de grands systèmes distribués](http://www.cs.cornell.edu/projects/ladis2009/talks/dean-keynote-ladis2009.pdf)
* [Conseils de génie logiciel pour la conception de grands systèmes distribués](https://static.googleusercontent.com/media/research.google.com/en//people/jeff/stanford-295-talk.pdf)

### Questions supplémentaires pour les entretiens sur la conception de systèmes

> Questions fréquentes lors des entretiens de conception de systèmes, avec des liens vers des ressources expliquant comment résoudre chacun des problèmes.

| Question                                                           | Référence(s)                                                                                                                                                                                                                                                                                                                                                                                                                                                |
|--------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Concevez un service de synchronisation de fichiers comme Dropbox   | [youtube.com](https://www.youtube.com/watch?v=PE4gwstWhmc)                                                                                                                                                                                                                                                                                                                                                                                                  |
| Concevez un moteur de recherche comme Google                       | [queue.acm.org](http://queue.acm.org/detail.cfm?id=988407)<br/>[stackexchange.com](http://programmers.stackexchange.com/questions/38324/interview-question-how-would-you-implement-google-search)<br/>[ardendertat.com](http://www.ardendertat.com/2012/01/11/implementing-search-engines/)<br/>[stanford.edu](http://infolab.stanford.edu/~backrub/google.html)                                                                                            |
| Concevez un crawler web évolutif comme Google                      | [quora.com](https://www.quora.com/How-can-I-build-a-web-crawler-from-scratch)                                                                                                                                                                                                                                                                                                                                                                               |
| Concevez Google Docs                                               | [code.google.com](https://code.google.com/p/google-mobwrite/)<br/>[neil.fraser.name](https://neil.fraser.name/writing/sync/)                                                                                                                                                                                                                                                                                                                                |
| Concevez un système clé-valeurs comme Redis                        | [slideshare.net](http://www.slideshare.net/dvirsky/introduction-to-redis)                                                                                                                                                                                                                                                                                                                                                                                   |
| Concevez un système de mise en cache comme Memcached               | [slideshare.net](http://www.slideshare.net/oemebamo/introduction-to-memcached)                                                                                                                                                                                                                                                                                                                                                                              |
| Concevez un système de recommandation comme celui d'Amazon         | [hulu.com](https://web.archive.org/web/20170406065247/http://tech.hulu.com/blog/2011/09/19/recommendation-system.html)<br/>[ijcai13.org](http://ijcai13.org/files/tutorial_slides/td3.pdf)                                                                                                                                                                                                                                                                  |
| Concevez un système de type "tinyurl" comme Bitly                  | [n00tc0d3r.blogspot.com](http://n00tc0d3r.blogspot.com/)                                                                                                                                                                                                                                                                                                                                                                                                    |
| Concevez une application de messagerie instantanée comme WhatsApp  | [highscalability.com](http://highscalability.com/blog/2014/2/26/the-whatsapp-architecture-facebook-bought-for-19-billion.html)                                                                                                                                                                                                                                                                                                                              |
| Concevez un système de partage de photos comme Instagram           | [highscalability.com](http://highscalability.com/flickr-architecture)<br/>[highscalability.com](http://highscalability.com/blog/2011/12/6/instagram-architecture-14-million-users-terabytes-of-photos.html)                                                                                                                                                                                                                                                 |
| Concevez la fonction de flux d'actualité pour Facebook             | [quora.com](http://www.quora.com/What-are-best-practices-for-building-something-like-a-News-Feed)<br/>[quora.com](http://www.quora.com/Activity-Streams/What-are-the-scaling-issues-to-keep-in-mind-while-developing-a-social-network-feed)<br/>[slideshare.net](http://www.slideshare.net/danmckinley/etsy-activity-feeds-architecture)                                                                                                                    |
| Concevez la fonction de timeline pour Facebook                     | [facebook.com](https://www.facebook.com/note.php?note_id=10150468255628920)<br/>[highscalability.com](http://highscalability.com/blog/2012/1/23/facebook-timeline-brought-to-you-by-the-power-of-denormaliza.html)                                                                                                                                                                                                                                          |
| Concevez un système de recherche graphique comme celui de Facebook | [facebook.com](https://www.facebook.com/notes/facebook-engineering/under-the-hood-building-out-the-infrastructure-for-graph-search/10151347573598920)<br/>[facebook.com](https://www.facebook.com/notes/facebook-engineering/under-the-hood-indexing-and-ranking-in-graph-search/10151361720763920)<br/>[facebook.com](https://www.facebook.com/notes/facebook-engineering/under-the-hood-the-natural-language-interface-of-graph-search/10151432733048920) |
| Concevez un réseau de diffusion de contenu (CDN) comme CloudFlare  | [figshare.com](https://figshare.com/articles/Globally_distributed_content_delivery/6605972)                                                                                                                                                                                                                                                                                                                                                                 |
| Concevez un système de sujets tendances comme celui de Twitter     | [michael-noll.com](http://www.michael-noll.com/blog/2013/01/18/implementing-real-time-trending-topics-in-storm/)<br/>[snikolov.wordpress.com](http://snikolov.wordpress.com/2012/11/14/early-detection-of-twitter-trends/)                                                                                                                                                                                                                                  |
| Concevez un système d'identifiants uniques aléatoires              | [blog.twitter.com](https://blog.twitter.com/2010/announcing-snowflake)<br/>[github.com](https://github.com/twitter/snowflake/)                                                                                                                                                                                                                                                                                                                              |
| Concevez un jeu de cartes multijoueur en ligne                     | [indieflashblog.com](https://web.archive.org/web/20180929181117/http://www.indieflashblog.com/how-to-create-an-asynchronous-multiplayer-game.html)<br/>[buildnewgames.com](http://buildnewgames.com/real-time-multiplayer/)                                                                                                                                                                                                                                 |
| Concevez un système de gestion de la collecte des déchets          | [stuffwithstuff.com](http://journal.stuffwithstuff.com/2013/12/08/babys-first-garbage-collector/)<br/>[washington.edu](http://courses.cs.washington.edu/courses/csep521/07wi/prj/rick.pdf)                                                                                                                                                                                                                                                                  |
| Concevez un seuil limite pour les appels d'API                     | [stripe.com](https://stripe.com/blog/rate-limiters)                                                                                                                                                                                                                                                                                                                                                                                                         |
| Concevez une bourse d'échange (comme NASDAQ ou Binance)            | [Jane Street](https://youtu.be/b1e4t2k2KJY)<br/>[Golang Implementation](https://around25.com/blog/building-a-trading-engine-for-a-crypto-exchange/)<br/>[Go Implementation](http://bhomnick.net/building-a-simple-limit-order-in-go/)                                                                                                                                                                                                                       |
| Ajoutez une question sur la conception de système                  | [Contribuer](#contributing)                                                                                                                                                                                                                                                                                                                                                                                                                                 |

### Architectures réelles

> Articles sur comment les systèmes réels sont conçus.

<p align="center">
  <img src="images/TcUo2fw.png">
  <br/>
  <i><a href=https://www.infoq.com/presentations/Twitter-Timeline-Scalability>Source : Les timelines de Twitter à grande échelle</a></i>
</p>

**Ne vous concentrez pas sur les détails insignifiants dans ces articles, mais :**

* Identifiez les principes partagés, les technologies communes, et les modèles décrits dans ces articles
* Étudiez quels problèmes sont résolus par chaque composant, où cela fonctionne et où cela ne fonctionne pas
* Examinez les leçons apprises

| Type                  | Système                                                                                     | Référence(s)                                                                                                                                   |
|-----------------------|---------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------|
| Traitement de données | **MapReduce** - Traitement de données distribué par Google                                  | [research.google.com](http://static.googleusercontent.com/media/research.google.com/zh-CN/us/archive/mapreduce-osdi04.pdf)                     |
| Traitement de données | **Spark** - Traitement de données distribué par Databricks                                  | [slideshare.net](http://www.slideshare.net/AGrishchenko/apache-spark-architecture)                                                             |
| Traitement de données | **Storm** - Traitement de données distribué par Twitter                                     | [slideshare.net](http://www.slideshare.net/previa/storm-16094009)                                                                              |
|                       |                                                                                             |                                                                                                                                                |
| Stockage de données   | **Bigtable** - Base de données orientée colonnes distribuée par Google                      | [harvard.edu](http://www.read.seas.harvard.edu/~kohler/class/cs239-w08/chang06bigtable.pdf)                                                    |
| Stockage de données   | **HBase** - Implémentation open-source de Bigtable                                          | [slideshare.net](http://www.slideshare.net/alexbaranau/intro-to-hbase)                                                                         |
| Stockage de données   | **Cassandra** - Base de données orientée colonnes distribuée par Facebook                   | [slideshare.net](http://www.slideshare.net/planetcassandra/cassandra-introduction-features-30103666)                                           |
| Stockage de données   | **DynamoDB** - Base de données orientée documents par Amazon                                | [harvard.edu](http://www.read.seas.harvard.edu/~kohler/class/cs239-w08/decandia07dynamo.pdf)                                                   |
| Stockage de données   | **MongoDB** - Base de données orientée documents                                            | [slideshare.net](http://www.slideshare.net/mdirolf/introduction-to-mongodb)                                                                    |
| Stockage de données   | **Spanner** - Base de données distribuée à l'échelle mondiale par Google                    | [research.google.com](http://research.google.com/archive/spanner-osdi2012.pdf)                                                                 |
| Stockage de données   | **Memcached** - Système de mise en cache distribué en mémoire                               | [slideshare.net](http://www.slideshare.net/oemebamo/introduction-to-memcached)                                                                 |
| Stockage de données   | **Redis** - Système de mise en cache distribué en mémoire avec persistance                  | [slideshare.net](http://www.slideshare.net/dvirsky/introduction-to-redis)                                                                      |
|                       |                                                                                             |                                                                                                                                                |
| Système de fichiers   | **Google File System (GFS)** - Système de fichiers distribué                                | [research.google.com](http://static.googleusercontent.com/media/research.google.com/zh-CN/us/archive/gfs-sosp2003.pdf)                         |
| Système de fichiers   | **Hadoop File System (HDFS)** - Implémentation open-source de GFS                           | [apache.org](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html)                                             |
|                       |                                                                                             |                                                                                                                                                |
| Divers                | **Chubby** - Service de verrouillage pour systèmes distribués faiblement couplés par Google | [research.google.com](http://static.googleusercontent.com/external_content/untrusted_dlcp/research.google.com/en/us/archive/chubby-osdi06.pdf) |
| Divers                | **Dapper** - Infrastructure de traçage pour systèmes distribués                             | [research.google.com](http://static.googleusercontent.com/media/research.google.com/en//pubs/archive/36356.pdf)                                |
| Divers                | **Kafka** - File d'attente de messages Pub/Sub par LinkedIn                                 | [slideshare.net](http://www.slideshare.net/mumrah/kafka-talk-tri-hug)                                                                          |
| Divers                | **Zookeeper** - Infrastructure centralisée permettant la synchronisation                    | [slideshare.net](http://www.slideshare.net/sauravhaloi/introduction-to-apache-zookeeper)                                                       |
|                       | Ajoutez une architecture                                                                    | [Contribuer](#contributing)                                                                                                                    |

### Architectures des entreprises

| Entreprise     | Référence(s)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
|----------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Amazon         | [Architecture Amazon](http://highscalability.com/amazon-architecture)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| Cinchcast      | [Produire 1 500 heures d'audio par jour](http://highscalability.com/blog/2012/7/16/cinchcast-architecture-producing-1500-hours-of-audio-every-d.html)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| DataSift       | [Analyse de données en temps réel à 120,000 tweets par seconde](http://highscalability.com/blog/2011/11/29/datasift-architecture-realtime-datamining-at-120000-tweets-p.html)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| Dropbox        | [Comment nous avons fait évoluer Dropbox](https://www.youtube.com/watch?v=PE4gwstWhmc)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| ESPN           | [Opérations à grande échelle : 100 000 "duh nuh nuhs" par seconde](http://highscalability.com/blog/2013/11/4/espns-architecture-at-scale-operating-at-100000-duh-nuh-nuhs.html)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| Google         | [Architecture Google](http://highscalability.com/google-architecture)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| Instagram      | [14 millions d'utilisateurs, téraoctets de photos](http://highscalability.com/blog/2011/12/6/instagram-architecture-14-million-users-terabytes-of-photos.html)<br/>[Ce qui propulse Instagram](http://instagram-engineering.tumblr.com/post/13649370142/what-powers-instagram-hundreds-of-instances)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| Justin.tv      | [Architecture de diffusion vidéo en direct de Justin.tv](http://highscalability.com/blog/2010/3/16/justintvs-live-video-broadcasting-architecture.html)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| Facebook       | [Scaling Memcached chez Facebook](https://cs.uwaterloo.ca/~brecht/courses/854-Emerging-2014/readings/key-value/fb-memcached-nsdi-2013.pdf)<br/>[TAO : Base de données distribuée de Facebook pour le graphe social](https://cs.uwaterloo.ca/~brecht/courses/854-Emerging-2014/readings/data-store/tao-facebook-distributed-datastore-atc-2013.pdf)<br/>[Stockage des photos sur Facebook](https://www.usenix.org/legacy/event/osdi10/tech/full_papers/Beaver.pdf)<br/>[Comment Facebook met en streaming 800 000 spectateurs simultanés](http://highscalability.com/blog/2016/6/27/how-facebook-live-streams-to-800000-simultaneous-viewers.html)                                                                                                                                                                                                                                                                                               |
| Flickr         | [Architecture Flickr](http://highscalability.com/flickr-architecture)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| Mailbox        | [Passer de 0 à un million d'utilisateurs en 6 semaines](http://highscalability.com/blog/2013/6/18/scaling-mailbox-from-0-to-one-million-users-in-6-weeks-and-1.html)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| Netflix        | [Une vue à 360 degrés sur toute la pile Netflix](http://highscalability.com/blog/2015/11/9/a-360-degree-view-of-the-entire-netflix-stack.html)<br/>[Netflix : Que se passe-t-il lorsque vous appuyez sur Play ?](http://highscalability.com/blog/2017/12/11/netflix-what-happens-when-you-press-play.html)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| Pinterest      | [Passer de 0 à des dizaines de milliards de pages vues par mois](http://highscalability.com/blog/2013/4/15/scaling-pinterest-from-0-to-10s-of-billions-of-page-views-a.html)<br/>[18 millions de visiteurs, une croissance x10, 12 employés](http://highscalability.com/blog/2012/5/21/pinterest-architecture-update-18-million-visitors-10x-growth.html)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| Playfish       | [50 millions d'utilisateurs mensuels et en croissance](http://highscalability.com/blog/2010/9/21/playfishs-social-gaming-architecture-50-million-monthly-user.html)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| PlentyOfFish   | [Architecture PlentyOfFish](http://highscalability.com/plentyoffish-architecture)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| Salesforce     | [Comment gérer 1,3 milliard de transactions par jour](http://highscalability.com/blog/2013/9/23/salesforce-architecture-how-they-handle-13-billion-transacti.html)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| Stack Overflow | [Architecture de Stack Overflow](http://highscalability.com/blog/2009/8/5/stack-overflow-architecture.html)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| TripAdvisor    | [40M de visiteurs, 200M de pages dynamiques, 30 To de données](http://highscalability.com/blog/2011/6/27/tripadvisor-architecture-40m-visitors-200m-dynamic-page-view.html)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| Tumblr         | [15 milliards de pages vues par mois](http://highscalability.com/blog/2012/2/13/tumblr-architecture-15-billion-page-views-a-month-and-harder.html)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| Twitter        | [Rendre Twitter 10 000 % plus rapide](http://highscalability.com/scaling-twitter-making-twitter-10000-percent-faster)<br/>[Stockage de 250 millions de tweets par jour avec MySQL](http://highscalability.com/blog/2011/12/19/how-twitter-stores-250-million-tweets-a-day-using-mysql.html)<br/>[150M d'utilisateurs actifs, 300K QPS, un flux de 22 Mo/s](http://highscalability.com/blog/2013/7/8/the-architecture-twitter-uses-to-deal-with-150m-active-users.html)<br/>[Timelines à grande échelle](https://www.infoq.com/presentations/Twitter-Timeline-Scalability)<br/>[Petites et grandes données chez Twitter](https://www.youtube.com/watch?v=5cKTP36HVgI)<br/>[Opérations chez Twitter : passer au-delà des 100 millions d'utilisateurs](https://www.youtube.com/watch?v=z8LU0Cj6BOU)<br/>[Comment Twitter gère 3 000 images par seconde](http://highscalability.com/blog/2016/4/20/how-twitter-handles-3000-images-per-second.html) |
| Uber           | [Comment Uber fait évoluer sa plateforme de marché en temps réel](http://highscalability.com/blog/2015/9/14/how-uber-scales-their-real-time-market-platform.html)<br/>[Leçons apprises : Passer à 2 000 ingénieurs, 1 000 services et 8 000 dépôts](http://highscalability.com/blog/2016/10/12/lessons-learned-from-scaling-uber-to-2000-engineers-1000-ser.html)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
| WhatsApp       | [L'architecture de WhatsApp achetée par Facebook pour 19 milliards de dollars](http://highscalability.com/blog/2014/2/26/the-whatsapp-architecture-facebook-bought-for-19-billion.html)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| YouTube        | [Évolutivité de YouTube](https://www.youtube.com/watch?v=w5WVu624fY8)<br/>[Architecture YouTube](http://highscalability.com/youtube-architecture)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |

### Blogs d'ingénierie des entreprises

> Architectures des entreprises pour lesquelles vous passez un entretien.
>
> Les questions que vous rencontrez pourraient être issues du même domaine.

* [Airbnb Engineering](http://nerds.airbnb.com/)
* [Atlassian Developers](https://developer.atlassian.com/blog/)
* [AWS Blog](https://aws.amazon.com/blogs/aws/)
* [Bitly Engineering Blog](http://word.bitly.com/)
* [Box Blogs](https://blog.box.com/blog/category/engineering)
* [Cloudera Developer Blog](http://blog.cloudera.com/)
* [Dropbox Tech Blog](https://tech.dropbox.com/)
* [Engineering at Quora](https://www.quora.com/q/quoraengineering)
* [Ebay Tech Blog](http://www.ebaytechblog.com/)
* [Evernote Tech Blog](https://blog.evernote.com/tech/)
* [Etsy Code as Craft](http://codeascraft.com/)
* [Facebook Engineering](https://www.facebook.com/Engineering)
* [Flickr Code](http://code.flickr.net/)
* [Foursquare Engineering Blog](http://engineering.foursquare.com/)
* [GitHub Engineering Blog](https://github.blog/category/engineering)
* [Google Research Blog](http://googleresearch.blogspot.com/)
* [Groupon Engineering Blog](https://engineering.groupon.com/)
* [Heroku Engineering Blog](https://engineering.heroku.com/)
* [Hubspot Engineering Blog](http://product.hubspot.com/blog/topic/engineering)
* [High Scalability](http://highscalability.com/)
* [Instagram Engineering](http://instagram-engineering.tumblr.com/)
* [Intel Software Blog](https://software.intel.com/en-us/blogs/)
* [Jane Street Tech Blog](https://blogs.janestreet.com/category/ocaml/)
* [LinkedIn Engineering](http://engineering.linkedin.com/blog)
* [Microsoft Engineering](https://engineering.microsoft.com/)
* [Microsoft Python Engineering](https://blogs.msdn.microsoft.com/pythonengineering/)
* [Netflix Tech Blog](http://techblog.netflix.com/)
* [Paypal Developer Blog](https://medium.com/paypal-engineering)
* [Pinterest Engineering Blog](https://medium.com/@Pinterest_Engineering)
* [Reddit Blog](http://www.redditblog.com/)
* [Salesforce Engineering Blog](https://developer.salesforce.com/blogs/engineering/)
* [Slack Engineering Blog](https://slack.engineering/)
* [Spotify Labs](https://labs.spotify.com/)
* [Stripe Engineering Blog](https://stripe.com/blog/engineering)
* [Twilio Engineering Blog](http://www.twilio.com/engineering)
* [Twitter Engineering](https://blog.twitter.com/engineering/)
* [Uber Engineering Blog](http://eng.uber.com/)
* [Yahoo Engineering Blog](http://yahooeng.tumblr.com/)
* [Yelp Engineering Blog](http://engineeringblog.yelp.com/)
* [Zynga Engineering Blog](https://www.zynga.com/blogs/engineering)

#### Sources et lectures complémentaires

Si vous cherchez à ajouter un blog, et pour éviter de dupliquer le travail, envisagez d'ajouter le blog de votre entreprise au dépôt suivant :

* [kilimchoi/engineering-blogs](https://github.com/kilimchoi/engineering-blogs)

## En cours de développement

Intéressé(e) à ajouter une section ou à compléter une partie en cours ? [Contribuer](#contributing) !

* Informatique distribuée avec MapReduce
* Hachage consistant
* Scatter-Gather
* [Contribuer](#contributing)

## Crédits

Les crédits et les sources sont fournis tout au long de ce dépôt.

Remerciements particuliers à :

* [Hired in tech](http://www.hiredintech.com/system-design/the-system-design-process/)
* [Cracking the coding interview](https://www.amazon.com/dp/0984782850/)
* [High scalability](http://highscalability.com/)
* [checkcheckzz/system-design-interview](https://github.com/checkcheckzz/system-design-interview)
* [shashank88/system_design](https://github.com/shashank88/system_design)
* [mmcgrana/services-engineering](https://github.com/mmcgrana/services-engineering)
* [System design cheat sheet](https://gist.github.com/vasanthk/485d1c25737e8e72759f)
* [A distributed systems reading list](http://dancres.github.io/Pages/)
* [Cracking the system design interview](http://www.puncsky.com/blog/2016-02-13-crack-the-system-design-interview)

## Informations de contact

N'hésitez pas à me contacter pour discuter de n'importe quel problème, question ou commentaire.

Mes informations de contact se trouvent sur ma [page GitHub](https://github.com/donnemartin).

## Licence

*Je vous fournis le code et les ressources de ce dépôt sous une licence open source. Étant donné qu'il s'agit de mon dépôt personnel, la licence que vous recevez pour mon code et mes ressources provient de moi et non de mon employeur (Facebook).*

    Copyright 2017 Donne Martin

    Licence Creative Commons Attribution 4.0 International (CC BY 4.0)

    http://creativecommons.org/licenses/by/4.0/