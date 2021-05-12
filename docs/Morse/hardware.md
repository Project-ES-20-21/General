---
layout: default
title: Hardware
parent: Morse
grand_parent: Onderdelen
nav_order: 2
---

## Hardware
### Speaker

### Micro
Op de PCB-plaat voorzien we een aansluiting voor een externe display, een externe microfoon en een externe button. Deze 3 elementen worden via pinheaders aangesloten. Om de ESP te programmeren, wordt een extra pinheader voorzien. Aangezien de ESP32 maar 1 keer geprogrammeerd moet worden, is dit de meest efficiënte manier. 

De ESP32 zal gevoed worden met een powerbank van 5V. Deze 5V is nodig om de display aan te sturen. Via de LDO zal 5V omgezet worden naar 3.3V. De program header, de microfoon en de ESP32 worden aangesloten aan de 3.3V.

De enable butten en reset button worden voorzien van een pull-up weerstand, net zoals (?)de transmit-pin en de VSPI-pin (GIO5)(?).Vaak wordt ook een pull-up weerstand van 4k7 voorzien voor SCL en SDA aansluiting. Deze is al voorzien in de externe display, de MH-sensor (? sensor?).

De display maakt gebruik van I2C communicatie. Hiervoor maken we gebruik van de SDA- en SCL-poorten. Via de SDA-poort sturen we data van en naar de display, vanuit de ESP32. Deze 2 poorten zijn active low. Daarom werden pull-up weerstanden voorzien op het display zelf. De pull-up weerstanden moeten dus niet op onze eigen PCB aangesloten worden.

Het centrale element, ESP32, voorzien we, naast de verschillende componenten die nog volgen, ook van een LEDlichtje. Hiermee kunnen we gemakkelijk controleren of het solderen gelukt is en of de ESP werkt.
<p align="center">
  <img src=https://user-images.githubusercontent.com/78847177/115971593-9707d280-a549-11eb-82d4-021c228c60fe.png>
</p>

De micro-USB voorziet een voltage van 5 volt. De andere pinnen worden niet gebruikt. De programmatie van de ESP32 gebeurt namelijk via een pinheader. Deze 5V wordt gebruikt om de display te voeden. Via MOSFET?? wordt deze 5 volt omgezet naar 3.3V om de ESP32 met de juiste spanning te voeden, om de pinheaders met de juiste spanning te voeden, en om de micro met de juiste spanning te voeden.
<p align="center">
  <img src=https://user-images.githubusercontent.com/78847177/115971624-c9b1cb00-a549-11eb-8010-be407baed09c.png>
</p>

De level shifters verhogen de voltage van 3.3 volt, afkomstig van de ESP32 naar 5 volt. Deze 5V is nodig om de display op de juiste voltage te kunnen aansturen via SCL- en SDA-poorten.
<p align="center">
  <img src=https://user-images.githubusercontent.com/78847177/115971693-2e6d2580-a54a-11eb-963d-52924a602175.png>
</p>


De verschillende pin headers zorgen voor de aansluiting van de externe componenten. We gebruiken mannetjes zodat de pinnen niet kunnen plooien als de PCB verplaatst wordt.
<p align="center">
  <img src=https://user-images.githubusercontent.com/78847177/115971717-62e0e180-a54a-11eb-96f8-1c58a65d20aa.png>
</p>

De verschillende externe componenten zijn de display. ... Daarnaast maken we ook gebruik van een button, deze button is niet enorm stabiel, het debounced bij het indrukken en loslaten. Voor deze puzzel is het geen stoorfactor, de button is namelijk snel genoeg stabiel om zijn functie te kunnen uitvoeren. Als derde externe component maken we, uiteraard, gebruik van een micro. Deze microfoon heeft een potentiometer die we kunnen aanpassen naar de gewenste gevoeligheid.

De [uiteindelijke printplaat](https://github.com/BachMorse/Micro_PCB) ziet eruit als volgt:
<p align="center">
  <img src=https://user-images.githubusercontent.com/78847177/116796141-7e179800-aada-11eb-9f48-b499f2575616.png)
</p>

Voor de duidelijkheid laten we het grondvlak even achterwege. Zoals te zien plaaststen we de ontladingscondensatoren zo dicht mogelijk bij de ESP32.

