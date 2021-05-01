---
layout: default
title: Software
parent: 5G
grand_parent: Onderdelen
nav_order: 1
---

# Inhoud 
- [Opmerkingen](#Opmerkingen)
  - [Communicatie](#Communicatie)
  - [Afstandsensor](#Afstandsensor)
- [Code](#Code)
  - [Stralingslocatie](#Stralingslocatie)
  - [Ledbar](#Ledbar)
  - [Auto](#Auto) 
# Opmerkingen
## Communicatie
De manier van communiceren in de puzzel zelf en met de broker. Is zo gekozen omdat er enkele zaken zijn waar mee rekening moest gehouden worden. Met de gebruikte esp32 met 4mb flash memory is het niet mogelijk om mqtt en bleutooth te gebruiken. Het wisselen tussen 2 wifi-netwerken ook al gebruik je een statisch ip-adress zorgt voor veel vertraging. Esp now moet op beide esp32 op hetzelfde wifi kanaal zitten. Hierdoor moet de stralingslocatie hetzelfde wifikanaal gebruiken als de wifi waarop de brooker zit. Dit is niet ideaal maar wel de enige mogelijkheid. Dit omdat de ledbar al een kanaal toegewezen krijgt door het verbinden met de broker. En aangezien we willen communiceren met de Auto moet deze hetzelfde kanaal hebben. Dit kan enkel als ook deze esp verbonden is met een wifisignaal op hetzelfde wifikanaal.
## Afstandsensor 
In de code wordt 2 keer de afstand na elkaar gechecked alvorens het alarm te laten afgaan dit omdat door voegen in de vloer als de wagen rijdt of andere fouten het alarm onnodig kan afgaan. Uit verschillende testen is gebleken dat dit een simpele oplossing is met een goed resultaat. 
# Code
De code van de verschillende onderdelen van deze puzzle vind u hieronder terug.
## Stralingslocatie
## Ledbar
## Auto)
