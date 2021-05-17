---
layout: default
title: Hardware
parent: Ontsmetting
grand_parent: Onderdelen
nav_order: 2
---



# PCB
De files van ons volledige PCB ontwerp binnen Kicad kunnen via [volgende link](https://github.com/Ontsmettinator3000/PCB) gevonden worden:


## 3D render PCB

![](https://github.com/Ontsmettinator3000/main/blob/main/docs/PCB_3DviewBOTTOM.png?raw=true)
![](https://github.com/Ontsmettinator3000/main/blob/main/docs/PCB_3DviewTOP.png?raw=true)

## Schema PCB

![](https://github.com/Ontsmettinator3000/main/blob/main/docs/Printing%20Print%20schema.pn.png?raw=true)

# Componenten
## Voeding
De PCB wordt gevoed door een powerbank verbonden via een micro-usb poort. Hierdoor krijgt de PCB een 5 volt spanningsniveau waarmee de versterker en pomp gevoed worden. De ESP-32, het lcd-scherm, de infrarood sensor en de NFC-module worden gevoed door 3.3 volt. Hierdoor werd een AMS1117-3.3 LDO gebruikt die het 5 volt niveau naar 3.3 volt verlaagt. We kozen voor een powerbank als voeding omdat deze het gewenste 5volt niveau geeft. Een powerbank heeft ook een grote capaciteit waardoor deze niet vaak opgeladen moet worden. Verder is deze ook gebruiksvriendelijk in het opladen en makkelijk te vervangen. Naast de micro-USB connector werd een schakelaar bevestigd om de mogelijkheid te geven om de pcb volledig uit te schakelen.

## Programmer

Om de ESP32 microcontroller te programmeren is een program header aanwezig die verbonden is met de TX en RX poorten van de ESP32. Deze zijn respectievelijk de transmitter en receiver bij seriële communicatie. Via een een USB naar Serial UART-bridge kan verbinding gemaakt worden. Het gebruik van seriele connectie werd gekozen boven het programmeren via micro usb omdat het programma de mogelijkheid heeft om OTA geprogrammeerd te worden. De aanwezigheid van een complexere verbinding via micro usb waar de usb naar seriele omzetting op de PCB zou moeten gebeuren was dus te complex om maar één keer te gebruiken. Op de PCB is ook een led aanwezig die gebruikt werd om de functionaliteit van de pcb te testen bij solderen. Deze led werd verder ook gebruikt om de verschillende foutcode visueel door te geven.

## NFC sensor

Als modus van identificatie wordt er gebruik gemaakt van een RFID NFC-lezer. Specifiek wordt er gebruik gemaakt van een PN532 RFID module. Deze kan gebruik maken van SPI, I2C of UART. Onze ESP communiceert via I2C. I2C maakt gebruik van twee buslijnen, SDA en SCL. SCL is de klok en SDA de datalijn. De ESP is in dit geval de master terwijl de NFC-lezer de slave is. Deze lijnen zijn aanwezig op onze PCB. Ook is er een reset, interrupt, Vcc en ground verbinding aanwezig. 

## Pomp

Om het ontsmettingsmiddel te pompen maken we gebruik van een verticale onderwaterpomp die met 5V gevoed wordt. De pomp wordt aangestuurd door een Mosfet wat op een abnormale manier werd uitgevoerd aangezien de mosfet naar de massa schakelt. De Pomp wordt voor een bepaalde tijdsduur ingeschakeld zodat de juiste hoeveelheid ontsmettingsmiddel kan verkregen worden. Ten slotte werd er ook een flyback diode toegevoegd om eventuele inverse stromen tegen te houden.

## Hand sensor

Als hands-free sensor maken we gebruik van een infrarood breakbeam sensor. Deze sensor gebruikt een IR-zender en receiver. Wanneer de verbinding tussen de twee wordt verbroken zal er een signaal worden verzonden. Door de zender en receiver parallel naast elkaar te plaatsen kan de sensor ook als afstandssensor gebruikt worden. Wanneer een object het IR licht weerkaatst tot de receiver zal er ook een signaal gegeven worden. Aan de uitgang werd een pull-up weerstand toegevoegd voor correcte werking. Door de simpele uitvoering van deze IR-sensor werd deze verkozen boven een ultrasone afstandssensor.

## LCD scherm

Om visuele instructies te geven aan de gebruiker wordt gebruik gemaakt van een TFT-LCD scherm. Deze communiceert via SPI, hierbij wordt een full duplex verbinding gemaakt. Er worden bij SPI vier verbindingen gebruikt. MOSI stuurt data van master naar slave terwijl MISO het omgekeerde doet. CS, chip select, bepaalt met welke slave gecommuniceerd wordt. Tenslotte is er ook nog een seriële klok nodig. Er is ook een enable ingang die gebruikt wordt om het scherm uit te schakelen wanneer het niet nodig is om er iets op te zien. Dit zorgt voor een kleiner verbruik.

## Versterker

Om het audiosignaal af te spelen is een PAM8403 versterker aanwezig op de PCB. Dit is een filterloze klasse D versterker die een analoog signaal afkomstig van de ESP-32 versterkt en omzet naar PWM. Een klasse D versterker maakt gebruik van de schakelende werking van transistoren of MOSFETs. Binnen duurdere klasse D versterkers wordt gebruik gemaakt van een laagdoorlaatfilter om de hoge frequenties van het PWM signaal terug weg te filteren. Goedkopere versterkers zoals de PAM8403 werken echter zonder LDF en rekenen op de inductantie van de speaker om de hoogfrequente componenten weg te halen. Aan de chip zijn 2 ingangspoorten aanwezig waarbij elk 2 uitgangspoorten horen, slechts één van deze in- en uigangskoppels werd gebruikt. Ook bevat deze een mute en shutdown poort. Deze zijn active low. De shutdown pin wordt gebruikt om de speaker uit te schakelen wanneer deze niet gebruikt moet worden, dit vermindert de ruis.

In het finale ontwerp was helaas een ground loop aanwezig waardoor het versterkercircuit geen correcte audiofragmenten af kon spelen. Omdat de massa onregelmatig was had de versterker geen correct referentieniveau en kon er enkel een ruis afgespeeld worden. Klasse D versterkers zijn extra hier gevoelig voor wanneer deze schakelen en een deel van de schakeling groot genoeg is om als antenne te dienen. Het scheiden van de power ground en analoge ground zou hier een oplossing kunnen bieden maar deze scheiden zou de PCB vernietigd hebben. De ground loop in de schakeling kon niet opgelost worden ondanks het weghalen van vele vias, toevoegen van extra ontkoppelcapaciteiten en vervangen van alle onderdelen. Als alternatief werd er gebruik gemaakt van een externe bestaande versterkerschakeling. De capaciteiten op dit bord kunnen het massaniveau genoeg stabiliseren waardoor het audiofragment correct kan worden afgespeeld.

