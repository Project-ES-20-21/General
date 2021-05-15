---
layout: default
title: Anderhalvemeter
parent: Afstand
grand_parent: Onderdelen
nav_order: 2
---

# Error handling
* Wanneer er tijdens het spel iets misloopt kunnen alle modules gereset worden door een "0" te sturen op het mqtt-kanaal "esp32/afstand/control".
* De externe antenne moet recht naar boven staan en er mogen geen obstructies zijn tussen de antenne en andere zenders. Indien aan deze voorwaarden niet voldaan is zullen de
BLE-signalen met minder energie aankomen met een een foute afstandsinschatting tot gevolg.
* Omwille van de vele ble-signalen die worden verzonden en het continu processen van RSSI-metingen heeft de schakeling een groot verbruik. Indien de schakeling niet lijkt te 
werken raden we aan om hem eens via micro-usb te voeden om te controleren of de batterij niet leeg is.
* Het kan lijken dat de module niets doet wanneer er slechts één module aanstaat. Dit komt omdat de code zo is geschreven dat de module pas begint te werken wanneer hij 
ble-signalen van een andere module ontvangt.
* Bij het inladen van de code op de ESP moet manueel de naam van de module in de code worden aangepast. Indien dit niet gebeurt zullen modules dezelfde naam hebben en elkaar
verdringen bij zowel ble als mqtt.
* De module luistert naar verschillende mqtt-kanalen. Men moet opletten dat hierop geen onverwachte berichten worden verzonden omdat de ESP dit als een commando zou kunnen
interpreteren waardoor hij acties onderneemt die niet de bedoeling zijn.
