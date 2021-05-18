---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: home
nav_order: 1
---

# Algemeen

Dit is de algemene beschrijving van de escape room. De documentatie en afspraken worden bijgehouden in de [docs folder](./docs/).

## Inhoud
 
- [Algemeen](#algemeen)
  - [Inhoud](#inhoud)
  - [Concept](#concept)
    - [Flow chart](#flow-chart)
    - [Verhaallijn](#verhaallijn)
  - [Onderdelen](#onderdelen)
    - [Fitnesstracker](#fitnesstracker)
    - [Morsecode](#morsecode)
    - [5G](#5g)
    - [Vaccintester](#vaccintester)
    - [Alohamora](#alohamora)
    - [‘t Is beter op anderhalve meter](#t-is-beter-op-anderhalve-meter)
    - [Ontsmetting](#ontsmetting)
  - [FAQ's](#faqs)
    - [Wat als een puzzel niet werkt?](#wat-als-een-puzzel-niet-werkt)


## Concept

### Flow chart

![flow chart](general_flowchart.png)

Elke deelpuzzel zal één digit van de code geven. Deze code kan dan gebruikt worden om de Alohomora puzzel op te lossen. Ondertussen zullen er steeds enkele maatregelen moeten gerespecteerd worden.

### Verhaallijn

Het is eind 2020. België gaat voor de tweede keer in lockdown. Vele mensen zijn hopeloos en de sfeer wordt slechter met de dag. Er is echter één lichtpuntje. Het vaccin is in de laatste fase van ontwikkeling. De hele wereld wacht in volle spanning af tot het middel klaar is.

Echter is niet iedereen zo enthousiast. Een groep anti vaxxers heeft het labo gesaboteerd door de stroom af te sluiten. Hierdoor is de ontwikkeling stil gevallen. Het is aan jullie om de productie herop te starten en het perfecte recept te vinden. Slagen jullie erin om dit binnen de tijd te doen?

Een goed vaccin moet ook getest worden. Dit klinisch onderzoek wordt gedaan een groep van onderzoekers, zij zijn het kloppende hart van dit labo. Door de sabotage is hier ook de communicatie weg gevallen. Ontvang de geheime boodschap en breng deze over naar het testteam zodat het klinisch onderzoek kan verder gaan.

De wetenschappers proberen de anti vaxxers te stoppen door één van hun geruchten te ontkrachten namelijk “ 5G veroorzaakt Corona”. Om de mensen terug aan hun zijde te krijgen sturen de wetenschappers een onbemande missie naar het punt met het meeste straling. De anti vaxxers proberen de wetenschappers te stoppen door de besturing te hacken. Als de wetenschappers de missie voltooid hebben krijgen ze hulp uit onverwachte hoek. Die hun een sleutel geeft die hun kan helpen bij het ontwikkelen van een vaccin.


## Onderdelen

### Fitnesstracker

We plaatsen een motionsensor aan een kettlebell en deze wordt dan gebruikt om de oefeningen te tracken. Ergens in de kamer wordt een boek of poster voorzien met nummers op met daarbij een reeks van bijhorende oefeningen. Hieruit kunnen de spelers afleiden welke serie van oefeningen ze moeten uitvoeren. 
Als de spelers een oefening juist uitvoeren, laadt de batterij op en stijgt het balkje op de display. De batterij mag niet leeg zijn indien men andere puzzels wenst uit te voeren. Als de spelers een volledige oefeningenreeks correct uitvoeren, wordt een getal vrijgegeven van het digitaal cijferslot en gaat de telefoon van morsecode af. Als de spelers een incorrecte beweging uitvoeren daalt het balkje. Doorheen het spel wordt er energie verbruikt en krimpt het balkje dus. Eenmaal de batterij helemaal leeg is, wordt dit met de andere puzzels gedeeld via de broker. 

### Morsecode
Zodra de fitnesstracker puzzel opgelost is zal er een telefoon beginnen rinkelen. Wanneer de spelers van de escape room deze telefoon hebben kunnen lokaliseren kan deze opgenomen worden door op de bijhorende knop te drukken. Eenmaal de knop ingedrukt is geweest wordt de telefoon beschouwd als opgenomen. 
De telefoon zal dan een morse code afspelen. Deze morse code zal beginnen met een lage toon om zo aan te geven dat de morse code van start gaat. Aan de andere kant van de kamer moet deze code worden nagefloten in de micro.
Wanneer de juiste morse sequentie wordt nagefloten zal er een cijfer verschijnen op de display, wat handig zal uitkomen voor de oplossing van het cijferslot, en zullen de virologen aan de hand van een tip (ook op display) verder kunnen met de volgende proef; 5G.


### 5G

Wanneer de morsecode puzzel voltooid is, zal er een signaal verstuurd worden over de broker waardoor de afstand tot de stralingslocatie zal weergegeven worden (aan de hand van een led-bar) en deze kan gevonden worden. De auto is bestuurbaar met een afstandsbediening die communiceert over een frequentie van 433MHz. De besturing wisselt na een bepaalde tijd: de knop die eerst rechts was, kan nu bijvoorbeeld rechtdoor worden.
Ergens in de ruimte zal een ESP32 verstopt zijn, dit is dus de stralingslocatie. De afstand tussen die locatie en de auto zal aan de hand van de sterkte van een wifi signaal gemeten worden en zal weergegeven worden op de led-bar. De auto en de led-bar communiceren via esp now. Als de auto dicht genoeg is (en dus alle ledjes branden) en er op de drukknop gedrukt wordt, zal er een kluis(in de auto) openen met de RFID-tag waarmee de volgende puzzel, de vaccintester, kan gestart worden.
Als de auto wordt opgenomen (dit wordt bepaald aan de hand van een afstandssensor aan de onderkant van het wagentje) zal er een buzzer afgaan en wordt de auto, bij wijze van straf, onbestuurbaar, en zal de led-bar niet meer werken. Tevens zal er ook een buzzer weerklinken.  Dit is eveneens dezelfde werkwijze die wordt toegepast als de handen ontsmet moeten worden. Afgezien van de buzzer.
De digit die nodig is voor het slot (Alohamora) zal kunnen afgeleid worden aan de hand van een kaart die in de ruimte aanwezig is: de coördinaten van de stralingslocatie op deze kaart zal overeenkomen met het juiste cijfer.


### Vaccintester

Het enige wat ons nog kan redden van corona is het vormen van een vaccin. Wetenschappers zitten dag in dag uit opgesloten in een lab, zoekende naar iets wat deze stille moordenaar kan bestrijden. Hoe voelen die wetenschappers zich gedurende deze periode? Dit alles kan ervaart worden in deze puzzel.

Ze zitten opgesloten in een lab en pas als ze een werkend vaccin gevonden hebben kunnen ze het lab verlaten. Maar hoe wordt dit vaccin gemaakt? Het vaccin wordt gemaakt aan de hand van een recept dat enkel beschikbaar is voor de bevoegde wetenschappers. Om dat recept te volgen moeten stoffen in verschillende gemengd worden. Als het vaccin geproduceerd is wordt het laatste cijfer van het cijferslot verkregen en kunnen de wetenschappers eindelijk het lab verlaten en hun normale leven weer opnemen! 

### Alohamora

In alle voorgaande proeven zijn er in totaal 4 cijfers verkregen, deze vormen in de volgorde waarin ze gevonden zijn een code. Wanneer we deze cijfers in volgorde ingeven op de touchscreen kan een bakje geopend worden. In dit bakje zit de sleutel om te ontsnappen uit de kamer. Hoera vrijheid!

### ‘t Is beter op anderhalve meter

De onderzoekers moeten voorkomen dat ze zelf ten prooi vallen van het virus. Ze moeten dus veilig werken door steeds 1,5 meter afstand van elkaar te houden. Dit wordt geïmplementeerd door aan elke onderzoeker een mobiele node mee te geven die continu controleert of de onderzoekers niet te dicht bij elkaar komen te staan.
Wanneer dit systeem een te dichte afstand detecteert, zal er een duidelijk alarmsignaal afgaan en zullen alle onderzoekers zich moeten gaan ontsmetten. Alle opdrachten worden direct gepauzeerd totdat elke onderzoeker zijn/haar handen heeft ontsmet. Daarna kan er verder worden gewerkt. 

### Ontsmetting

Zodat de Onderzoekers verder kunnen werken wanneer ze te dicht bij mekaar zijn gekomen moeten ze hun handen ontsmetten. Werk kan dus maar verdergaan wanneer dit gebeurd is.
Via een slimme ontsmettingsdispenser moet er dus bijgehouden worden of alle onderzoekers hun handen ontsmet hebben en of elke individuele onderzoeker dit gedaan heeft. Dit wordt gedaan via een nfc badge die elke onderzoeker moet scannen om ontsmetting te krijgen. De dispenser werkt volledig zonder fysiek contact aangezien deze via een afstandssensor werkt.

Wanneer elke individuele onderzoeker zijn handen ontsmet heeft is het weer veilig om verder te werken waardoor de andere puzzels terug hervat kunnen worden.

## Centraal systeem

### Hardware

Als centraal deel van ons systeem hebben we een broker/server en een router. Als broker/server gebruiken we een Raspberry Pi 2 met een aantal programma's, meer uitleg hierover is te vinden onder het onderdeel [framework](https://project-es-20-21.github.io/General/docs/framework.html). 
Als router mochten we gebruik maken van een router van het merk NETGEAR, waar we verder niks aan hebben aangepast. 

### Software

Ook hierover wordt er verder uitgewijd onder het onderdeel [framework](https://project-es-20-21.github.io/General/docs/framework.html).

### Communicatie

De communicatie verloopt steeds via MQTT, behalve dan enkele interne communicaties. Elke proef communiceert met de andere proeven waar nodig, de broker is dus slechts een tussenstation maar implementeert zelf (bijna) geen functionaliteit. Dit heeft als voordeel dat ook de broker makkelijk vervangen of uitgebreid kan worden, zonder de hele werking van de escape room in gevaar te brengen. Verdere uitleg omtrent communicatie bevindt zich [hier](https://project-es-20-21.github.io/General/docs/framework.html#communicatie).

## FAQ's

### Wat als een puzzel niet werkt?

Aan de hand van een user interface (gemaakt met behulp van Node-RED) kan op een lokale webpagina, elke proef apart of alle proeven samen, bestuurd worden via MQTT berichten. Via een smartphone kan dus bijvoorbeeld alles gemanipuleerd worden. Zo kunnen bijvoorbeeld missende digits alsnog doorgegeven worden, elk deel kan gereset worden,... Bij voorkeur is de escape room volledig op te lossen met de aanwezige elektronica en waar nodig met een tip.
