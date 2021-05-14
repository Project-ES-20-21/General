---
layout: default
title: Morse
parent: Onderdelen
nav_order: 8
has_children: true
---

# Morse

## Doel en verhaal
Wanneer er voldoende energie werd opgewekt bij de vorige puzzel [fitnesstracker](https://github.com/Project-ES-20-21/General/tree/gh-pages/docs/Fitnesstracker), wordt ook meteen duidelijk dat de Russen via morse code belangrijke informatie doorsturen. Deze informatie kan ook gebruikt worden bij de klinische testen van ónze virologen. Deze klinische testen vinden zogezegd plaats in een ruimte naast de escape room. Daarom moeten ze de morse informatie doorgeven aan de onderzoekers van de klinische testen.

De morse sequentie komt uit de speaker. Wanneer de sequentie juist werd nagefloten, zal er op een display een cijfer verschijnen én een tip voor de volgende proef. Het cijfer kan gebruikt worden bij de Alohomora-puzzel.

De bedoeling van onze puzzel is om een uitgezonden morse code na te fluiten aan de andere kant van de kamer. Een microfoonsensor zal dit geluid opvangen en het zal nagaan of het nagebootste gefluit overeenkomt met het uitgezonden ritme van de speaker. Deze communicatie gebeurt over de broker via MQTT.

Om aan de spelers te laten weten wat er juist moet gebeuren zal er een blad naast de micro liggen waarop, deels in morse, de uitleg staat.

## Blokschema
Zoals te zien op het blokschema wacht onze volledige puzzel op een signaal van de vorige puzzel. Vanaf dan wordt onze puzzel opgestart. Signalen tussen de microfoon en de speaker worden vanaf dan verstuurd over de broker. 
![](https://raw.githubusercontent.com/BachMorse/Documentatie/master/BlokschemaMorse.JPG)

## Communicatie
Eerst en vooral moet er gecommuniceerd worden met de vorige puzzel, de fitnesstracker, en met de volgende puzzel, 5G-proef. Wanneer de opdracht van de fitnesstracker werd volbracht, mag de speaker beginnen met het uitzenden van de morse code.
Via het kanaal "esp32/fitness/telefoon" wordt het signaal "BEL" gestuurd. Zo weet de speaker dat het geluid mag beginnen spelen.

Wanneer de spelers klaar zijn met onze proef, zal er een start signaal gegeven worden naar de afstandsbediening van de 5G-proef. Hiervoor wordt het "morse_einde" doorgestuurd via het kanaal "esp32/morse/output".

Daarnaast wordt over de broker ook de juiste morse-sequentie doorgegeven door de speaker aan de microfoon. Hierdoor weet de microfoon met welke sequentie het ontvangen fluitsignaal moet vergeleken worden. Het kanaal waarover we hiervoor communiceren is "esp32/morse/intern".

En als laatste zorgt de broker ook voor een start-, pauze- en stopsignaal. Wanneer we een pauzesignaal ontvangen, zal de button van de microfoon niet werken, de speaker zal stoppen met geluid uitzenden.

## Opstelling
De speaker wordt geïmplementeerd in een 3D-geprinte telefoon. De verdere hardware voor deze telefoon wordt weggestoken in een box die werd gelasercut. We voorzien in deze behuizing ook plaats voor een button.
Voor de microfoon maakten we ook gebruik van de lasercutter. In deze behuizing voorzien we plaats voor de button, de display en de microfoon. Voor de 5V voeding van beide delen, is een powerbank nodig.
De plaats van onze onderdelen is niet echt van belang, aangezien het kleine beweegbare onderdelen zijn. Door de 2 onderdelen (speaker en micro) ver uit elkaar te zetten, en vast te maken, wordt de puzzel wel moeilijker. 

## Mogelijke errors
Mogelijke errors zijn debouncing van de button. In principe is dit geen probleem als de speler niet al te snel na het indrukken begint te fluiten. Ook zal het LCD-scherm blijven staan wanneer de juiste code wordt nagefloten. Het bouncen van de button heeft daarom geen effect op de zichtbaarheid van het Alohomoracijfer en de tip voor de volgende puzzel.

Een andere factor die fout kan veroorzaken is het achtergrondgeluid. Om dit tegen te gaan, werkten we met een button die ingedrukt moet worden tijdens het fluiten. Zo weet de microfoon dat het ontvangen geluid gefluit zal zijn. Als het relatief stil is op de achtergrond, heeft de microfoon geen moeite om achtergrondgeluid te onderscheiden van het gefluit. Enkel wanneer er veel geluid is op de achtergrond, kan het zijn de microfoon foute signalen ontvangt.
