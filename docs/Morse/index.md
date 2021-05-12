# Morse

## Doel en verhaal
Wanneer er voldoende energie werd opgewekt bij de vorige puzzel [fitnesstracker](fitnesstracker), wordt ook meteen duidelijk dat de Russen via morse code belangrijke informatie doorsturen. Deze informatie kan ook gebruikt worden bij de klinische testen van ónze virologen. Deze klinische testen vinden zogezegd plaats in een ruimte naast de escape room. Daarom moeten ze de morse informatie doorgeven, via een fluitje, aan de onderzoekers van de klinische testen.

De morse sequentie komt uit de speaker. Wanneer de sequentie juist werd nagefloten, zal er op een display een cijfer verschijnen én een tip voor de volgende proef. Het cijfer kan gebruikt worden bij de Alohomora-puzzel.

De bedoeling van onze puzzel is om een uitgezonden morse code na te fluiten aan de andere kant van de kamer. Een microfoonsensor zal dit geluid opvangen en het zal nagaan of het nagebootste gefluit overeenkomt met het uitgezonden ritme. Om een morse code uit te zenden maken we gebruik van een speaker. Deze speaker is geïmplementeerd in een telefoon.

Om aan de spelers te laten weten wat er juist moet gebeuren met het fluitje zal er een blad naast de micro liggen waarop, deels in morse, de uitleg staat.

## Blokschema

## Hardware
### Speaker

### Micro
Op de PCB-plaat voorzien we een aansluiting voor een externe display, een externe microfoon en een externe button. Deze 3 elementen worden via pinheaders aangesloten. Om de ESP te programmeren, wordt een extra pinheader voorzien. Aangezien de ESP32 maar 1 keer geprogrammeerd moet worden, is dit de meest efficiënte manier. 

De ESP32 zal gevoed worden met een powerbank van 5V. Deze 5V is nodig om de display aan te sturen. Via de LDO zal 5V omgezet worden naar 3.3V. De program header, de microfoon en de ESP32 worden aangesloten aan de 3.3V.

De enable butten en reset button worden voorzien van een pull-up weerstand, net zoals (?)de transmit-pin en de VSPI-pin (GIO5)(?).Vaak wordt ook een pull-up weerstand van 4k7 voorzien voor SCL en SDA aansluiting. Deze is al voorzien in de externe display, de MH-sensor (? sensor?).

De display maakt gebruik van I2C communicatie. Hiervoor maken we gebruik van de SDA- en SCL-poorten. Via de SDA-poort sturen we data van en naar de display, vanuit de ESP32. Deze 2 poorten zijn active low. Daarom werden pull-up weerstanden voorzien op het display zelf. De pull-up weerstanden moeten dus niet op onze eigen PCB aangesloten worden.

Het centrale element, ESP32, voorzien we, naast de verschillende componenten die nog volgen, ook van een LEDlichtje. Hiermee kunnen we gemakkelijk controleren of het solderen gelukt is en of de ESP werkt.
<p align="center">
  <img src=https://user-images.githubusercontent.com/78847177/115971593-9707d280-a549-11eb-82d4-021c228c60fe.png>
</p>

De micro-USB voorziet een voltage van 5 volt. De andere pinnen worden niet gebruikt. De programmatie van de ESP32 gebeurt namelijk via een pinheader. Deze 5V wordt gebruikt om de display te voeden. Via MOSFET?? wordt deze 5 volt omgezet naar 3.3V om de ESP32 met de juiste spanning te voeden, om de pinheaders met de juiste spanning te voeden, en om de micro met de juiste spanning te voeden.
<p align="center">
  <img src=https://user-images.githubusercontent.com/78847177/115971624-c9b1cb00-a549-11eb-8010-be407baed09c.png>
</p>

De level shifters verhogen de voltage van 3.3 volt, afkomstig van de ESP32 naar 5 volt. Deze 5V is nodig om de display op de juiste voltage te kunnen aansturen via SCL- en SDA-poorten.
<p align="center">
  <img src=https://user-images.githubusercontent.com/78847177/115971693-2e6d2580-a54a-11eb-963d-52924a602175.png>
</p>


De verschillende pin headers zorgen voor de aansluiting van de externe componenten. We gebruiken mannetjes zodat de pinnen niet kunnen plooien als de PCB verplaatst wordt.
<p align="center">
  <img src=https://user-images.githubusercontent.com/78847177/115971717-62e0e180-a54a-11eb-96f8-1c58a65d20aa.png>
</p>

