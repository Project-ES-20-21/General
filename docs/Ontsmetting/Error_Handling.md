---
layout: default
title: ErrorHandling
parent: Ontsmetting
grand_parent: Onderdelen
nav_order: 5
---

# Error handling

Wanneer zich een fout voordoet, kan de ESP-32 softwarematig gereset worden. Dit gebeurt via een MQTT bericht. De ESP-32 connecteert automatisch met het topic "esp32/ontsmetten/control". Wanneer het bericht '0' op dit topic geplaatst wordt, zal deze zich resetten via de functie `esp.restart`.

Om visueel zichtbaar te maken wanneer een bepaalde fout zich voordoet, zonder dat een seriële monitor aangesloten moet worden, wordt er gebruik gemaakt van een led die de verschillende errorcodes weergeeft. 
Deze code worden hieronder opgelijst:

| Wifi error     | kort uit, lang aan  | ![wifi gif](blink_wifi.gif) |
| MQTT error    | lang uit, kort aan    | ![mqtt gif](blink_mqtt.gif)     |
| NFC error     |    kort aan, kort uit    |     ![nfc gif](blink_nfc.gif)       | 


Meer info over deze codes wordt gegeven in [het deel software.](software.md)