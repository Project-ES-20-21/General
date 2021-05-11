---
layout: default
title: Alohomora
parent: Onderdelen
nav_order: 2
has_children: true
---

# Alohomora

## Inhoud

- [Touchlock](#Touchlock)
- [Timer](#Timer)

## Touchlock

### Algemeen
De touchlock bestaat uit een touchscreen met een 4-digit cijferslot dat toegang verleent tot een compartiment met de sleutel die gebruikt kan worden om te ontsnappen uit de escape room. Dit is de laatste stap die nodig is om te ontsnappen uit de escape room en gebruikt cijfers die verkregen zijn in alle voorgaande proeven.

De bedoeling van de interface op de touchscreen is dat deze vrij intuïtief is, zonder extra uitleg zou alles duidelijk moeten zijn. Het is wel zo dat men pas kan weten dat er maar 3 pogingen zijn na de eerste foute poging. Bij 3 foute pogingen zal het slot ook openen maar dan wel met de boodschap 'GAME OVER' op het scherm.

De touchscreen zelf zal niet responsief zijn tot alle nodige info doorgestuurd is via MQTT en de wifi kan afgesloten worden op de ESP32. Dit is een gevolg van het feit dat voor touchscreen functionaliteit nagenoeg alle pins van de ESP32 beschikbaar moeten zijn, sommige moeten zelf doorverbonden worden. De wifi functionaliteit gebruikt een aantal van deze pins als register, een fout signaal kan bijvoorbeeld het SSID of wachtwoord voor de verbonden router resetten. Aangezien dit gedrag in zekere zin handig is zien we dit als een feature en omzeilen we de beperkingen van de ESP32 door de code in 2 fases op te delen; met wifi en zonder wifi. 

Door dit gedrag van de ESP32 is er wel geen manier om het slot vanop afstand te openen eens de wifi uitgeschakeld is. Om hier eventuele problemen op te lossen voorzien we simpelweg code '0000' als noodoplossing moest iemand in de kamer zelf in paniek raken. Als de wifi nog aan staat kan er simpelweg van buitenaf een signaal gestuurd worden om het compartiment met de sleutel te openen.

### Blokschema
![Blokschema](Blokschema.png)

### Communicatie
Alle communicatie verloopt via de broker. De touchlock ontvangt in principe alleen maar data en verzendt zelf nooit, behalve dan voor debug doeleinden. De gebruikte channels zijn:
- "esp32/alohomora/control" dit kanaal wordt enkel gebruikt als er handmatig moet ingegrepen worden wanneer de ESP32 nog in wifi mode staat. Bij perfecte werking wordt dit kanaal in principe niet benut.
- "esp32/alohomora/code1" de kanalen van deze vorm hebben als enig nut om de correcte digit te ontvangen en die op de daarmee corresponderende positie in de code te plaatsen.
- "esp32/alohomora/code2","esp32/alohomora/code3","esp32/alohomora/code4" zie hierboven.

### Opstelling
Aangezien de touchlock de sleutel bevat die de uitweg biedt uit de escape room is het logisch om de opstelling zo dicht mogelijk bij de deur te plaatsen. Door het feit dat we in het voorziene lokaal niets aan de muur mogen bevestigen zorgen we er wel best voor dat het geheel tegen een muur staat, alles zou stevig genoeg moeten zijn maar er is niet uitvoerig getest op schudden en kantelen en dit zou anders voor problemen kunnen zorgen. 

### Error handling

## Timer

 