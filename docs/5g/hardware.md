---
layout: default
title: Hardware
parent: 5G
grand_parent: Onderdelen
nav_order: 2
---

- [Auto](#Auto)
  - [Afstandsbediening](#Afstandsbediening)
  - [De deur](#De-deur)
  - [Voeding](#Voeding)
  - [Anti tamper](#Anti-tamper)
  - [Motoren](#Motoren)
  - [PCB ontwerp](#PCB-ontwerp)
  - [Omhulsel](#Omhulsel)
 -[Led-bar](#Led-bar)
 -[Stralingslocatie](#Stralingslocatie)

# Auto
## Afstandsbediening
Voor het besturen van de auto wordt er gebruik gemaakt van een afstandsbediening en ontvanger dat communiceert over 433Mhz.
Het besturen van de auto gebeurt random. Dus een knop moet niet specifiek gekoppeld worden aan een bepaald signaal. We maken gebruik van 4 pinnen van de ontvanger voor het bepalen of er een knop wordt ingedrukt en welke. De interrupt op de ontvanger is zo ingesteld dat deze hoog is en blijft zodra 1 knop wordt ingedrukt. De ontvanger voorziet een interrupt signaal en een signaal per drukknop. Zoals eerder gezegd kan als de interrupt zodanig wordt ingesteld. 1 Van de signalen voor de drukknoppen weggelaten worden. Omdat we via uitsluitsel weten dat heet de 4de is.
## De deur
De verborgen compartiment in de auto kan geopend worden door het indrukken van een drukknop als de rssi waarde een bepaalde grens onderscheid. Het geraamte van de auto waarin deze deur voorzien is gelazer cut. Het open van de deur gebeur met behulp van een solenoid van 12V. Deze wordt geschakeld via een relais van 5V. Het aansturen van de relais gebeurt via een optocoupler dit heeft als voordeel dat wanneer de relais een puls genereert bij het terugschakelen deze de esp niet kan beschadigen.  
##Voeding
De auto wordt gevoed door een LiPo batterij van 12V met 2400mAh. Deze spanning wordt omgevormd met een Buck converter naar 5V Deze spanning wordt gebruikt voor de afstand sensor, level shifters, … . De spanning wordt vanaf ook nog naar 3.3V gebracht met behulp van een LDO (AMS1117). Deze laatste spanning wordt gebruikt voor het voeden van de esp.
## Anti tamper
Omdat we willen vermijden dat de auto kan opgenomen worden en zo verplaatst worden naar de juiste locatie. Maken we gebruik van een afstand sensor deze meet constant de afstand onder de auto. Wanneer deze afwijkt zal er een buzzer beginnen piepen tot de auto wordt neergezet. Eenmaal neergezet zal de buzzer nog even verder gaan als tijdstraf. De buzzer is een active buzzer dus moet geschakeld worden met een dc spanning van 5V. Omdat dit een verbruiker is en geen stuursignaal maken we gebruik van een N-mosfet voor het schakelen. 
## Motoren
De motoren (FIT0441)  hebben een ingebouwde sturing en kunnen gestuurd worden via een PWM signaal en een stuursignaal dat hoog of laag is afhankelijk van de gewenste richting. We maken gebruik van mecanum wielen deze zijn in staat om in alle richtingen te rijden. Zoals weergegeven op onderstaande afbeelding. 
Omdat we dit niet nodig hebben en beperkt zijn tot 4 knoppen van de afstandsbediening. Graag hebben we ook dat de wielen kunnen bestuurt worden met zo weinig mogelijk pinnen. Indien we kiezen voor A en E en hun inverse hebben de wielen link en recht steeds dezelfde richting en hebben alle wielen steeds dezelfde snelheid. Dit maakt dat we alle wielen kunnen besturen met 3 signalen. 
Het PWM signaal kan gegeneerd met ledcWrite deze laat ons toe om de frequentie en duty-cycle aan te passen.
## PCB ontwerp
![autopcbupper](https://github.com/5Gstraling/autopcb/blob/master/autopcbupper.png)
![autopcbbottom](https://github.com/5Gstraling/autopcb/blob/master/autopcbupperbottem.png)
Het volledige project (Kicad) is terug te vinden [hier](https://github.com/5Gstraling/autopcb).
## Omhulsel
[Afbeelding]
Het volledige project () is terug te vinden [hier]().
# Led-bar
De lebar wordt gevoed door een powerbank. Dit omdat de lebar 8 leds heeft van 20mA en zo het stroomverbruik een stuk hoger ligt dan bij de stralingslocatie. Van Hardware is hier niets speciaal gebruikt enkel de al vermelde leds en natuurlijk hun voorschakelweerstand. Het is wel op te merken dat niet alle kleuren kunnen gebruikt worden van leds aangezien de esp32 maar een spanning op de gpi pinnen kan aanleggen van 3.3V. En bij sommige kleuren ligt de drempelspanning hoger. 

# Stralingslocatie
De stralingslocatie is een vereenvoudigde versie van de lebar( zonder alle aansluitingen voorzien voor de leds). De stralingslocatie wordt gevoed door 4 AA batterijen. Aangezien de esp32 enkel gebruikt wordt voor een wifi signaal uit te zenden is het stroomverbruik niet zo groot en volstaan batterijen. 
