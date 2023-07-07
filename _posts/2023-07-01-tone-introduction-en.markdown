---
title:  "Tone Introduction"
categories: projets tone
permalink: /tone-introduction/
tags: data_analysis videogame_modding northstar titanfall_2
lang: en
---
Because Tone links both Titanfall 2, my favourite FPS game and multiple domains of IT development, this project is close to my heart.

Before presenting it, here is a brief introduction of its context.
### Titanfall 2
Titanfall 2 is a first person shooter released in 2016, developed by Respawn and published by EA.

It has been subject of [multiple dramas][harmony-redtape] around 2020. During this period, game hacks and security flaws has been exploited during multiple months, making the game servers unplayable. 

### Northstar 
[Northstar][northstar] is a community project that was born because of the inaccessibility of the official Titanfall2 servers. Northstar allows anyone to host their own game servers.

Today, Northstar hosts around 200 players in all community servers

### What is Tone ? 
Because of its decentralized nature, Northstar doesn't implements saving statistics of the games.

Tone aims to collect statistics from the games, allowing every player to visualize them.
It is split into three major parts : 

* Data gathering
* Data backup and processing
* Data display
<object style="max-width:100%" data="{{site.baseurl}}/assets/images/diagram-tone-simple-en.svg" type="image/svg+xml" class="mailicon"></object>

The project has been created the 15 march 2023. At the moment you read this article, more than <span id=toneKills>8</span> million `kills` have been recorded in the database.

<script type="application/javascript">
    window.addEventListener('DOMContentLoaded', async () => {
    const kills = Object.values(await (await fetch("https://tone.sleepycat.date/v2/client/players")).json()).reduce((acc,e)=>acc+e.kills,0);
    document.getElementById('toneKills').innerText = Math.floor(kills/1000000)
});
    
</script>

[harmony-redtape]:  https://harmony.tf/redtape-response/
[northstar]:        https://northstar.tf/