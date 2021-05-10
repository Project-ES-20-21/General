---
layout: default
title: Anderhalvemeter
parent: Onderdelen
nav_order: 7
---

# 'T is beter op anderhalve meter
## Algemeen doel
Het doel van dit ontwerp is het verzekeren van een corona-veilige afstand tussen de spelers van de escaperoom. Hierdoor mogen de spelers niet binnen een afstand van 1,5 meter van elkaar komen. Wanneer een kleinere afstand gedetecteerd wordt, worden de nodige signalen verzonden om het spel te pauzeren en de spelers die te dicht staan te laten weten dat ze hun handen moeten gaan ontsmetten.
## Hardware
#### Gebruikte componenten:
| Component                       | type            | gebruik|
| --------------------------------|:---------------:|:------|
| ESP32-WROOM-32UE 4MB FLASH      | micro processor  |<li>verzenden BLE signalen</li><li>RSSI metingen</li><li>controleren van veilige afstand</li><li>communicatie met broker</li>|
| AMS1117-3.3                     | LDO                  |conversie van inputspanning naar een stabiele 3.3V voeding  |
| nog vragen                      | buzzer               |    piept wanneer de speler te dicht bij een andere speler staat  |
|MOLEX 105017-1001                | micro-usb            | voeding vanuit een 5V powerbank/oplader|
|nog te bekijken                  |printkroonsteentje    | voeding vanuit een 9V batterij         |
|nog te bekijken                  | bistabiele schakelaar| keuzeschakelaar voor voeding uit usb versus uit batterij |
|W1049B030                        | omnidirictionele antenne| verzenden en ontvangen van wifi en ble signalen|



#### PCB

[bovenaanzicht van PCB](https://github.com/blijf-weg/PCB_RSSI/blob/main/eerste_versie/bovenaanzicht.png)

[schema van PCB](https://github.com/blijf-weg/PCB_RSSI/blob/main/schema.JPG)
![alt text](https://github.com/blijf-weg/PCB_RSSI/blob/main/schema.JPG)

#### Voeding
Toegestane spanning aan USB poort of batterij: 5V - 12V <br/>
Gemiddele getrokken stroom bij nominale werking met 9V baterij: 134 mA <br/>
Verbruikt vermogen bij een ingangsspanning van 8.82V: 1.18W <br/><br/>
Uit veiligheidsoverwegingen wordt aangeraden om nooit tegelijkertijd een voeding op de USB of batterij aan te sluiten wanneer de programmeerpinnen worden gebruikt.
## Blokschema


## Software

## Communicatie
Alle communicatie verloopt via de broker met mqtt. Er is zowel interne communicatie voor synchronisatie tussen de modules als externe communicatie voor de nodige controlesignalen te versturen en ontvangen van andere elementen in de escape room.
#### interne communicatie
Omdat er geen volledige zekerheid is dat beide speler een overtreding van de afstand meten is er een intern kanaal voor onderlinge communicatie tussen de nodes. Bijvoorbeeld als speler 1 een overtreding meet t.o.v. speler 2 maar omgekeerd niet dan zou alleen speler 1 beginnen met piepen en speler 2 niet. Doordat speler 2 niet weet dat hij/zij een overtreding heeft begaan, weet deze speler ook niet dat hij/zij zijn handen moet ontsmetten en zal de escaperoom vast lopen. Via het kanaal "esp32/afstand/piep/2" (in dit geval is het speler 2) stuurt speler 1 een "1" die speler 2 dan kan verwerken. Zo wordt ervoor gezorgd dat speler 2 ook zeker begint met piepen. 
#### externe communicatie
1. Controle </br>
  De module subscribet op het kanaal "esp32/afstand/control". Op volgende karakters wordt gereageerd:
    * '0': de module zal resetten
    * '1': pauzeer de werking totdat '2' wordt aangekregen
    * '2': hervat de werking van de schakeling
2. Ontsmetten </br>
   Langs het kanaal "esp32/ontsmetten/id" laat de module Afstand_0 weten welke spelers te dicht bij elkaar staan aan de ontsmettingspomp. Dit gebeurt aan de hand van id's gaande van 1 voor de centrale    module tot vier. Het overtredende koppel wordt dan doorgestuurd al een combinatie van de twee id's. Bijvoorbeel wanneer Afstand_0 te dicht komt bij Afstand_1, wordt "12" doorgstuurd.
## Opstelling
De module wordt gedragen door de spelers. Voor een correcte meting is het noodzakelijk dat de antenne bevenop het hoofd gedragen wordt zodat er een minimale verstoring is van het signaal en meting. Via een draad en gaten door de behuizing kunnen de modules aan een faceshield verbonden worden maar ook een pet die achterste voren wordt gedragen is mogelijk.(Op de achterkant van de behuizing kan een plaatjes geschroefd worden, dit laat toe om tussen de behuizing en het plaatje een band zoals bijvoorbeeld een hoofdband of bandje van een petje te klemmen. <br/>
![alt text](https://github.com/blijf-weg/casing/blob/master/montage_hoofdband.JPG)<br/>
Bij de antenne zit een mounting ring meegeleverd. Deze ring past op de mounting hole op de bovenkant van het deksel. Zorg ervoor dat de antenne loodrecht naar boven wijst.<br/>
![alt text](https://github.com/blijf-weg/casing/blob/master/antenne_mounting.JPG)<br/>

## Locatiebepaling (optioneel)
Als uitbreiding kan locatiebepaling in de ruimte via trilateratie worden toegevoegd. Deze uitbreiding is niet noodzakelijk voor een correcte werking. Er is voor deze uitbreiding geen andere hardware of PCB nodig. Er zijn wel drie extra PCB's nodig, de opstelling wordt uitgebreid. Het blokschema, de gebruikte software en de interne communicatie verlopen wel anders en moeten worden uitgebreid. De code is voor elke ESP ook niet hetzelfde. 
### Blokschema

### Software
Code voor Afstand_0: </br>
Code voor Afstand_1, Afstand_2 en Afstand_3: </br>
Code voor Afstand_A, Afstand_B en Afstand_C: </br>
### interne communicatie
Eén module fungeert als het centrale controle orgaan. Bij conventie krijgt deze de naam Afstand_0. Andere modules heten dan Afstand_1, Afstand_2 en Afstand 3.
Elke module stuurt zijn coördinaten door via mqtt kanalen. Voor elke coördinaat bestaat er een apart kanaal, dit kanaal begint met "esp32/afstand/" en dan de naam van de specifieke coördinaat. Bijvoorbeeld de x-coördinaat van Afstand_1 wordt verzonden om het kanaal "esp32/afstand/x1", de y-coördinaat op "esp32/afstand/y1".

### Opstelling
De opstelling wordt uitgebreid met drie modules, deze drie extra modules zijn de drie trilateratie nodes zoals eerder vermeld in code. In de hoeken van de kamer worden deze nodes bevestigd op 1,80 meter (een schatting van de gemiddelde lengte van de kandidaten). Met de drie nodes wordt een assenstelsel gebouwd, de coordinaten van de nodes in dit assenstelsel worden aangepast in de code in de klasse "Metingen.h". De rest van de opstelling is identiek hetzelfde.

