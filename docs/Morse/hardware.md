---
layout: default
title: Hardware
parent: Morse
grand_parent: Onderdelen
nav_order: 2
---

# Hardware
## Speaker

## Micro
De documentatie van de printplaat kan [hier](https://github.com/BachMorse/Micro_PCB) gevonden worden. De printplaat ziet eruit als volgt:

Voorkant: 
![](https://raw.githubusercontent.com/BachMorse/Documentatie/master/PCB_voorkant.JPG)

Achterkant:
![](https://raw.githubusercontent.com/BachMorse/Documentatie/master/PCB_achterkant.JPG)

### ESP32
Het centrale element in ons project is een ESP32. Verschillende pinnen voorzien we van pull-up weerstanden van 10k en/of ontladingscondensatoren van 100nF. De 3.3V voeding voorzien we van een ontladingscondensator. De 2 buttons (enable en boot) worden voorzien van zowel ontladingscondensatoren, als pull-up weerstanden. Ook GOI5-pin wordt voorzien van een pull-up weerstand, aangezien deze heel de tijd HIGH moet zijn gedurende de boot. Ook de Tx-pin moet hoog blijven, dus ook deze wordt voorzien van een pull up weerstand. Het LED-lichtje dat aan pin 12 hangt, is een controle-LED die gebruikt kan worden om te zien of de ESP32 goed werd gesoldeerd. De andere componenten worden verder besproken.
![](https://raw.githubusercontent.com/BachMorse/Documentatie/master/schakeling_ESP32.JPG)

### Low Drop-Out regulator
Zoals gezegd wordt de ESP32 gevoed met een spanning van 3.3V. De display daarentegen moet gevoed worden met 5V. Om 5V te geven aan de elementen die het nodig hebben, maken we dus gebruik van een micro-USB en een powerbank om de volledige PCB-plaat te kunnen voeden. De micro-USB wordt rechtstreeks gekoppeld met de display. Voor de andere elementen die gebruik maken van 3.3V wordt een LDO (Low Drop-Out regulator) voorzien. Via een interne transistor, een MOSFET en 2 weerstanden, zet een LDO 5V om naar 3.3V.
Zoals te zien op de schakeling, heeft de micro-USB meerdere mogelijke pinnen. Deze zouden kunnen gebruikt worden voor verzenden (Tx) en ontvangen (Rx) van data. Omdat de ESP32 in principe maar 1 keer moet geprogrammeerd worden, volstaat het om een program pin header toe te voegen, in combinatie met een UART bridge.
![](https://raw.githubusercontent.com/BachMorse/Documentatie/master/schakeling_2voeding.JPG)

### Levelshifters
De outputpinnen van de ESP32 kunnen maximum 3.3V doorgeven. Om voldoende sterke signalen te geven aan de 2 displaypinnen (SDA en SCL), moeten dus levelshifters worden voorzien. Deze levelshifters zullen 3.3V vanuit de ESP32 omzetten naar 5V voor de display. 
![](https://raw.githubusercontent.com/BachMorse/Documentatie/master/schakeling_levelshifters.JPG)

### Pin headers
De verschillende pin headers zorgen voor de aansluiting van de externe componenten. We gebruiken vrouwtjes zodat de pinnen niet kunnen plooien als de PCB verplaatst wordt. 
* De display maakt gebruik van I2C communicatie. Hiervoor maken we gebruik van de SDA- en SCL-poorten. De SCL-poort zorgt voor de transmissie van het kloksignaal. Via de SDA-poort sturen we data van en naar de ESP32, vanuit de display. Deze 2 poorten zijn active low. Daarom werden pull-up weerstanden van 4k7 voorzien op het display zelf (ze dienen dus niet toegevoegd te worden op de zelfgemaakte display).
* De program header wordt, naast Tx en Rx kanalen, voorzien van een voeding. Deze aansluiting kan gebruikt worden wanneer de micro-USB nog niet werd gesoldeerd op de PCB-plaat. Verder hebben deze 2 pinnen geen functie.
* Ook voorzien we pinheaders voor een button, deze button is niet enorm stabiel, het debounced bij het indrukken en loslaten, dit wordt softwarematig opgelost.
* De laatste pinheader zal zorgen voor de connectie tussen de microfoon en de ESP32. De microfoon wordt voorzien van een potentiometer. Hoe lager de weerstand van de potentiometer, hoe gevoeliger de mcirofoon zal zijn.
![](https://raw.githubusercontent.com/BachMorse/Documentatie/master/schakeling_headers.JPG)