De verschillende externe componenten zijn de display. ... Daarnaast maken we ook gebruik van een button, deze button is niet enorm stabiel, het debounced bij het indrukken en loslaten. Voor deze puzzel is het geen stoorfactor, de button is namelijk snel genoeg stabiel om zijn functie te kunnen uitvoeren. Als derde externe component maken we, uiteraard, gebruik van een micro. Deze microfoon heeft een potentiometer die we kunnen aanpassen naar de gewenste gevoeligheid.

De [uiteindelijke printplaat](https://github.com/BachMorse/Micro_PCB) ziet eruit als volgt:
<p align="center">
  <img src=https://user-images.githubusercontent.com/78847177/116796141-7e179800-aada-11eb-9f48-b499f2575616.png)
</p>

Voor de duidelijkheid laten we het grondvlak even achterwege. Zoals te zien plaaststen we de ontladingscondensatoren zo dicht mogelijk bij de ESP32.


## Software
### Speaker
### Micro
Bij de micro maken we gebruik van de waarde 4095. Dit is de maximum waarde die de microfoon kan doorsturen wanneer het een luid signaal detecteert. Het komt overeen met 3.3V en het is een 12bit-signaal. Er wordt gekeken naar de som van de vorige 100 samples, wanneer deze een bepaalde grens overschrijdt, zal de code dit zien als een kort/lang signaal. De detectie van een kort of een lang signaal wordt toegevoegd aan een andere array. 
Omdat we de som nemen, zal er bij een lang signaal eerst een kort signaal gedetecteerd worden. Ook kan het zijn dat een kort signaal van de speler iets langer duurt dan het opgegeven kort signaal, zonder controlevoorwaarde, worden er dan meerdere korte signalen gedetecteerd. Om dit te vermijden voerden we een controlevoorwaarde in. Ook de stiltes moeten dan gedetecteerd worden om 2 korte signalen na elkaar mogelijk te maken.

<p align="center">
  <img src=https://user-images.githubusercontent.com/78847177/116791979-47cc1f80-aabe-11eb-94c7-dcd034fb1ed0.png>
</p>

## Communicatie
Eerst en vooral moet er gecommuniceerd worden met de vorige puzzel, de fitnesstracker, en met de volgende puzzel, 5G-proef. Wanneer de opdracht van de fitnesstracker werd volbracht, mag de speaker beginnen met het uitzenden van de morse code.
Via het kanaal "esp32/fitness/telefoon" wordt het signaal "BEL" gestuurd. Zo weet de speaker dat het geluid mag beginnen spelen.

Wanneer de spelers klaar zijn met onze proef, zal er een start signaal gegeven worden naar de afstandsbediening van de 5G-proef. Hiervoor wordt het "morse_einde" doorgestuurd via het kanaal "esp32/morse/output".

Daarnaast wordt over de broker ook de juiste morse-sequentie doorgegeven door de speaker aan de microfoon. Hierdoor weet de microfoon met welke sequentie het ontvangen fluitsignaal moet vergeleken worden. Het kanaal waarover we hiervoor communiceren is "esp32/morse/intern".

En als laatste zorgt de broker ook voor een start-, pauze- en stopsignaal. Wanneer we een pauzesignaal ontvangen, zal de button van de microfoon niet werken, de speaker zal stoppen met geluid uitzenden.

## Opstelling
De speaker wordt geïmplementeerd in een telefoontoestel. Voor de microfoon maakten we gebruik van de lasercutter. In deze behuizing voorzien we plaats voor de button, de display en de microfoon. Voor de 5V voeding van beide delen, is een powerbank nodig.
De plaats is niet echt van belang, aangezien het kleine beweegbare componenten zijn.

## Mogelijke errors
Mogelijke errors zijn debouncing van de button. In principe is dit geen probleem als de speler niet al te snel na het indrukken begint te fluiten. Ook zal het LCD-scherm blijven staan wanneer de juiste code wordt nagefloten. Het bouncen van de button heeft daarom geen effect op de zichtbaarheid van het Alohomoracijfer en de tip voor de volgende puzzel.

Een andere factor die fout kan veroorzaken is het achtergrondgeluid. Om dit tegen te gaan, werkten we met een button die ingedrukt moet worden tijdens het fluiten. Zo weet de microfoon dat het ontvangen geluid gefluit zal zijn. Als het relatief stil is op de achtergrond, heeft de microfoon geen moeite om achtergrondgeluid te onderscheiden van het gefluit. Enkel wanneer er veel geluid is op de achtergrond, kan het zijn de microfoon foute signalen ontvangt.
