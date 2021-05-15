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

Het doel van de puzzel is het mengen van kleuren om een sequentie van kleuren na te maken. Wanneer deze sequentie volledig correct nagemaakt is, wordt een getal van de code verkregen. 

In het ideale geval zou er een RFID kaart gescand moeten worden aan een kast. Door het scannen van deze kaart wordt er een random sequentie van zeven kleuren aangemaakt. Deze sequentie wordt getoond door leds in een bepaalde volgorde te doen oplichten. Elke led staat voor een bepaald kleur. Dat kleur zal duidelijk gemaakt worden door een pillendoosje met materiaal in bijhorende kleur boven de led te plaatsen. Na het tonen van de zeven kleuren lichten alle leds tegelijk twee maal op en herhaalt de sequentie zich. 

Deze kleuren moeten worden nagemaakt door groen, blauw of rood in de dozen met de kleurensensors te schuiven. Door op de knoppen bovenop de dozen te duwen worden deze kleuren 
doorgestuurd naar de ledstrip en kunnen deze dus gemengd worden door bijvoorbeeld groen in de ene doos te steken en blauw in de andere doos en dan op de knoppen te drukken. 
Wanneer men zeker is van de kleur, kan men op de knop aan de ledstrip drukken om het kleur op de ledstrip te plaatsen en te laten controleren. Wanneer de juiste kleur gevormd is zal de ledstrip groen pinken en kan de volgende kleur van de sequentie gevormd worden. Wanneer een foutieve kleur aangemaakt is zal de ledstrip rood pinken en moet men opnieuw bij de eerste kleur van de sequentie beginnen. Wanneer uiteindelijke alle kleuren juist gevormd zijn zal de ledstrip opeenvolgend een aantal keer blauw pinken en een aantal keer wit pinken, wat het laatste getal van de code voorstelt. Wanneer de ledstrip vier maal blauw pinkt en daarna vier keer wit, is het getal dus gelijk aan vier. Dit getal wordt dan ook via de broker naar Alohomora verstuurd.

De puzzel zou moeten opgelost kunnen worden in 10-15 minuten.
## Flowcharts
### Vaccintester
![Flowchart_vaccintester](Flowchart_general.PNG)
### RFID
![Flowchart_RFID](flowchart_RFID.PNG)
### Buttons
![Flowchart_buttons](flowchart_button.PNG)
### Centrale ESP32
![Flowchart_centralESP32](flowchart_central_ESP32.PNG)

## Implementatie

De positie van deze puzzel binnen het lokaal maakt in principe niet zo veel uit. Er moet wel voor gezorgd worden dat de knoppen minstens 1,5 meter van elkaar gescheiden zijn zodat telkens aan de voorwaarde van de "T'is beter op anderhalve meter"-puzzel voldaan is. Daarnaast gaat de spuit aan een muur moeten hangen zodat alles op een correcte manier aangesloten kan worden. In dit project zijn de buttons gemaakt aan de hand van een 3D-printer. De spuit die aan de muur opgehangen wordt is met behulp van een laser cutter tot stand gebracht. Om de sequentie te tonen is gebruik gemaakt van een kast, ook tot stand gebracht met behulp van een lasercutter. In deze kast zijn pillendoosjes geplaats op een balk waarin de leds ingebouwd zijn. 

De bijhorende files horende bij het 3D-printen en de laser cutter zijn terug te vinden via onderstaande links:

- [3D-prints](https://github.com/Project-ES-20-21/General/tree/gh-pages/docs/Vaccintester/3Dprints)
- [Laser cutter](https://github.com/Project-ES-20-21/General/tree/gh-pages/docs/Vaccintester/Lasercut)

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

Algemeen totaal: € 101.81

## Risico's

- Wanneer er iets misloopt binnen de puzzel kan deze steeds gereset worden door via MQTT "0" te sturen via het kanaal "esp32/vaccin/control".

- Wanneer de 5G-puzzle niet werkt, kan de nodige RFID kaart niet verkregen worden. Hierdoor zou de puzzel niet gestart kunnen worden. 

- Wanneer de knoppen op de buttons niet goed ingedrukt worden wordt het juiste kleur niet daar de ledstrip gestuurd. Dit kan ervoor zorgen dat de gevormde kleur fout is en de spelers opnieuw moeten beginnen. Er wordt dus beter eens extra op de knop gedrukt. 

- Indien de broker niet werkt is er geen communicatie via MQTT meer mogelijk. Dit zou ervoor zorgen dat de verschillende delen van de puzzel niet met elkaar kunnen communiceren en de puzzel dus niet opgelost kan worden.

- Op een gegeven moment kunnen de PCB's van de buttons of deze met de NFC reader uitvallen doordat de powerbanks plat zijn. Dit kan opgelost worden door de powerbanks altijd zeer goed op te laden of door krachtigere powerbanks te gebruiken.

## Gantt chart

Via volgende link gaat u naar onze Gantt chart horende bij dit project: 
[Gantt vaccintester (ClickUp)](https://share.clickup.com/g/h/4dne7-50/c3532202026c060)
