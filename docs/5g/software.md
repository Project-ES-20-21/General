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
  - [LED-bar](#LED-bar)
  - [Auto](#Auto) 
# Opmerkingen
## Communicatie
De manier van communiceren in de puzzel zelf, alsook met de broker is zo gekozen omdat er enkele zaken zijn waar mee rekening moest gehouden worden. Met de gebruikte ESP-32 met 4MB flash memory is het niet mogelijk om zowel MQTT als Bluetooth te gebruiken. Het wisselen tussen 2 WiFi-netwerken (ook al gebruik je een statisch IP-adres) zorgt voor veel vertraging. ESP-now moet op beide ESP-32's op hetzelfde WiFi-kanaal zitten - hierdoor moet de stralingslocatie hetzelfde WiFi-kanaal gebruiken als de WiFi waarop de broker zit. Dit is niet een ideale situatie, maar om met ESP-now te willen werken de enige mogelijkheid. Dit is omdat de LED-bar namelijk al een kanaal toegewezen krijgt door het verbinden met de broker, en aangezien we willen communiceren met de auto moet deze hetzelfde kanaal hebben. Dit kan dus enkel als ook deze ESP-32 verbonden is met een WiFi-signaal op hetzelfde WiFi-kanaal.
## Afstandsensor 
In de code wordt de afstand twee keerna elkaar gechecked, alvorens het alarm af gaat. Dit is om te vermijden dat door omgevingsfactoren, zoals voegen in de vloer als de wagen rijdt, dit onnodig afgaat. Uit verschillende testen is gebleken dat dit een simpele oplossing is met een goed resultaat. 
# Code
De code van de verschillende onderdelen van deze puzzle vind u hieronder terug. (Under Construction)
## Stralingslocatie
## LED-bar
## Auto
