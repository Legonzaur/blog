---
title:  "Tone v2 : présentation technique"
categories: projets tone
permalink: /tone-technical/
tags: data_analysis videogame_modding northstar titanfall_2 technical
lang: fr
---
Le processus de création de Tone n'a pas fait en un jour. Comme ce projet nécessite la création de plusieurs programmes, une organisation Github les regroupant a été crée. Celle ci peut être accédée au lien suivant :  [ToneAPI Github org][Github].

Au jour où cet article est écrit, Tone en est à sa deuxième version majeure.
## V 1.0
La première version de Tone a été très simpliste. Le projet partait quasiment de zéro, deux mois après que Northstar ait sorti une mise à jour permettant de faire des requêtes HTTP depuis le jeu.
![Announement de la mise à jour permettant les requêtes HTTP](/assets/images/Northstar%20HTTP%20announcement.png)

Afin de pouvoir fonctionner, il a fallu créer trois programmes : 
* #### [Mod coté serveur de jeu][Tone_servermod]
Ce mod se charge de récupérer les données en jeu et de les envoyer au serveurs de Tone via des requêtes HTTP.
Il est une fork de [fvnkhead.killstat], mod crée par Fvnkhead afin de récolter des statistiques dans un fichier CSV. 
Ce mod est codé en Squirrel, langage de scripting utilisé par Titanfall 2.
Le mod fonctionne de manière très simple : il envoie une requête POST à l'API REST fournie par les serveurs de Tone.

* #### [Serveurs de Tone][Tone_backend]
Les serveurs de Tone reçoivent les informations transmises par le mod, les traitent, puis les restituent aux clients.
Les données sont reçues par un processus [NodeJS], puis sauvegardées dans un serveur [PostgresSQL][Postgres].
Le serveurs fournissent une API REST permettant aux serveurs de jeu de soumettre des informations, et aux clients de les récupérer. La documentation de cette API est accessible via [son repository github][Tone_backend].

* #### [Site internet de visualisation des données][Tone_webclient]
Le [site internet][Tone_webclient] récupères les données depuis les serveurs de Tone, puis les affiche sous la forme de graphes et de tableaux.
Il est crée en [Vue][VueJS] et utilise la librarie [Chart.js][ChartJS]
![Tone webclient screenshot][Tone_og]

Les serveurs étant hébergés par la communauté, cela signifie que n'importe qui pourrait mettre en ligne des fausses données, et par conséquent manipuler les statistiques.
Afin de résoudre ce problème, un token d'authentification unique est assigné à chaque hébergeur. Il est utilisé afin de séparer les données par hébergeur. Ainsi, si une personne décide de falsifier les statistiques, il sera possible de lui révoquer l'accès et de supprimer les fausses données.

Tone enregistre chaque kill dans la base de donnée. Cette décision a été prise afin de permettre d'avoir plus de granularité sur les possibilités d'analyses de données.
Cette décision a causé de nombreux problèmes de performances, par manque d'optimisation. Ceux ci sont partiellement expliqués dans le chapitre suivant de cet article.

Le diagramme suivant représente l'architecture finale de la première version de Tone.
<object style="max-width:100%" data="{{site.baseurl}}/assets/images/diagram-tone-v1-fr.svg" type="image/svg+xml" class="mailicon"></object>


## V 2.0
La deuxième itération de Tone a été créée afin de résoudre certains problèmes d'échelle.

En effet, parce que Tone enregistre chaque kill dans la base de données, le serveur devait reparcourir toutes les entrées à chaque requête client.
Afin de résoudre ce problème, j'ai opté pour un système de cache a été mis en place, permettant de regrouper les données préalablement afin de réduire la complexité temporelle liée à la lecture des données.

Le diagramme suivant illustre la mise en cache des informations : la table de gauche comporte toutes les entrées de kills, tandis que la table de droite les regroupe les morts et kills par joueur arme.
![Tone v2 diagram](/assets/images/tone-v2-caching.svg)
Ce système permet de réduire le nombre d'entrées à parcourir en regroupant préalablement les données.

Une autre couche de cache est mise en place à l'intérieur du processus nodejs, permettant de récupérer la liste des joueurs sans avoir à faire de requêtes à la base de données.
Toutes les couches de cache sont mises à jour en temps réel grâce à un [trigger][Postgres_trigger].
![Tone v2 diagram](/assets/images/diagram-tone-v2-en.png)

Cette deuxième version correspond également à un changement de l'API REST utilisée par les serveurs de jeu. Un changement était nécessaire afin de permettre aux hébergeurs d'utiliser un seul token pour tous leurs serveurs. 

## V 3.0
J'ai eu la change de pouvoir recevoir de l'aide de la part d'autres fans de Titanfall 2 dans ce projet. [Okvdai] et [Lars] sont mainteant membres de  [l'organisation github][Github].
Ils développent tous les deux des clients récupérant des statistiques depuis l'API Tone ; respectivement [Pulse], un mod client Northstar, et [Sonar], un bot discord.

La troixième itération de Tone n'en est encore qu'à l'état de design. Cependant, elle addressera les problèmes liés à l'organisation monolythique de la base de donnée, et sauvegardera les données sous plusieurs tables. Ce changement est nécessaire afin de fournir de nouvelles statistiques tout en mitigeant partiellement les problèmes de performances.

Le diagramme suivant est un exemple de ce à quoi pourra ressembler la base de données en production. Il a été réalisé entièrement par Lars.
![Tone v3 diagram](/assets/images/Tone-v3-model.png)

## Réferences
- [Okvdai Github profile][Okvdai]
- [Lars Github profile][Lars]
- [Tone Github org](https://github.com/ToneAPI)
- [Northstar Github org](https://github.com/R2Northstar)
- [Northstar HTTP documentation](https://r2northstar.readthedocs.io/en/latest/reference/northstar/httprequests.html)

- [How I Have Set Up a Cost-Effective Modern Data Stack for a Charity - Marie Lestavel](https://medium.com/@marielestavel/how-i-have-set-up-a-cost-effective-modern-data-stack-for-a-charity-3fe7e7c9162)

[Github]: https://github.com/ToneAPI
[fvnkhead.killstat]: https://github.com/fvnkhead/fvnkhead.killstat
[Tone_servermod]: https://github.com/ToneAPI/ToneAPI_servermod
[Tone_backend]: https://github.com/ToneAPI/ToneAPI_backend
[Tone_webclient]: https://github.com/ToneAPI/ToneAPI_webclient
[Pulse]: https://github.com/ToneAPI/pulse
[Sonar]: https://github.com/ToneAPI/Sonar
[Tone_og]: https://toneapi.github.io/ToneAPI_webclient/og_image.png
[VueJS]: https://vuejs.org/
[ChartJS]: https://www.chartjs.org/
[NodeJS]: https://nodejs.org/en
[Postgres]: https://www.postgresql.org/
[Postgres_trigger]: https://www.postgresql.org/docs/current/sql-createtrigger.html
[Okvdai]: https://github.com/okvdai
[Lars]: https://github.com/iiLarsH