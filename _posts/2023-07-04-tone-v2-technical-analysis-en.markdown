---
title:  "Tone technical presentation"
categories: projets tone
permalink: /tone-technical/
tags: data_analysis videogame_modding northstar titanfall_2 technical
lang: en
---
Tone didn't just get created all in one go. Because multiple programs needed to be created, a Github organization has been created. You can find it here:  [ToneAPI Github org][Github].

At the day I write this article, Tone is running on its second major version.
## V 1.0
Tone was at first very simple. The project started almost from scratch, two months after Northstar released an update allowing mods to create HTTP requests from the game.
![Northstar update announcement](/assets/images/Northstar%20HTTP%20announcement.png)

To make it work, three programs were needed
* #### [Game server mod][Tone_servermod]
This mod gathers ingame data and sends them to Tone's servers through REST HTTP requests.
It's a fork of [fvnkhead.killstat][fvnkhead.killstat] which is a mod created by Fvnkhead to gather statistics into a CSV file.
This mod is written in Squirrel, the scripting language used by Titanfall2.

* #### [Tone servers][Tone_backend]
Tone servers recieve informations from game servers, computes them and returns them to clients that request statistics.
Data is collected by a [NodeJS][NodeJS] process, then saved into a [PostgresSQL][Postgres] server.
Tone servers provide a REST API, allowing game servers to sumbit data and clients to request statistics. The documentation for this API is available through its [github repository][Tone_backend].

* #### [Tone website][Tone_webclient]
Tone's website fetches data from Tone servers and shows them in charts and tables.
It is created in [Vue][VueJS] and uses [Chart.js][ChartJS].
![Tone webclient screenshot][Tone_og]

Because game servers are hosted by the community, anyone could upload fake data and manipulate statistics.
To resolve this issue, an unique authentication token is given to each host. It is used to separate data by host. Thus, if someone decides to falsify statistics, it will be possible to revoke their access and delete the faked data.

Tone saves each kill in the database. This decision has been made to allow more granularity over data analysis.
This decision caused numerous performance problems by lack of optimization. Those problems will be partially explained in the next chapter of this article.

The following article represents the final architecture of the first version of Tone.
<object style="max-width:100%" data="{{site.baseurl}}/assets/images/diagram-tone-v1-en.svg" type="image/svg+xml" class="mailicon"></object>


## V 2.0
Tone's second version allowed us to tackle some scaling problems.

Because Tone saves each kill in the database, the server had to go through every kill at each client request. 
To resolve this, I opted to use a caching system that would preliminarily regroup data to reduce the temporal complexity.

The following diagram is an illustration of this process : the table on the left contains all kills entries, while the table on the right groups them by player and weapon.
![Tone v2 diagram](/assets/images/tone-v2-caching.svg)

Another caching layer is implemented inside the NodeJS process, allowing clients to fetch the player list in a flash.
All cached data is updated in realtime thanks to a [trigger][Postgres_trigger].
![Tone v2 diagram](/assets/images/diagram-tone-v2-en.png)

This second version also corresponds to a change in the REST API used by game servers. A change was necessary to allow hosts to use only one token for all of their game server.

## V 3.0
I've had the chance to recieve help from two other Titanfall2 fans for this project. [Okudai][Okvdai] and [Lars][Lars] are now members of the [Github organization][Github].
They both develop clients that fetch statistics from Tone API : a Northstar client mod named [Pulse] and a Discord bot named [Sonar] respectively.

The third iteration of Tone is still in the design stage of development. It will address problems linked to the monolithic model and thus will allow saving data under multiple tables. This change is necessary to allow Tone to provide new statistics while partially mitigating performance problems.

The following diagram is an example of a database model. It has been entirely created by Lars.
![Tone v3 diagram](/assets/images/Tone-v3-model.png)

## References
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