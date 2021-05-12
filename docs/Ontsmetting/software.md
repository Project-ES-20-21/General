---
layout: default
title: Software
parent: Ontsmetting
grand_parent: Onderdelen
nav_order: 1
---

# Software

Het gehele project wordt bestuurd via een ESP32 module. Deze module kan geprogrammeerd worden via Arduino flavoured C en C++. Alle geschreven software staat opgeslaan op de [algemene reopository](https://github.com/Ontsmettinator3000/main) van onze [organisatie](https://github.com/Ontsmettinator3000). Bij de start van de ontwikkeling kozen we er voor om het geheel "modulair" maken. Hierbij splitsten we de verschillende onderdelen op in aparte klasses. Dit deden we om de main loop leesbaar en overzichtelijk te maken. Ook is het hierdoor makkelijk om dingen uit te breiden of te overbruggen bij foutieve werking.

## Setup en main loop

Zoals elk Arduino programma, zal ook onze code lopen vanuit een loop. Hierbij zal eenmaal de setupmethode overlopen worden alvorens de loop te starten. In de setup zullen we de verschillende onderdelen juist configureren. Zo zullen we de wifi en MQTT verbinding instellen, alsook enkele pinModes definiëren. Ook de OTA connectie wordt hier ingesteld.

De main loop is een vertaling van de [algemene flowchart](index.md#Blokschema) in code. Eerst en vooral zullen we loop functies hebben voor verschillende klasses. Deze zullen zeer frequent opgeroepen worden. Vaak zullen deze functie bepaalde informatie ophalen of dingen updaten. Hierna zullen we eerste test doen. Merk op dat in deze fase alle andere onderdelen uitgeschakeld zijn door de setup. Indien een alarm ontvangen is, zullen we het scherm updaten en de NFC handler inschakelen. We bevinden ons nu in fase twee van het proces. Hierbij zal een gescande kaart de login functie oproepen. Deze zal testen of de tag al dan niet correct is en eventueel reeds geregistreerd. Als dit onderdeel doorlopen is, rest er nog enkel de alcohol te laten lopen. Dit zal gebeuren nadat de scanner een hand geregistreerd heeft via de scanner klasse. Indien deze cyclus doorlopen is voor iedere besmette speler, zal een OK signaal verzonden worden naar de broker.

Doorheen de code mag er geen gebruik gemaakt worden van blocking code, deze zal namelijk zorgen voor een vertraagde/foute OTA. Het gebruik van millis() bij tijdsmetingen is dus vereist. Ook zal de code niet blokkeren bij een fout in de setup. Er zal dus steeds vanuit de setup naar de loop gegaan worden, onafhankelijk of deze voorafgaande setup succesvol was. Hierdoor is programmeren via WiFi op elk moment mogelijk.

## Config

Tijdens het hele ontwikkelingsproces moeten er veel parameters bepaald worden. We kozen ervoor om alle instellingen te centraliseren in één document. Hierdoor konden we in één oogopslag alle instellingen bekijken en snel aanpassen. Dit zorgt er ook voor dat we verschillende parameters in meerdere klasses tegelijk kunnen gebruiken zonder deze meerdere keren te moeten definiëren.

## IR sensor

De IR sensor klasse is relatief klein. In de eerste versie van de software, maakten we gebruik van interrupts. Later bleken deze in onze toepassing niet stabiel genoeg. Hierdoor schakelden we over naar een if() statement in de loop. Deze zal pollen of de sensor al dan niet geactiveerd is. Later bleek de instabiliteit eerder te liggen aan het omgevingslicht. We schakelden niet meer terug naar interrupts. De polling gebeurt in de loop functie van de klasse. Indien de sensor (softwarematig) ingeschakeld wordt, zal bij de detectie van een hand de boolean handDetected op true gezet worden. Deze kan men in de main loop gebruiken als paramater om te pompen. Deze kan men ophalen op volgende manier:
```c
if (handDetector.handDetected)
  {
      ...
  }
```

### Functies
De klasse heeft enkele kleine functies.
##### setup()
De setup functie dient aan het begin van de code opgeroepen te worden. Deze zal zorgen voor de juiste pinmode van de inputpin.
##### enable() en disable()
Via deze twee functies kan men de sensor in en uitschakelen. Na het uitschakelen van de sensoren zullen geen handen meer gedetecteerd worden tot deze weer is ingeschakeld.
##### loop()
Functie die regelmatig dient opgeroepen te worden. Deze zal de handdetectie vertalen in een boolean. Indien een hand gedetecteerd wordt, zal deze onthouden worden tot de sensor wordt uitgeschakeld. Meerdere keren handen na elkaar scannen heeft dus geen zin indien er tussenin niet wordt uitgeschakeld.


## Scherm

Voor de visuele indicatie maken we gebruik van een LCD scherm. Dit scherm zal communiceren via SPI. Deze communicatie wordt softwarematig verwezenlijkt door enkele libraries. We zullen steeds rechtstreeks gebruik maken van de Adafruit_ILI9341 library. Deze is speciaal ontwikkeld voor het type scherm dat we gebruiken. Het pakket zal gebruik maken van de algemene Adafruit_GFX library voor het genereren van de beelden. Bij het aanmaken van de klasse moeten een Adafruit_ILI9341 scherm object aangemaakt worden. Dit kan al dan niet statisch gebeuren via volgende constructor:
```c
static Adafruit_ILI9341 tft = Adafruit_ILI9341(TFT_CS, TFT_DC, TFT_RST);
```
Hierbij worden volgende IO pin nummers mee gegeven:

| `TFT_CS`    | clock select pin   |
| `TFT_DC`    | data / command pin |
| `TFT_RESET` | reset pin          |

### Functies
Het gebruik van de Adafruit bibliotheek zal het afbeelden van vormen en figuren sterk vergemakkelijken. Er verschillende functies in de Scherm klasse, die verantwoordelijk zijn voor het tekenen/afbeelden van bepaalde "scenes". 
##### setup()
Moet worden opgeroepen in het begin van de code om het scherm te configureren.

##### loop()
Functie die tijdens het lopen van het programma geregeld moet opgeroepen worden. Deze wordt gebruikt voor een niet blokkerende timer.

##### paintCross(int positie)
Hiermee kan men de indicator voor een personen op een kruisje zetten. Dit betekend dat de persoon niet ontsmet is. Het positie argument zal de plaats bepalen waar het icoon afgebeeld wordt. De positie is een int van 0 tot en met 3. De plaatsing is als volgt:<br>
>>![volgorde paint](volgorde_paint.png)

##### paintCheck(int positie)
Deze functie zal checks (vinkjes) afbeelden bij bepaalde personen. De plaatsing van de icoontjes zal op dezelfde manier als paintCross() gebeuren.
##### paintGevaar()
Paint gevaar zal een gevarendriehoek centraal op het scherm afbeelden. Hierbij wordt gebruikt gemaakt van de ingebouwde shape functies van de Adafruit library. Hierdoor zal het gebruikte geheugen beperkt worden.
##### clear()
Hierdoor zal het scherm volledig overtrokken worden met een witte kleur. Let op: een clear zal het scherm niet uitschakelen, indien energieverbruik beperkt moet worden maakt men best gebruik van de enable functionaliteit van het scherm.
##### validTag()
Functie die het scannen van een geldige tag zal visualiseren.
##### clearTag()
Functie die de bovenstaande visualisatie wist.


## Login
De registratie en authenticatie van de verschillende (besmette) spelers gebeurd via deze klasse. Deze personen worden geïdentificeerd via een nfc tag. Deze tag heeft een uniek id dat kan worden uitgelezen. De geldige tags zijn gekoppeld aan een nummer vanaf 0 tot en met 3. De unieke id's van de tags zijn opgeslagen in een extern document genaamd "*validTags.h*". Dit document staat in de include folder en heeft volgende structuur:
```c
int length = 4;
String tags[4] = {"0X12 0X34 0X56 0XC78 0X00 0X00 0X00",
                  ...
                  "0X9A 0XBC 0XDE 0XF0 0X00 0X00 0X00"};
```
De int `length` zal het aantal tags bijhouden om achteraf makkelijk alle tags te kunnen overlopen. De array van Strings `tags[]` zal de id's van de tags bijhouden. Men kan hierbij het aantal tags aanpassen. Het nummer van de tag (en bijhorend id) zal bepaald worden door de index van id. Indien de id van een bepaalde tag als eerste in de array staat, zal deze dus id 0 krijgen.

Het is belangrijk dat elke id uit 7 hexadecimale delen bestaat, gescheiden door een spatie. Indien de tag een korter id gebruikt, wordt dit aangevuld met 0X00. Ook moet zowel de X als de letters van de getalwaarde (A tot F) van het hexadecimale getal steeds in hoofdletter geschreven worden.
## Monitor
## MQTT
## NFC
## Speaker