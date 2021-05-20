---
layout: default
title: Software
parent: Morse
grand_parent: Onderdelen
nav_order: 2
---

# Software
Voor het project wordt gebruik gemaakt van een ESP32-chip. 

## void setup()
Zoals elk Arduino programma zal de code beginnen bij de `setup()`-methode. Deze methode wordt eenmaal doorlopen, het bevat informatie voor de opstart van het programma. Variabelen, pin modes, wifi-instellingen ... worden hier geïnitialiseerd. 

Eerst en vooral verhogen we hier de baud rate. Voor ons project verhogen we het maximum aantal symbolen per seconde naar 115200. De ROM bootloader van de ESP32 communiceert namelijk met deze snelheid. Wanneer we de baud rate niet zouden aanpassen, kan de inkomende data niet correct gelezen worden door de UART.

In beide onderdelen wordt ook een debounce tijd voorzien voor de button, hiervoor wordt het commando `setDebounceTime(x)` gebruikt. Deze methode wordt toegepast, elke keer de button wordt gebruikt. Wanneer een verandering van de button-toestand wordt gedetecteerd (van low naar high of andersom), zal daarna elk binnenkomend signaal voor x milliseconden genegeerd worden. 

Om te kunnen werken met de broker via MQTT, moet ook een wifi-setup() doorlopen worden. Aangezien deze methode wordt opgeroepen in de setup()-methode, wordt `setup_wifi()` dus 1 keer uitgevoerd. 

Verder wordt ook de MQTT-server en MQTT-poort geïnitialiseerd via de methode `client.setServer(MQTT_SERVER, MQTT_PORT)`. Dit is nodig om informatie ("messages") te krijgen van het juiste IP-adres en de juiste poort.

## void setup_wifi()
In deze setup gebeuren de wifi-instellingen. Via de methode `WiFi.begin(SSID, PWD)` wordt de Service Set IDentifier meegegeven (dit is de naam voor een bepaald netwerk met een IP-adres), én het bijhorende paswoord van dit WiFi-signaal. 
Ter controle vragen we aan het einde van de methode het IP-adres waarmee werd verbonden via de methode `WiFi.localIP()`.

Na deze setups kan het programma beginnen aan de loop()-methode. Deze methode zal telkens opnieuw herhaald worden. Het eerste dat wordt gedaan, is nakijken of er verbonden is met de MQTT-server. Als dit (nog) niet het geval is, zal via `reconnect()` (opnieuw) worden geconnecteerd met de MQTT-server. Deze methode zal blijven doorlopen worden zolang er niet geconnecteerd werd. Wanneer er connectie gelegd werd, zal de client ook onmiddellijk subscriben op kanalen waarvan het informatie wil ontvangen. 

Zodra deze connectie op punt staat, kan de rest van de code worden uitgevoerd. Het verdere verloop is verschillend voor de speaker en de micro. Deze onderdelen worden verder dus apart besproken.

## void loop() - Speaker
Volgende paragraaf bespreekt `void loop()` van de speaker.

De loop functie wordt continu doorlopen zolang er geen reset, pauze, of ander interrupt commando wordt gegeven.
Er wordt eerst gezorgd dat er een wifi en MQTT connectie gemaakt is met de broker. Nadat dit gebeurd is wordt de `playRinkeltoon()` opgeroepen die een rinkeltoon laat afspelen zolang de knop niet ingedrukt is geweest. Eenmaal de knop ingedrukt is wordt er overgegaan naar de tweede luidspreker die in de telefoon zit. De methode `playMorse()` wordt opgeroepen.

### button.loop()
Deze functie wordt opgeroepen bij de start van de `void.loop()`. De knop is dan actief en er wordt waargenomen wanneer de knop ingedrukt wordt.

