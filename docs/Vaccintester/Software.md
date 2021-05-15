---
layout: default
title: Software
parent: Vaccintester
grand_parent: Onderdelen
nav_order: 2
---

# Software

## Inhoud
- [Code](#Code)
    - [Code NFCsender](#Code NFCsender)
    - [Code Button](#Code Button)
    - [Code Ledstrip](#Code Ledstrip)

- [Communicatie](#Communicatie)

## Code
Aangezien we werken met 3 verschillende PCB's moest voor elk hiervan verschillende code geschreven worden. Deze werken allemaal met een ESP32 module en kunnen geprogrammeerd worden via Arduino C/C++. De code van elke deel kan teruggevonden worden op de algemene github van dit onderdeel van de Escape room.

### Code NFCsender
Het eerste PCB dat in de schakeling gebruikt wordt is dat van de kast. Onze code is opgedeeld in 3 grote delen, namelijk de methodes om de NFC-reader op te zetten, de methodes om de MQTT communicatie te implementeren en tenslotte de setup- en loopmethodes.

In dit deel van de proef moet met een RFID tag gescand worden aan een PN532 board om de een ledsequentie te laten oplichten. Dit kan ook gebruikt worden om de hele puzzel manueel te resetten met behulp van een andere kaart.

In de loop-methode wordt steeds gekeken of er een kaart is voorgelegd aan de reader en of er een MQTT-message op één van de gevolgde kanalen is gezet. Indien de juiste kaart is voorgelegd wordt er zelf data op vershillende kanalen gezet.




### Code Button

### Code Ledstrip


## Communicatie

