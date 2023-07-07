---
title:  "Introduction à Tone"
categories: projets tone
permalink: /tone-introduction/
tags: data_analysis videogame_modding northstar titanfall_2
lang: fr
---
Tone est un projet qui me tient à coeur, car il lie à la fois Titanfall 2, mon jeu de tir à la première personne multijoueur favori, et plusieurs domaines du développement informatique à la fois.

Avant de le présenter, voici une brève une introduction à son contexte.
### Titanfall 2
Titanfall 2 est un jeu de tir nerveux à la premiere personne sorti en 2016, développé par Respawn, publié par EA.

Il a été sujet à [de nombreux débats][harmony-redtape] entre 2020 et 2021. Durant cette période, des failles du jeu et des attaques étaient lancées sur les seveurs du jeu, les rendant inaccessibles pendant plusieurs mois. 

### Northstar 
[Northstar][northstar] est un projet maintenant devenu communautaire qui a vu le jour suite aux serveurs du jeu devenus inaccessibles. Celui ci permet à n'importe qui d'héberger ses propres serveurs de jeu.

Aujourd'hui, Northstar accueille en moyenne un peu plus de 200 joueurs concurrents sur tous les serveurs communautaires confondus.

### Qu'est ce qu'est Tone ? 
A cause de sa nature décentralisée, le projet Northstar n'implémente pas de fonctionnalités permettant de sauvegarder les données relatives aux parties de jeu.

Tone est un projet qui vise à collecter des données durant les parties de jeu, permettant alors à chaque joueur de visualiser ses statistiques.
Celui ci se sépare en trois majeures parties : 
* Collecte de données
* Sauvegarde et traitement des données
* Affichage des données
<object style="max-width:100%" data="{{site.baseurl}}/assets/images/diagram-tone-simple-fr.svg" type="image/svg+xml" class="mailicon"></object>

Le projet a été lancé le 15 mars 2023. A l'heure où vous lisez cet article, plus de <span id=toneKills>8</span> millions de `kills` ont été enregistrés dans la base de données.

<script type="application/javascript">
    window.addEventListener('DOMContentLoaded', async () => {
    const kills = Object.values(await (await fetch("https://tone.sleepycat.date/v2/client/players")).json()).reduce((acc,e)=>acc+e.kills,0);
    document.getElementById('toneKills').innerText = Math.floor(kills/1000000)
});
    
</script>

[harmony-redtape]:  https://harmony.tf/redtape-response/
[northstar]:        https://northstar.tf/