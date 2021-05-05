layout: default
title: Hardware
parent: Ontsmetting
nav_order: 2
---

# Hardware

Zoals vooraf vermeld deelden we het project op kleinere onderdelen. Hieronder zullen we deel per deel toelichten. Op het einde geven we een totale beschrijving van alle onderdelen in het geheel.

## Voeding

De PCB wordt gevoed door een powerbank verbonden via een micro-usb poort. Hierdoor krijgt de PCB een 5 volt spanningsniveau waarmee de versterker en pomp gevoed worden. De ESP-32, het lcd-scherm, de infrarood sensor en de NFC-module worden gevoed door 3.3 volt. Hierdoor werd een LDO gebruikt dit het 5 volt signaal naar 3 volt verlaagt. We kozen voor een powerbank als voeding omdat deze het gewenste 5volt niveau geeft. Een powerbank heeft ook een grote capaciteit aardoor deze niet vaak opgeladen moet worden.

## NFC sensor

Als modus van identificatie wordt er gebruik gemaakt van een RFID NFC-lezer. Specifiek wordt er gebruik gemaakt van een PN532 RFID module. Deze kan gebruik maken van SPI, I2C of UART. Onze ESP communiceert via I2C. I2C maakt gebruik van twee buslijnen, SDA en SCL. SCL is de klok en SDA de datalijn. De ESP is in dit geval de master terwijl de NFC-lezer de slave is. Deze lijnen zijn aanwezig op onze PCB. Ook is er een reset, interrupt, Vcc en ground verbinding aanwezig. 

## Pomp

Om het ontsmettingsmiddel te pompen maken we gebruik van een onderwaterpomp van 3 tot 6 Volt. De pomp wordt aangestuurd door een Mosfet. Deze werd wel op een abnormale manier uitgevoerd aangezien de mosfet naar de ground schakelt. De Pomp wordt door een PWM-signaal aangestuurd zodat de juiste hoeveelheid ontsmettingsmiddel kan verkregen worden. Ten slotte werd er ook een flyback diode toegevoegd om eventuele inverse stromen tegen te houden.

## Hand sensor

Als hands-free sensor om ontsmetting te dispensen wordt gebruik gemaakt van een infrarood breakbeam sensor. Deze sensor gebruikt een IR-zender en receiver. Wanneer de verbinding tussen de twee wordt verbroken zal er een signaal worden verzonden. Door de zender en receiver parallel naast elkaar te plaatsen kan de sensor ook als afstandssensor gebruikt worden. Wanneer een object het IR licht weerkaatst tot de receiver zal er ook een signaal gegeven worden. Aan de uitgang werd een pull-up weerstand toegevoegd voor correcte werking.

## LCD scherm

Om visuele instructies te geven aan de gebruiker wordt gebruik gemaakt van een TFT-LCD scherm. Deze werkt via SPI, hier wordt een full duplex verbinding gemaakt. Er worden bij SPI vier verbindingen gebruikt. MOSI stuurt data van master naar slave terwijl MISO het omgekeerde doet. CS, chip select, bepaalt met welke slave gecommuniceerd wordt. Tenslotte is er ook nog een seriele klok nodig. 