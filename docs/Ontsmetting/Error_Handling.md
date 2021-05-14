layout: default
title: ErrorHandling
parent: Ontsmetting
grand_parent: Onderdelen
nav_order: 5
---

# Error handling

Wanneer zich een fout voordoet kan de ESP-32 softwarematig gereset worden. Dit gebeurt via een MQTT bericht. De ESP-32 connecteert automatisch met het topic esp32/ontsmetten/control.Wanneer het bericht '0' op dit topic geplaatst wordt zal deze zich resetten via de functie esp_restart.

Om visueel zichtbaar te maken wanneer een bepaalde fout zich voordoet zonder dat een seriële monitor aangesloten moet worden wordt er gebruik gemaakt van een led die de verschillende errorcodes weergeeft. Meer info over deze codes wordt gegeven in [het deel software.](software.md) 