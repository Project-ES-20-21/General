---
layout: default
title: Communicatie
parent: Ontsmetting
grand_parent: Onderdelen
nav_order: 3
---

# Communicatie

Communicatie met andere proeven gebeurt aan de hand van het MQTT protocol via de broker. De ESP reageert op 3 signalen en stuurt er evenveel uit.


| topic                     | bericht| betekenis                         |  zender   | ontvanger |
| "esp32/ontsmetten/control"|'0'| reset de ESP-32                   |           |     X     |
| "esp32/ontsmetten/control"|'1'| start ontsmettingssequentie       |           |     X     |
| "esp32/ontsmetten/id"     |'##'| id's te onsmetten personen        |           |     X     |
| "esp32/ontsmetten/ip"     |'IP'| post het IP-adres van de ESP32    |     X     |           |
| "esp32/ + /control"       |'2'| hervat de andere puzzels           |     X     |           |
|"esp32/ontsmetten/monitor" |'...'| imiteert functie seriële monitor  |     X     |           |
