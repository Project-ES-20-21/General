---
layout: default
title: Fitnesstracker
parent: Onderdelen
nav_order: 2
---

# Fitnesstracker

## Inhoud

- [Algemeen](#Algemeen)
- [Blokschema](#Blokschema)
- [Communicatie](#Communicatie)
- [Error Handling](#Error-Handling)

## Algemeen
De fitnesstracker heeft eigenlijk twee belangrijke functies. Een eerste die ervoor zorgt dat de kamer voorzien wordt van ‘elektriciteit’ dat voorzien wordt vanuit een virtuele batterij. Wanneer deze batterij niet opgeladen is, kunnen de andere puzzels niet worden uitgevoerd. De spelers kunnen deze opladen door juiste oefeningen te doen. Één juiste oefening resulteert in een kleine stijging(2 à 3%) van de batterij. Deze batterij wordt weergegeven op een LCD samen met een oefeningenreeks, dat bestaat uit een combinatie van verschillende oefeningen en tenslotte extra informatie voor de spelers. De oefeningenreeks kan teruggevonden worden op de poster dat ergens in de kamer is geplaatst. 

De andere belangrijke functie van de fitnesstracker is het vrijgegeven van een nummer van het digitaal cijferslot en het laten rinkelen van de telefoon van de puzzel morsecode. De spelers kunnen dit realiseren door de een volledige oefeningenreeks correct uit te voeren. Om zo een volledige reeks uit te voeren moeten de spelers eerst op de knop aan de kettlebell drukken, waarbij er een dubbele piep weerklinkt. Dit wil zeggen dat de esp32 aan de kettlebell klaar is om te meten. Op de LCD verschijnt er nu ook ‘Meten’. Zolang de kettlebell relatief stil gehouden wordt zal deze nog niet beginnen meten. Pas kort nadat men de beweging effectief inzet zal deze beginnen te meten. De spelers moeten één enkele beweging binnen de 2 seconden uitvoeren. Pas bij de volgende enkele korte piep mogen de spelers een volgende beweging beginnen. Dit process gaat door totdat men ofwel een lange piep  of een korte dubbele piep hoort. Een lange piep wil zeggen dat de spelers 5 seconden hebben om zich klaar te maken voor de volgende oefening soort binnen de oefeningenreeks. Na de 5 seconden zal een korte piep weerklinken, wat wil zeggen dat men opnieuw de oefening mag beginnen. 

Een dubbele piep wil zeggen dat de serie juist of fout werd uitgevoerd. Welke van de twee het is zal worden weergegeven op de LCD. Als deze fout werd uitgevoerd zal men de reeks vanaf nul moeten herbeginnen door weer op de knop te duwen en er zal 10% van de batterij afgetrokken worden. Als deze juist is zal men een cijfer van het cijferslot zien verschijnen in de rechterbovenhoek van de LCD. Tevens zal er ook een signaal naar de broker gestuurd worden dat de telefoon van morsecode mag beginnen rinkelen.

## Blokschema
