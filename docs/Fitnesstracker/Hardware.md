---
layout: default
title: Hardware
parent: Fitnesstracker
grand_parent: Onderdelen
nav_order: 2
---
# Hardware

## Inhoud
- [Kettlebell] (#Kettlebell)
- [LCD] (#LCD)
## Kettlebell
De hardware van dit deel van de puzzel bestaat uit twee delen. 
Het eerste deel bestaat uit een gewicht waarmee de fitness oefeningen worden uitgevoerd. 
In dit project wordt gebruik gemaakt van een kettlebell van 4kg.

![Kettlebell](Kettlebell.jpg ) 

Het tweede deel van de hardware is de PCB die de oefeningen meet. Een eerste belangrijke component die we terug vinden op de PCB is een esp32. Deze zal worden gevoed met een compacte lipo batterij die een spanning van 3,7 volt levert. Aangezien de esp32 werkt op een spanning van 3,3 volt zal hiertussen een LDO (AMS1117-3.3) geschakeld worden die de spanning omzet naar 3,3 volt. Een tweede belangrijke component is de bewegingssensor. Hiervoor hebben we de MPU-9250 gebruikt. Deze heeft een voeding van 3,3 volt - 5 volt nodig en kan dus rechtstreeks aangesloten worden op de 3,3 volt van de esp (pin vcc). De GND van de sensor kan ook gewoon aan de ground van de esp32 aangesloten worden. Verder moeten ook nog de SCL- en SDA-pin aan de esp32 verbonden worden. Over deze lijnen worden respectievelijk het kloksignaal en de data uitgewisseld. Naast deze twee belangrijke componenten, zijn ook nog twee componenten voorzien die handig zijn bij het gebruik van de PCB: een buzzer en een extra drukknop. De + zijde van de buzzer is verbonden met pin 5 van de esp32 en de - zijde met de ground. De extra drukknop is aan de ene kant aangesloten op de 3,3 volt van de esp32 en aan de andere kant op pin 15 van esp32 en via een weerstand van 10kΩ aan de ground. Over deze knop staat ook een condensator van 100nF die zorgt voor de debouncing van de knop. Naast deze componenten zijn ook enkele componenten voorzien voor het programmeren van de esp32.  Er is een pinheader met 4 pinnen aanwezig. Op twee pinnen worden de voeding en de GND aangesloten, op de andere twee worden de RX en TX aangesloten. Ook zijn een enable en boot knop aanwezig die beide voorzien zijn van een condensator zoals de extra knop die eerder werd besproken. Enkele van deze componenten moeten echter ook ontkoppeld worden. Voor de esp32 en de MPU-9250 wordt dit gedaan met een condensator van 100nF en voor de LDO wordt dit gedaan met een condensator van 10µF. Voor de rest zijn nog een paar pull up weerstanden van 4,7 kΩ voorzien. Voor verdere details en de precieze aansluiten wordt verwezen naar de Kicad-files.
## LCD
