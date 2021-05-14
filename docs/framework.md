---
layout: default
title: Framework
parent: Onderdelen
nav_order: 1
---

# Framework
Als MQTT broker werd ons een Raspberry Pi (v2) aangeboden. Om alle communicatie met het MQTT protocol vlot te laten verlopen moest wel nog een framework gekozen worden om op de Raspberry Pi te installeren. We lichten onze keuzes omtrent het framework voor dit project even bij.

## Mosquitto
Voor ons project hebben we als basis gekozen voor **Eclipse Mosquitto™**.
Eclipse Mosquitto is een *open source message broker* die het MQTT protocol versie 5.0, 3.1.1 en 3.1 implementeert. Mosquitto is *lightweight* en is geschikt voor gebruik op alle apparaten van *low power single board computers* tot volledige servers.
Wat ons vooral aansprak aan Mosquitto was de simpele implementatie, testen met een applicatie op een smartphone of een laptop is zeer makkelijk, en de mogelijkheid tot aanmaken van een user interface met **Node-RED** zonder extra aanpassingen.

## Node-RED
Om een gebruiksvriendelijke interface te voorzien voor onze escape room hebben we dus gebruik gemaakt van Node-RED.
Node-RED is een handige tool die toelaat om op een simpele manier *flows* te implementeren en deze *flows* virtueel met elkaar te verbinden om zo de communicatie tussen verschillende apparaten te automatiseren en/of te verduidelijken. Node-RED heeft een aantal voordelen die ons overtuigd hebben om deze tool te gebruiken, we lijsten ze even op:
- Node-RED biedt een flow editor aan die gebruikt kan worden van uit je browser. Aanmaken, bewerken, opslaan,... alles kan zonder installatie van een extra programma.
- Node-RED is gebaseerd op Node.js. Dit is vooral interessant aangezien we werken met een Raspberry Pi, Node.js is namelijk *light-weight* en zeker compatibel met *low-cost hardware* zoals Raspberry Pi.
- Ten slotte is er nog het feit dat alle *flows* en dergelijke worden opgeslaan als .js bestanden, wat een vrij leesbaar en makkelijk te delen bestandsextensie is.
