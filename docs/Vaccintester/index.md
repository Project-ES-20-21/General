---
layout: default
title: Vaccintester
parent: Onderdelen
nav_order: 5
has_children: true
---

# Vaccintester
Dit is de uitleg horende bij de vaccintester uit de escape room.

## Inhoud

- [Algemene uitleg](#algemene-uitleg)
- [Flowcharts](#flowcharts)
- [Implementatie](#implementatie)
- [Budget](#budget)
- [Risico's](#risicos)
- [Gantt chart](#gantt-chart)

## Algemene uitleg

Door het oplossen van de 5G-puzzel, wordt er een RFID kaart verkregen. Die kaart kan gebruikt worden om de uiteindelijke kleurensequentie voor de vaccintester te vinden. In de deur van een kast zit op een bepaalde plaats een NFC reader. De spelers moeten dan met de kaart op zoek gaan naar de kast en uitzoeken waar in de deur de NFC reader zit. Door de kaart dan te scannen worden er witte leds in de kast geactiveerd. Boven elke witte led bevindt zich een gekleurde vloeistof in een proefbuis. De houders voor de proefbuis kunnen gebruikt worden om de witte leds weg te steken. De leds gaan in een bepaalde volgorde oplichten, zodat er een sequentie gevonden kan worden van 7 kleuren die na elkaar oplichten. Om die kleurensequentie in te geven wordt er gebruik gemaakt van drie knoppen die verspreid zijn in de ruimte. Bij elke knop hoort een kastje, hierin kan een voorwerp met een primaire kleur gestoken worden. Om de kleuren te vormen worden de knoppen dan ingedrukt, de kleur van het voorwerp wordt gescand. Om alle spelers te laten meedoen moeten de knoppen om de kleuren te vormen binnen een bepaald interval ingedrukt worden. Zo wordt geel gevormd door groen, rood en zwart moeten groen, rood en zwart dus ongeveer op hetzelfde moment ingedrukt worden. Die knoppen zorgen er dan voor dat de bijhorende ledstrip die zich in een spuit bevindt oplicht. Wanneer de juiste kleur verkregen is wordt er gesprongen naar de volgende kleur in de sequentie en herhaalt het vorige zich. Wanneer de laatste kleur juist is, wordt een random getal gegenereerd dat een digit van de te zoeken code is. Deze wordt doorgestuurd naar de centrale eenheid en ook getoond aan de deelnemers door het aantal proefbuizen die oplichten binnen de kast.

Om dit alles te kunnen uitvoeren wordt er gebruik gemaakt van twee ESP32's, die met elkaar kunnen communiceren via een ESPnow-connectie. Wanneer de juiste RFID gescand wordt, wordt er een signaal verzonden van de ESP32 die verbonden is met de NFC-reader naar de ESP32 die verbonden is met de kleursensoren, de drukknoppen en de leds binnen de spuit en in de kast. Dat signaal zal ervoor zorgen dat de kleursensoren, de leds binnen de spuit en in de kast en de drukknoppen geactiveerd worden.

Daarnaast zal de laatst besproken ESP via het wifi-netwerk geconnecteerd zijn met de broker. Op die manier kan er uiteindelijk een digit van de code die ingevoerd moet worden bij alohomara doorgestuurd worden naar de centrale eenheid of broker.

## Flowcharts
### Vaccintester
![Flowchart_vaccintester](https://github.com/Project-ES-20-21/General/blob/gh-pages/docs/Vaccintester/Foto's/Flowchart_general.png)
### RFID
![Flowchart_RFID](https://github.com/Project-ES-20-21/General/blob/gh-pages/docs/Vaccintester/Foto's/flowchart_RFID.png)
### Buttons
![Flowchart_buttons](https://github.com/Project-ES-20-21/General/blob/gh-pages/docs/Vaccintester/Foto's/flowchart_button.png)
### Centrale ESP32
![Flowchart_centralESP32](https://github.com/Project-ES-20-21/General/blob/gh-pages/docs/Vaccintester/Foto's/flowchart_central_ESP32.png)



## Implementatie

De positie van deze puzzel binnen het lokaal maakt in principe niet zo veel uit. Er moet wel voor gezorgd worden dat de knoppen minstens 1,5 meter van elkaar gescheiden zijn zodat telkens aan de voorwaarde van de "T'is beter op anderhalve meter"-puzzel voldaan is. Daarnaast gaat de spuit aan een muur moeten hangen zodat alles op een correcte manier aangesloten kan worden.

## BOM

De kostenanalyse wordt hieronder weergegeven.

| Spuit                     | Aantal | Prijs per stuk | Totaal  |
|---------------------------|--------|----------------|---------|
| PCBway (10 per soort)     | 1      | € 5.00         | € 5.00  |
| Kleurenledstrip 1m        | 1      | € 10.00        | € 10.00 |
| ESP32                     | 1      | € 2.60         | € 2.60  |
| Drukknop groot            | 1      | € 0.50         | € 0.50  |
| DC jack DCJ250            | 1      | € 0.84         | € 0.84  |
| LDO 3.3V                  | 1      | € 0.43         | € 0.43  |
| Boot/enable button        | 2      | € 0.48         | € 0.96  |
| Condensator 10 μF (1206)  | 2      | € 0.24         | € 0.49  |
| Condensator 100 nF (1206) | 4      | € 0.12         | € 0.47  |
| Weerstand 10 kΩ (1206)    | 7      | € 0.05         | € 0.35  |
| Weerstand 47 kΩ (1206)    | 1      | € 0.05         | € 0.05  |
| Mosfet 2N7000             | 3      | € 0.21         | € 0.63  |
| Andere mosfet             | 3      | € 0.25         | € 0.75  |
| Pin header 2              | 1      | € 0.05         | € 0.05  |
| Pin header 4              | 2      | € 0.05         | € 0.10  |
| Adapter 12V               | 1      | € 5.00         | € 5.00  |
| Totaal                    |        |                | € 28.22 |

| Buttons (beide samen)     | Aantal | Prijs per stuk | Totaal  |
|---------------------------|--------|----------------|---------|
| PCBway (10 per soort)     | 1      | € 5.00         | € 5.00  |
| ESP32                     | 2      | € 2.60         | € 5.20  |
| Drukknop groot            | 2      | € 0.50         | € 1.00  |
| Micro-USB female          | 2      | € 0.92         | € 1.84  |
| LDO 3.3V                  | 2      | € 0.43         | € 0.86  |
| Boot/enable button        | 4      | € 0.48         | € 1.92  |
| Condensator 10 μF (1206)  | 4      | € 0.24         | € 0.98  |
| Condensator 100 nF (1206) | 8      | € 0.12         | € 0.94  |
| Weerstand 10 kΩ (1206)    | 8      | € 0.05         | € 0.40  |
| Weerstand 47 kΩ (1206)    | 2      | € 0.05         | € 0.10  |
| Pin header 8              | 2      | € 0.05         | € 0.10  |
| Pin header 4              | 2      | € 0.05         | € 0.10  |
| Pin header 2              | 2      | € 0.05         | € 0.10  |
| Powerbank                 | 2      | € 5.00         | € 10.00 |
| Kleurensensor             | 2      | € 4.00         | € 8.00  |
| Totaal                    |        |                | € 36.54 |

| Kast                      | Aantal | Prijs per stuk | Totaal  |
|---------------------------|--------|----------------|---------|
| PCBway (10 per soort)     | 1      | € 5.00         | € 5.00  |
| ESP32                     | 1      | € 2.60         | € 2.60  |
| Witte Leds                | 6      | € 0.10         | € 0.60  |
| Micro-USB female          | 1      | € 0.92         | € 0.92  |
| LDO 3.3V                  | 1      | € 0.43         | € 0.43  |
| Boot/enable button        | 2      | € 0.48         | € 0.96  |
| Condensator 10 μF (1206)  | 2      | € 0.24         | € 0.49  |
| Condensator 100 nF (1206) | 3      | € 0.12         | € 0.35  |
| Weerstand 10 kΩ (1206)    | 4      | € 0.05         | € 0.20  |
| Weerstand 5.6 kΩ (1206)   | 6      | € 0.05         | € 0.30  |
| Pin header 6              | 3      | € 0.05         | € 0.15  |
| Pin header 4              | 1      | € 0.05         | € 0.05  |
| Powerbank                 | 1      | € 20.00        | € 20.00 |
| NFC reader                | 1      | € 5.00         | € 5.00  |
| Totaal                    |        |                | € 37.05 |

| Algemeen totaal | € 101.81 |
|-----------------|----------|

## Risico's

- Wanneer de 5G-puzzle niet werkt, kan de nodige RFID kaart niet verkregen worden. Dit kan opgelost worden door een tweede kaart te gebruiken die ook gebruikt kan worden om onze puzzel te starten. Deze kaart kan ergens verstopt worden binnen de kamer. De positie van die kaart kan dan via een raadsel achterhaald worden.

- Wanneer de RFID reader en de receiver niet geconnecteerd zijn, kan de puzzel niet volledig gestart worden. Dit kan eventueel opgelost worden door Alohomora een signaal te laten versturen naar de tweede ESP32. 

- De kleursensor kan soms bij de eerste meting een fout resultaat geven. Dit kan opgelost worden door bijvoorbeeld tien metingen na elkaar te doen en het gemiddelde te nemen van de verschillende waarden.

- De kleursensor kan foute resultaten geven door reflectie. Er moet dus gebruik gemaakt worden van een buis die mat is, waardoor de resultaten geen invloed van reflectie zullen hebben.

## Gantt chart

Via volgende link gaat u naar onze Gantt chart horende bij dit project: 
[Gantt vaccintester (ClickUp)](https://share.clickup.com/g/h/4dne7-50/c3532202026c060)
