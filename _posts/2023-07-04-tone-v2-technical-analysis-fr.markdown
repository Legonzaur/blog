---
title:  "Tone v2 : présentation technique"
categories: projets tone
tags: data_analysis videogame_modding northstar titanfall_2 technical
lang: fr
---
Le processus de création de [Tone] n'a pas été direct. Comme ce projet nécessite une implémentation sur plusieurs niveaux ([cf. Introduction à Tone][Tone]), plusieurs programmes ont été créés. Ceux ci peuvent être trouvés sur [l'organisation Github du projet.][Github]

Aujourd'hui, Tone en est à sa deuxième itération majeure. 
## V 1.0
La première version de Tone a été très simpliste. Le projet partait quasiment de zéro, deux mois après que Northstar ait sorti une mise à jour permettant de faire des requêtes HTTP depuis le jeu.
![Announement de la mise à jour permettant les requêtes HTTP](/assets/images/Northstar%20HTTP%20announcement.png)
Afin de pouvoir fonctionner, il a fallu créer trois programmes : 
* ### [Mod coté serveur de jeu][Tone_servermod]
Ce mod se charge de récupérer les données en jeu et de les envoyer au serveurs de Tone via des requêtes HTTP.
Il est une fork de [fvnkhead.killstat][fvnkhead.killstat], mod crée par Fvnkhead afin de récolter des statistiques dans un fichier CSV. 
* ### [Serveurs de Tone][Tone_backend]
Les serveurs de Tone reçoivent les informations transmises par le mod, les traitent, puis les restituent aux clients.
* ### [Site internet de visualisation des données][Tone_webclient]
Le site internet récupères les données depuis les serveurs de Tone, puis les affiche sous la forme de graphes et de tableaux.
## V 2.0
[Tone]:  /tone-introduction-fr
[fvnkhead.killstat]: https://github.com/fvnkhead/fvnkhead.killstat
[Tone_servermod]: https://github.com/ToneAPI/ToneAPI_servermod
[Tone_backend]: https://github.com/ToneAPI/ToneAPI_backend
[Tone_webclient]: https://github.com/ToneAPI/ToneAPI_webclient