---
layout: default
title: Software
parent: Afstand
grand_parent: Onderdelen
nav_order: 2
---

# Software
Link naar de [algemene repository](https://github.com/blijf-weg/Afstand_BLE.git) waar alle code die hier besproken wordt instaat.
- [Flowchart](#Flowchart)
- [Variabelen](#Variabelen)
- [Setup](#Setup)
- [Loop](#Loop)
- [BLE Callback](#BLE-Callback)
- [piepNonBlocking](#piepNonBlocking)
- [stuurAlarm](#StuurAlarm)
- [MQTT](#MQTT)
## Flowchart
![flowchart](bachproef_flowchart_afbeelding.png)
## Variabelen
Nog voor we overgaan naar de bespreking van de belangrijke methoden, de setup en de loop die eigen zijn aan elk arduinoprogramma worden enkele variabelen uitgelegd. 
### BLE en buffer
We stellen een grenswaarde in, als de RSSI waarde kleiner is dan deze `waarde` dan is er overtreding. Er worden ook vier tellers (`teller0`, `teller1`, `teller2` en `teller3`) aangemaakt en vier buffers (`buffer0`, `buffer1`, `buffer2` en `buffer3`) uit de "circ-buffer.h" klasse. De grootte van de buffers wordt ook meegegeven in de `size` variabele. De `size` wordt ingesteld op 20. De `send_to_broker` boolean, die ingesteld wordt op true, geeft aan of een spelers een overtreding hebben begaan en zich nog moeten ontsmetten, als deze op true staat dan betekent het dat de spelers hun handen hebben ontsmet sinds de vorige overtreding. De `buzzerPin` is pin 15. Elke speler heeft ook een speler nummer, dit nummer wordt meegegeven aan de variabele `esp_naam` (speler 0 krijgt de naam "Afstand_0", speler 1 "afstand_1", enzovoort). Er wordt ook een object uit de BLEScan library aangemaakt, `*pBleScan` en een object uit de BLECast klasse `bleCast(esp_naam)`. 
```c
double waarde = -32;

int teller0 = 0;
int teller1 = 0;
int teller2=0;
int teller3=0;

CircBuffer buffer0;
CircBuffer buffer1;
CircBuffer buffer2;
CircBuffer buffer3;

int size = 20;

bool send_to_broker = true;

const char* esp_naam = "Afstand_0";

BLEScan *pBLEScan;
BLECast bleCast(esp_naam);
``` 

### Wi-Fi en MQTT
We stellen het wachtwoord en het ssid  in via de variabelen `password` en `ssid`. Het ip-adres van de MQTT-server wordt meegegeven via de variabele `mqtt_server`. We gebruiken de WiFiClient bibliotheek voor Wi-Fi en PubSubClient bibliotheek voor MQTT. Uit elke bibliotheek maken we een object aan. Het WifiClient `Afstand_0`  object en het PubSubClient `client(Afstand_0)` object ("0" aanpassen naar het correcte nummer voor een ander speler, dit moet overeenkomen met het laatste cijfer van `esp_naam`). 

```c
const char* ssid = "NETGEAR68";
const char* password = "excitedtuba713";
const char* mqtt_server = "192.168.1.2";

const char* piepkanaal = "esp32/afstand/piep/0";

WiFiClient Afstand_0;
PubSubClient client(Afstand_0);
```


### Piep 
We stellen een wachttijd in, een boolean die aangeeft of de buzzer aan het piepen is en het begintijdstip wanneer de buzzer begint met piepen wordt ook bijgehouden. De boolean "beginPiep" geeft aan of de buzzer moet beginnen met piepen. De wachttijd wordt gelijk gesteld aan 180000 milliseconden, dus als een speler zijn handen heeft ontsmet dan kan hij/zij 3 minuten geen overtreding begaan, deze cooldown wordt ingesteld omdat de speler anders veel te veel tijd verliezen aan het ontsmetten van de handen en omdat speler die hun handen gaan ontsmetten dicht bij elkaar komen.
```c
int wachttijd = 180000;
int cooldown = -wachttijd;
bool piepActief = false;
int beginTijdstip = 0;
bool beginPiep = false;

```


## Setup
In de setup methode die eenmaal wordt uitgevoerd, zorgen we ervoor dat alles klaar staat om vervolgens RSSI signalen uit te sturen, te ontvangen en te verwerken. 
### Wi-Fi en MQTT
Als eerste wordt de Wi-Fi verbinding opgesteld, hiervoor wordt de methode setup_wifi() gebruikt. In deze methode probeert de ESP32 te verbinden met het Wi-Fi netwerk dat wordt meegegeven in de variabele ssid met het paswoord in de variabele password. Vervolgens wordt gecontroleerd of de ESP32 verbonden is of niet. Zolang de status van de Wi-Fi niet verbonden is zullen er "." geprint worden terwijl er gewacht wordt. Deze methode geeft niets terug.
```c
void setup_wifi() {
  ...
}
```
Nadat de ESP32 verbonden is met het netwerk, wordt de MQTT client opgesteld. Er wordt wel nog geen verbinding gemaakt met de MQTT server. Dit gebeurt pas in de loop methode. De callback functie wordt ook toegewezen aan de client om de ontvangen MQTT berichten te verwerken.
```c
    client.setServer(mqtt_server, 1883);
    client.setCallback(callback);
```
### BLE
Vervolgens wordt in de `setup()` methode alles ingesteld om BLE signalen te verzenden en ontvangen. We maken een nieuwe scan aan. Aan deze scan linken we via de `setAdvertisedDeviceCallbacks(...)` methode een object van de klasse AdvertisedDeviceCallBack met de methode in die de resultaten van de scan continu verwerkt. We zetten de boolean "wantDuplicates" op true, we willen heel de tijd resultaten ontvangen van dezelfde apparaten, zodat we continu de afstand kunnen bepalen. Er wordt een scantijd van 99 milliseconden ingesteld met een pauze van 1ms tussen de scans door. Hoe meer vermogen de BLE signalen hebben des te better de verwerking van de resultaten, daarom wordt het "scan_type" ingesteld op "active" via de methode setActiveScan(true) van de BLEScan klasse, "active" verbruikt meer batterij maar de resultaten zijn wel better. Bij het uitzenden van signalen willen we dat de ESP32 een naam heeft, dit werd ingesteld bij de initialisatie van het bleCast object. Via de methode bleCast.begin() worden een aantal parameters ingesteld. Het vermogen van de signalen wordt ook manueel nog eens ingesteld door `esp_ble_tx_power_set(ESP_BLE_PWR_TYPE_ADV,ESP_PWR_LVL_P9)` te doen.
```c
    BLEDevice::init("RSSI scan");
    pBLEScan = BLEDevice::getScan(); 
    pBLEScan->setAdvertisedDeviceCallbacks(new AdvertisedDeviceCallbacks(),true);
    pBLEScan->setActiveScan(true); 
    pBLEScan->setInterval(100);
    pBLEScan->setWindow(99); 
    bleCast.begin();
    if (esp_ble_tx_power_set(ESP_BLE_PWR_TYPE_ADV,ESP_PWR_LVL_P9) == OK) {
        Serial.println("Transmission power changed\n");
    }
```
### Buffers
Omdat er veel waarden ,met uitschieters die niet gewenst zijn, worden gemeten op korte tijd (er moet snel gemeten worden), worden circulaire buffers gebruikt zodat het gemiddelde van de buffer kan worden genomen. De buffers worden geinitialiseerd in de setup via de methode `initBuffers(size)`, met `size` als instelbare variabele. Er worden RSSI waarden gemeten t.o.v drie andere modules, er zijn dus drie andere buffers nodig. Omdat de code telkens naar vier verschillend ESP's geüpload worden, worden er direct vier buffers aangemaakt zodat deze niet elke keer opnieuw aangepast moeten worden.  
```c
    initBuffers(size);
```
```c
CircBufferStatus_t initBuffers(uint8_t size){
  ...
}
```
### Buzzer
We stellen ook de correcte pin in voor de buzzer. Dit is pin 15 voor het pcb-ontwerp.

## Loop
De `loop()` methode die heel de tijd wordt overlopen heeft drie functies, zorgen dat de verbinding met de MQTT server actief blijft , BLE scans starten en stoppen, en als de andere twee functies niet uitgevoerd worden en er is een MQTT bericht, de MQTT callback opgeroepen in de client.loop() methode. 
### BLE scan
Terwijl een scan wordt uitgevoerd voor een duratie van 1 seconde, kan de BLE callback ,die in de setup gekoppeld werd aan de scan, opgeroepen worden. Terwijl een scan wordt uitgevoerd kan de MQTT callback niet opgeroepen worden of kan de status van de MQTT verbinding niet gecontroleerd worden. Daarom wordt niet heel de tijd gescand (dit kan door de scan tijd op nul te zetten , "BLEScanResults foundDevices = pBLEScan->start(0);").
```c
    Serial.println("Scanning:");
    BLEScanResults foundDevices = pBLEScan->start(1);
    pBLEScan->clearResults();
```

### MQTT reconnect
De ESP32 moet constant verbonden zijn met de MQTT server. Als dit niet het geval is, dus als `client.connected()` "false" is dan moet er herverbonden worden met de server. De methode `reconnect()` wordt uitgevoerd. In deze methode wordt eigenlijk alleen maar een while lus overlopen zolang er geen verbinding kan gemaakt worden, als de ESP32 geen verbinding kan maken  na 5 seconden wordt deze opnieuw opgestart. In deze methode subscriben we ook op de juiste kanalen, het controle kanaal en het piepkanaal van de ESP32. Het piepkanaal zorgt ervoor dat beide ESP's sowieso beginnen met piepen, zelfs al detecteert maar één ESP van de twee overtreders een overtreding. 

```c
   if (!client.connected()) {
        reconnect();
    }
```
```c
void reconnect() {
  ...
}
```

## BLE Callback
Deze callback zorgt voor de verwerking van de resultaten van de BLE scan. Eerst wordt de naam van de verzender van het ontvangen signaal vergeleken met een aantal string zodat we zeker zijn van welke andere ESP32 het signaal komt. De RSSI waarde van het signaal wordt in de buffer gestoken en een teller wordt verhoogd. Als de teller gelijk is aan de buffer grootte, wat betekent dat elke waarde vernieuwd is ten opzichte van de vorige keer dat de teller gelijk was aan de buffer grootte, er zijn dus buffer grootte aantal nieuwe RSSI waarden gemeten, dan kunnen de resultaten uit de buffer verwerkt worden. De teller wordt terug op nul gezet. Welke teller en buffer er verandert wordt hangt af van welke ESP het signaal komt, als "Afstand_2" iets ontvangt van Afstand_0 dan zullen `teller0` en `buffer0` aangepast worden. Als de `send_to_broker` variabele "true" is, de wachttijd van de vorige overtreding gedaan is en er wordt een overtreding begaan, het gemiddelde van de buffer is dus kleiner dan de opgestelde grenswaarde, dan moet het alarm afgaan. De `send_to_broker` parameter is een variabele die weergeeft of dat de personen zich hebben ontsmet sinds de vorige fout. Er is een overtreding gededecteerd, er moet dus een melding gestuurd worden naar de ontsmettingsgroep met daarin de id's van de overtreders. `beginPiep` wordt op "true" gezet. Op het einde van de callback functie word de `non blockingPiep()` methode opgeroepen. Als `beginPiep` op "true" staat zal deze functie het alarm laten afgaan.

```c
class AdvertisedDeviceCallbacks : public BLEAdvertisedDeviceCallbacks{
  
  void onResult(BLEAdvertisedDevice advertisedDevice){
      beginPiep = false;
      
      ...
  
      piepNonBlocking();
  }
  
}
```

## piepNonBlocking
In deze functie staan twee if statements. De ene if kijkt of het alarm moet afgaan en de buzzer moet geactiveerd worden, de andere kijkt of de buzzer aan staat en of deze uitgezet mag worden. 
De voorwaarden voordat een ESP32 module mag beginnen met piepen (`buzzerPin` instellen op hoog/"1") is dat de `buzzerPin` laag/"0" staat (er is nog geen gepiep) en dat er een overtreding is gebeurd die lang genoeg na de vorige overtreding is gebeurd. Als er aan deze voorwaarden worden voldaan dan wordt de buzzerPin hoog gezet, wordt de timer ,die bijhoudt hoelang het geleden de module is beginnen met piepen, ingesteld op de huidige tijd en worden via de methode `stuurAlarm()` de andere onderdelen van de escaperoom gealarmeerd. 
De buzzer zal vijf seconden afgaan, er wordt dus gekeken of de huidige tijd (via `millis()`) min het "beginTijdstip" groter is dan 5000 milliseconden.
```c
void piepNonBlocking(){  
  if (!piepActief && beginPiep){
  ...
  }
  else if (millis() - beginTijdstip > 5000 && piepActief){
  ...
  } 
}
```
## stuurAlarm
Deze methode zorgt ervoor dat de andere onderdelen van de escaperoom op de hoogte worden gesteld van de overtreding door een "1" te sturen naar hun controle kanaal. Als de "1" goed verwerkt wordt zouden de puzzels niet verder kunnen worden opgelost tot de handen ontsmet zijn.  Er wordt ook een "1" gestuurd naar ons eigen controle kanaal. Omdat we niet willen dat ,terwijl de twee huidige overtreders hun handen ontsmetten, de andere speler ook in de buurt moeten komen om hun handen ook te moeten ontsmetten. De twee andere spelers kunnen dus geen overtreding begaan terwijl de andere spelers hun handen ontsmetten. 

```c
void stuurAlarm(){
    if (send_to_broker){
        client.publish("esp32/afstand/control", "1");
        client.publish("esp32/ontsmetten/control","1");
        client.publish("esp32/vaccin/control","1");
        client.publish("esp32/5g/control","1");
        client.publish("esp32/morse/control","1");
        client.publish("esp32/fitness/control","1");        
    }
}
```
## MQTT
De MQTT callback functie wordt in de loop opgeroepen via `client.loop()`. Als er een bericht is op een kanaal waarop gesubscribed is dan zal de callback dit bericht verwerken. Eerst wordt gekeken op welk kanaal het bericht is. 

Als een bericht verstuurd is naar ons controle kanaal (`esp32/afstand/control`) dan moet gecontroleerd worden of het bericht  "0","1" of "2" is. Als er een "0" binnenkomt moet de ESP32 herstart worden, bij "1" wordt de werking stopgezet door `send_to_broker` op "false" te zetten. Bij "2" mag de werking verdergaan, de boolean `send_to_broker` wordt op "true" gezet en de `cooldown` timer wordt opnieuw ingesteld, een twee betekent dus dat de handen van de overtreders ontsmet zijn.

Als een bericht verstuurd is naar het interne piepkanaal van de ESP dan moet het alarm afgaan ,als de cooldown is verstreken. De huidige overtreders gaan hun handen ontsmetten. Er moeten dus geen nieuwe overtredingen van deze spelers (en andere spelers) gedectecteerd worden, `send_to_broker` moet dus op "false" gezet worden. De buzzer moet beginnen met piepen voor vijf seconden, `beginPiep` wordt dus op "true" gezet.

```c
void callback(char* topic, byte* message, unsigned int length) {
    char test = (char)message[0];
    if (strcmp(topic, "esp32/afstand/control") == 0){
            if ('0' == test)
            ESP.restart();
        else if('2' == test){
            send_to_broker = true;
            cooldown = millis();
        }
        else if ('1' == test)
            send_to_broker = false;
    else {
        if (strcmp(topic, piepkanaal) == 0){
            if(strcmp(topic,piepkanaal) == 0){
                if(test == '1'){
                    beginPiep = true;
                    send_to_broker = false;
                    piepNonBlocking();
                    beginPiep = false;
                }
            }
        }
    }
}
```



