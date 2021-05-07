---
layout: default
title: Hardware
parent: Ontsmetting
grand_parent: Onderdelen
nav_order: 2
---



# PCB


![](https://github.com/Ontsmettinator3000/main/blob/main/docs/PCB_3DviewBOTTOM.png?raw=true)
![](https://github.com/Ontsmettinator3000/main/blob/main/docs/PCB_3DviewTOP.png?raw=true)
## Voeding

De PCB wordt gevoed door een powerbank verbonden via een micro-usb poort. Hierdoor krijgt de PCB een 5 volt spanningsniveau waarmee de versterker en pomp gevoed worden. De ESP-32, het lcd-scherm, de infrarood sensor en de NFC-module worden gevoed door 3.3 volt. Hierdoor werd een AMS1117-3.3 LDO gebruikt die het 5 volt niveau naar 3.3 volt verlaagt. We kozen voor een powerbank als voeding omdat deze het gewenste 5volt niveau geeft. Een powerbank heeft ook een grote capaciteit waardoor deze niet vaak opgeladen moet worden. Verder is deze ook gebruiksvriendelijk in het opladen en makkelijk te vervangen.

## Programmer

Om de ESP32 microcontroller te programmeren is een program header aanwezig. Via een een USB naar Serial UART-bridge kan verbinding gemaakt worden. 

## NFC sensor

Als modus van identificatie wordt er gebruik gemaakt van een RFID NFC-lezer. Specifiek wordt er gebruik gemaakt van een PN532 RFID module. Deze kan gebruik maken van SPI, I2C of UART. Onze ESP communiceert via I2C. I2C maakt gebruik van twee buslijnen, SDA en SCL. SCL is de klok en SDA de datalijn. De ESP is in dit geval de master terwijl de NFC-lezer de slave is. Deze lijnen zijn aanwezig op onze PCB. Ook is er een reset, interrupt, Vcc en ground verbinding aanwezig. 

## Pomp

Om het ontsmettingsmiddel te pompen maken we gebruik van een verticale onderwaterpomp die met 5V gevoed wordt. De pomp wordt aangestuurd door een Mosfet. Deze werd wel op een abnormale manier uitgevoerd aangezien de mosfet naar de ground schakelt. De Pomp wordt door een PWM-signaal aangestuurd zodat de juiste hoeveelheid ontsmettingsmiddel kan verkregen worden. Ten slotte werd er ook een flyback diode toegevoegd om eventuele inverse stromen tegen te houden.

## Hand sensor

Als hands-free sensor om ontsmetting te dispensen wordt gebruik gemaakt van een infrarood breakbeam sensor. Deze sensor gebruikt een IR-zender en receiver. Wanneer de verbinding tussen de twee wordt verbroken zal er een signaal worden verzonden. Door de zender en receiver parallel naast elkaar te plaatsen kan de sensor ook als afstandssensor gebruikt worden. Wanneer een object het IR licht weerkaatst tot de receiver zal er ook een signaal gegeven worden. Aan de uitgang werd een pull-up weerstand toegevoegd voor correcte werking. Door de simpele uitvoering van deze IR-sensor werd deze verkozen boven een ultrasone afstandssensor.

## LCD scherm

Om visuele instructies te geven aan de gebruiker wordt gebruik gemaakt van een TFT-LCD scherm. Deze werkt via SPI, hier wordt een full duplex verbinding gemaakt. Er worden bij SPI vier verbindingen gebruikt. MOSI stuurt data van master naar slave terwijl MISO het omgekeerde doet. CS, chip select, bepaalt met welke slave gecommuniceerd wordt. Tenslotte is er ook nog een seriele klok nodig. 

## Versterker

Om het audiosignaal af te spelen is een PAM8403 versterker aanwezig op de PCB. Dit is een filterloze klasse D versterker die een analoog signaal afkomstig van de ESP-32 versterkt en omzet naar PWM. Een klasse D versterker maakt gebruik van de schakelende werking van transistoren of MOSFETs. Binnen duurdere klasse D versterkers wordt gebruik gemaakt van een laagdoorlaatfilter om de hoge frequenties van het PWM signaal terug weg te filteren. Goedkopere versterkers zoals de PAM8403 werken echter zonder LDF en rekenen op de inductantie van de speaker om de hoogfrequente componenten weg te halen.

In het finale ontwerp was helaas een ground loop aanwezig waardoor het versterkercircuit geen correcte audiofragmenten af kon spelen. Omdat de massa onregelmatig was had de versterker geen correct referentieniveau en kon er enkel een ruis afgespeeld worden. De ground loop in de schakeling kon niet gevonden worden ondanks het weghalen van vele vias, toevoegen van extra ontkoppelcapaciteiten en vervangen van alle onderdelen. Als alternatief werd er gebruik gemaakt van een externe bestaande versterkerschakeling. De capaciteiten op dit bord kunnen het massaniveau genoeg stabiliseren waardoor het audiofragment correct kan worden afgespeeld.