### playRinkeltoon()
Wanneer het "BEL" signaal van de fitnesstracker puzzel binnenkomt start de luidspreker een rinkeltoon te spelen. Deze rinkeltoon is een WAV-bestand dat softwarematig wordt toegevoegd. Er is ook de optie om dit als mp3 bestand af te spelen met behulp van een SD kaart. Dit vereist dan wel de nodige hardware. 
Het WAV-bestand wordt eerst bewerkt met audacity; samplefrequentie verlagen, lengte audiofragment inkorten,... Zo wordt er een overflow voorkomen aangezien WAV-bestanden veel ruimte innemen. Nadien wordt dat bewerkte audio bestand in HxD gebracht (een Hex. editor). Hier wordt het audio bestand omgezet naar hex en kan het dan gekopieerd worden naar een C-array. Het bestand wordt als 'SoundData.h' onder include toegevoegd. Volgende [link](https://www.xtronical.com/basics/audio/dacs-for-sound/playing-wav-files/) kan hierbij helpen.

### playMorse()
Zodra de knop ingedrukt is geweest (`button.isPressed()`) wordt de `playMorse()` methode opgeroepen. In deze methode wordt de willekeurige aangemaakte morse code afgespeeld. Voordat de willekeurige morse code wordt afgespeeld, zal deze als string worden aangemaakt via de methode `getKar()`. De aangemaakte string wordt opgeslaan in `String st`. Deze wordt in `playMorse()` overlopen aan de hand van een for-lus die voor elk karakter in de string zijn bijhorende methode oproept. De methode `Morse()` is hier een tussenpersoon voor. Elk letter heeft zijn eigen methode; bv. indien het karakter 'b' in de string zit wordt de methode `getB()` opgeroepen waarin gedefinieerd staat wat de vertaling van 'b' is in morse. Voor deze vertaling wordt er gebruik gemaakt van de methodes `punt()` en `streep()`. De morse zelf wordt voorafgegaan door een laagfrequente toon om aan te geven dat het afspelen van start gaat. 

#### punt() & streep()
Beide methodes maken gebruik van de functie `tone()` waarin de pin, lengte en frequentie als argumenten kunnen worden opgegeven.
Voor een kort signaal wordt de `punt()`-methode opgeroepen, voor een lang signaal, dat zo'n tien maal langer is, wordt `streep()` opgeroepen.

## void loop() - Micro
De volgende delen bespreekt `void loop()` van de microfoon.

Het blokschema geeft een overzicht van het verloop van de code. Zoals eerder vermeld wordt `void loop()` oneindig keer herhaald. De verschillende methoden die verder worden besproken zullen dus ook vaak worden opgeroepen.

![](https://raw.githubusercontent.com/BachMorse/Documentatie/master/BlokschemaCodeMicro.JPG)

Beginnen doen we met de uitleg voor de default ingestelde waarden, zo wordt onmiddellijk duidelijk welke elementen gebruikt worden. We gebruiken 2 arrays van dezelfde lengte. De ene onthoudt de gefloten sequentie, de andere bewaart wat werd doorgestuurd van de speaker. Beide hebben ze een vaste lengte, zo moet niet elke keer opnieuw geheugen worden vrijgemaakt als de grens wordt overschreden. We nemen hier een lengte van 44, aangezien dit de langst mogelijke lengte is die kan bekomen worden. Naast deze array, worden er ook verschillende 'lopers' aangemaakt. Elk van deze loper houdt de positie in een array of op het display bij.

### button.loop()
Via `button.loop()` zal er continu gekeken worden wat er gebeurt met de signalen afkomstig van de button. Wanneer de button wordt ingedrukt (`button.isPressed()`), wordt al het voorgaande gewist. Voor de spelers is het dus gemakkelijk om opnieuw te beginnen wanneer ze merken dat ze fout bezig waren: de knop loslaten en opnieuw indrukken.

Wanneer de button ingedrukt blijft (dus wanneer zijn waarde 0 is, de button is namelijk active HIGH), zal de volgende code van `void loop()` worden uitgevoerd.

### analogRead()
Het eerste wat gebeurt is het detecteren van de analoge waarde uit de microfoon, via `analogRead()`. Wanneer we deze waarde weergeven in de terminal, ligt het tussen 0 (stil) en 4095 (max gedetecteerde waarde, bij een relatief luide omgeving). Hoe luider de omgeving, hoe groter de uitgeprinte digitale waarde. We zien digitale waarden verschijnen, omdat de ESP32 het analoge signaal dat ontvangen wordt van de microfoon omzet naar een digitale waarde. De maximale analoge waarde die de microfoon kan doorgeven is 3.3V, dit wordt omgezet naar de maximale digitale waarde. Deze digitale waarde is afhankelijk van de resolutie. De resolutie van de analoge pinnen is 12 bit. 3.3V wordt daarom omgezet naar 4095 (=111111111111).
Via de potentiometer kan de gevoeligheid van de microfoon worden aangepast. 4095 kan dus overeen komen met verschillende decibelwaarden, afhankelijk van de weerstand van de potentiometer. De potentiometer werd nu zó ingesteld dat 4095 wordt gedetecteerd als men fluit op een afstand van 5 cm van de speaker.

### som(array)
De gedetecteerde digitale waarde wordt opgeslagen in een rij via de functie `voegToe(analogValue)`. In deze rij zitten de laatste 100 digitale waarden die werden gedetecteerd. Hiervan wordt telkens de som uitgerekend. Hoe langer men fluit, hoe hoger de som wordt. 

### if-lus
Verder wordt de som vergeleken met vooraf opgegeven waarde. Zo moet de som minstens `4095*45` zijn om een lang signaal te detecteren. Aangezien we tussen elke detectie 2 milliseconden wachten, moet er voor een lang signaal minstens `45*2=90` milliseconden gefloten worden aan 1 stuk. Hetzelfde wordt gedaan voor een kort signaal, ook de duur van de stilte wordt gedeclareerd.

Telkens er een kort/lang/stil signaal wordt gedetecteerd, zal dit worden toegevoegd aan een morse-array. Deze morse array bevat de gedetecteerde morse sequentie. Het wordt gebruikt om te vergelijken met het signaal dat werd doorgestuurd vanuit de speaker. Er is wel een voorwaarde gekoppeld voordat een kort/lang/stil signaal wordt toegevoegd: het vorige gedetecteerde signaal dat werd toegevoegd, mag namelijk niet hetzelfde zijn als het signaal dat wordt toegevoegd. Hierdoor wordt vermeden dat hetzelfde kort/lang/stil signaal 2 keer wordt toegevoegd.

De speaker stuurt {0, 1} door bij een Punt (= kort signaal), en {0, 1, 2} bij een Streep (= lang signaal). Zoals te zien, zal er bij een lang signaal voorafgaand eerst een kort signaal worden gedetecteerd. Om te vermijden dat, onmiddellijk na toevoeging van '2' aan de morse array, opnieuw '1' wordt gedetecteerd, zullen alle waarden van de som-array verwijderd worden.

### vergelijk() en vergelijk_fout()
Als laatste komt het erop neer om de gefloten sequentie te vergelijken met de sequentie afkomstig van de speaker. De volledige gefloten array wordt hierbij overlopen, en voor elk element wordt gekeken of ze overeenkomt met de sequentie van de speaker. Als dit het geval is, voegen we 1 toe bij een teller. Wanneer de teller even groot is als de lengte van de morse-array, wil dit zeggen dat alle waarden overeen komen. De oplossing zal dan verschijnen op de display.

De methode `vergelijk_fout()` vergelijkt het gefloten deeltje met het overeenkomstige deel van de array afkomstig van de speaker. 

### aansturen display
Via de methode `lcd.setCursor(0, 1)` zetten we de cursor van het display op rij 1, kolom 0. Vanaf dat er 1 fout werd gefloten, zal dit te zien zijn op het scherm. Verder wordt de display ook gebruikt om te tonen wat de spelers fluiten. Een punt wordt weergegeven met '.', een streep met '_'. Om het resultaat te tonen zal de display ook gebruikt worden. 

## Communicatie
Al het voorgaande zal enkel worden uitgevoerd wanneer er geen pauze- of resetsignaal werd ontvangen. Om deze berichten te ontvangen, moeten de devices gesubscribed zijn op de kanalen waarvan het berichten wil ontvangen. Dit gebeurt net nadat er connectie wordt gelegd met de broker in de methode `reconnect()`. Verder is er ook steeds een `callback()`-functie voorzien, die opgeroepen wordt vanaf er een bericht ontvangen wordt op één van de gesubscribde kanalen. Afhankelijk van het kanaal en de "message" wordt een andere actie uitgevoerd.

Over de broker kunnen verschillende berichten worden verstuurd. Wel is belangrijk dat deze berichten steeds van het type `char` zijn. Integers kunnen namelijk niet worden verstuurd over alle kanalen.

### Wat bij pauze en reset?
Deze messages worden verstuurd over het kanaal "esp32/morse/control".
Bij een reset signaal wordt er volledig opnieuw opgestart. Geen enkele vorige waarde wordt onthouden, en ook de WiFi wordt verbroken om vervolgens opnieuw te connecteren vanuit de setup()-methode. Reset gebeurt wanneer de broker het bericht "0" over het kanaal stuurt. Bij de speaker zal ook een reset worden toegepast, wanneer het signaal correct werd nagefloten, en de puzzel dus niet meer gebruikt zal worden tijdens het spel.

Pauzesignalen zorgen ervoor dat de speaker stopt met het uitzenden van de morsesequentie. De sequentie zal nog worden afgerond, maar het zal niet opnieuw beginnen met een herhaling. Bij de microfoon zal niet meer worden gereageerd op binnenkomende signalen, waardoor ze terug opnieuw moeten beginnen met fluiten van de sequentie. Het display zal ook uitvallen om duidelijk te maken dat de microfoon niet werkt.

### Overige communicatie
* Het kanaal "esp32/fitness/telefoon" wordt gebruikt om onze puzzel een startsignaal te geven. De telefoon zal beginnen rinkelen en de display van de microfoon zal oplichten. Hierdoor is het duidelijk dat er voldoende energie is om beide onderdelen te laten werken. Het bericht is afkomstig van de fitnenesstracker.
* De morse-sequentie die wordt aangemaakt bij de speaker, wordt via het kanaal "esp32/morse/intern" doorgestuurd. Bij de morse code wordt het karakter omgezet naar een integer, zodat correct kan worden vergeleken. 
* Wanneer onze puzzel gedaan is, zal vanuit de microfoon het bericht "einde_morse" over het kanaal "esp32/morse/output" worden gestuurd, om aan de volgende puzzel te laten weten dat het mag starten. Ook laat de micro aan de speaker weten dat het morse signaal niet meer moet worden herhaald. Dit gebeurt over het kanaal "esp32/morse/intern".
* Aangezien het cijfer voor de alohomorapuzzel varieert, moet ook dit over een kanaal worden doorgestuurd. Er wordt gecommuniceerd over het kanaal "esp32/alohomora/code2".
