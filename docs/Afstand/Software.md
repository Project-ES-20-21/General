---
layout: default
title: Software
parent: Afstand
grand_parent: Onderdelen
nav_order: 2
---

# Software
## Flowchart
![flowchart](bachproef_flowchart_afbeelding.png)
## Variabelen
Nog voor we overgaan naar de bespreking van de belangrijke methoden, de setup en de loop die eigen zijn aan elk arduinoprogramma worden enkele variabelen uitgelegd. 
### BLE en buffer
We stellen een grenswaarde in, als de RSSI waarde kleiner is dan deze waarde dan is er overtreding. Er worden ook vier tellers ("teller0", "teller1", "teller2" en "teller3") aangemaakt en vier buffers ("buffer0", "buffer1", "buffer2" en "buffer3"). De grootte van de buffers wordt ook meegegeven in de "size" variabele. De "send_to_broker" boolean, die ingesteld wordt op true, geeft aan of een spelers een overtreding hebben begaan en zich nog moeten ontsmetten, als deze op true staat dan betekent het dat de spelers hun handen hebben ontsmet sinds de vorige overtreding. De "buzzerPin" is pin 15. Er wordt ook een object uit de BLEScan library aangemaakt, "\*pBleScan" en een object uit de BLECast klasse "bleCast(esp_naam)". De size wordt ingesteld op 20.
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
We stellen het wachtwoord in en het ssid, via de variabelen "password" en "ssid". Het ip-adres van de MQTT-server wordt meegegeven via de variabele mqtt_server. We gebruiken de WiFiClient bibliotheek voor Wi-Fi en PubSubClient bibliotheek voor MQTT. Uit elke bibliotheek maken we een object aan. Het WifiClient "Afstand_X" object en het PubSubClient "client(Afstand_X)" object. 

```c
const char* ssid = "NETGEAR68";
const char* password = "excitedtuba713";
const char* mqtt_server = "192.168.1.2";

const char* piepkanaal = "esp32/afstand/piep/0";

WiFiClient Afstand_0;
PubSubClient client(Afstand_0);
```


### Piep 
We stellen een wachttijd in, een boolean die aangeeft of de buzzer aan het piepen is en het begintijdstip wanneer de buzzer begint met piepen wordt ook bijgehouden. De boolean "beginPiep" geeft aan of de buzzer moet beginnen met piepen. De wachttijd wordt gelijk gesteld aan 180000 milliseconden, dus als een speler zijn handen heeft ontsmet dan kan hij/zij 3 minuten geen overtreding begaan, deze cooldown wordt ingesteld omdat de speler anders veel te veel tijd verliezen aan het ontsmetten van de handen.
```c
int wachttijd = 180000;
int cooldown = -wachttijd;
bool piepActief = false;
int beginTijdstip = 0;
bool beginPiep = false;

```


## Setup
In de setup methode die eenmaal wordt uitgevoerd, zorgen we ervoor dat alles klaar staat om vervolgens RSSI signalen uit te sturen, te ontvangen en te verwerken. 
### Wi-Fi
Als eerste wordt de Wi-Fi verbinding opgesteld, hiervoor wordt de methode setup_wifi() gebruikt. In deze methode probeert de ESP32 te verbinden met het Wi-Fi netwerk dat wordt meegegeven in de variabele ssid met het paswoord in de variabele password. Vervolgens wordt gecontroleerd of de ESP32 verbonden is of niet. Zolang de status van de Wi-Fi niet verbonden is zullen er "." geprint worden terwijl er gewacht wordt. Deze methode geeft niets terug.
```c
void setup_wifi() {
  ...
}
```
### MQTT
Nadat de ESP32 verbonden is met het netwerk, wordt de MQTT client opgesteld. Er wordt wel nog geen verbinding gemaakt met de MQTT server. Dit gebeurt pas in de loop methode. De callback functie wordt ook toegewezen aan de client om de MQTT ontvangen berichten te verwerken.
```c
    client.setServer(mqtt_server, 1883);
    client.setCallback(callback);
```
### BLE
Vervolgens wordt in de setup() methode alles ingesteld om BLE signalen te verzenden en ontvangen. We maken nieuwe scan aan. Aan deze scan linken we via de setAdvertisedDeviceCallbacks methode een object van de klasse AdvertisedDeviceCallBack met de methode in die de resultaten van de scan continu verwerkt. We zetten de boolean wantDuplicates op true, we willen heel de tijd resultaten ontvangen van dezelfde apparaten, zodat we continu de afstand kunnen bepalen. Er worden een scantijd van 99 milliseconden ingesteld met een pauze van 1ms tussen de scans door. Hoe meer vermogen de BLE signalen hebben des te better de verwerking van de resultaten, daarom wordt het scan_type ingesteld op active via de methode setActiveScan(true) van de BLEScan klasse. Bij het uitzenden van signalen willen we dat de ESP32 een naam heeft, dit werd ingesteld bij de initialisatie van het bleCast object. Via de methode bleCast.begin() worden een aantal parameters ingesteld. Het vermogen van de signalen wordt ook manueel nog eens ingesteld door "esp_ble_tx_power_set(ESP_BLE_PWR_TYPE_ADV,ESP_PWR_LVL_P9)" te doen.
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
Omdat er veel waarden worden gemeten op korte tijd met uitschieters die niet gewenst zijn wordt een circulaire buffers gebruikt zodat het gemiddelde van de buffer kan worden genomen. De buffers worden geinitialiseerd in de setup via de methode initBuffers(size), met size als instelbare variabele. 
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
De loop methode die heel de tijd wordt overlopen heeft drie functies, zorgen dat de verbinding met de MQTT server actief blijft , BLE scans starten en stoppen, en als de andere twee functies niet uitgevoerd worden en er is een MQTT bericht, wordt de MQTT callback opgeroepen in de client.loop() methode. 
### BLE scan
Terwijl een scan wordt uitgevoerd voor een duratie van 1 seconde, kan de BLE callback ,die in de setup gekoppeld werd aan de scan, opgeroepen worden. Terwijl een scan wordt uitgevoerd kan de MQTT callback niet opgeroepen worden of de status van de MQTT verbinding niet gecontroleerd worden. Daarom wordt niet heel de tijd gescand (dit kan door de scan tijd op nul te zetten , "BLEScanResults foundDevices = pBLEScan->start(0);").
```c
    Serial.println("Scanning:");
    BLEScanResults foundDevices = pBLEScan->start(1);
    pBLEScan->clearResults();
```

### MQTT reconnect
De ESP32 moet constant verbonden zijn met de MQTT server. Als dit niet het geval is, dus als client.connected false is dan moet er herverbonden worden met de server. De methode reconnect() wordt uitgevoerd. In deze methode wordt eigenlijk alleen maar een while lus overlopen, als de ESP32 geen verbinding kan maken  na 5 seconden wordt deze opnieuw opgestart. In deze methode subscriben we ook op de juiste kanalen, het controle kanaal en het piepkanaal van de ESP32. Het piepkanaal zorgt ervoor dat beide ESP's sowieso beginnen met piepen, zelfs al detecteert maar één ESP een overtreding. 

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
Deze callback zorgt voor de verwerking van de resultaten van de BLE scan. Eerst wordt de naam van de verzender van het ontvangen signaal vergeleken met een aantal string zodat we zeker zijn van welke andere ESP32 het signaal komt. De RSSI waarde van het signaal wordt in de buffer gestoken en een teller wordt verhoogd. Als de teller gelijk is aan de buffer grootte, wat betekent dat elke waarde vernieuwd is ten opzichte van de vorige keer dat de teller gelijk was aan de buffer grootte, er zijn dus buffer grootte aantal nieuwe RSSI waarden gemeten, dan kunnen de resultaten uit de buffer verwerkt worden. De teller wordt terug op nul gezet. Als de send_to_broker variabele true is, de wachttijd van de vorige overtreding gedaan is en er wordt een overtreding begaan, het gemiddelde van de buffer is dus kleiner dan de opgestelde grenswaarde, dan moet het alarm afgaan. De send_to_broker parameter is een variabele die weergeeft of dat de personen zich hebben ontsmet sinds de vorige fout. Er is een overtreding gededecteerd, er moet dus een melding gestuurd worden naar de ontsmettingsgroep met daarin de id's van de overtreders. BeginPiep wordt op true gezet.Op het einde van de callback functie word een non blockingPiep methode opgeroepen. Als beginPiep op true staat zal deze functie het alarm laten afgaan.

```c
class AdvertisedDeviceCallbacks : public BLEAdvertisedDeviceCallbacks{
  
  void onResult(BLEAdvertisedDevice advertisedDevice){
    
    ...
  
  piepNonBlocking();
  }
  
}
```

## piepNonBlocking()
In deze functie staan twee if statements. De ene if kijkt of het alarm moet afgaan en de buzzer moet geactiveerd worden, de andere kijkt of de buzzer aan staat en of deze uitgezet mag worden. De voorwaarden voordat een ESP32 module mag beginnen met piepen (buzzerPin hoog) is dat de buzzerPin laag staat (er is nog geen gepiep) en dat er een overtreding is gebeurd die lang genoeg na de vorige overtreding is gebeurd. Als er aan deze voorwaarden worden voldaan dan wordt de buzzerPin hoog gezet, wordt de timer die bijhoudt hoelang het geleden is dat er een nieuwe piep was ingesteld op de huidige tijd en worden via de methode stuurAlarm() de andere onderdelen van de escaperoom gealarmeerd.
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
## stuurAlarm()
Deze methode zorgt ervoor dat de andere onderdelen van de escaperoom op de hoogte worden gesteld van de overtreding door een '1' te sturen naar hun controle kanaal. Als de '1' goed verwerkt wordt zouden de puzzels niet verder kunnen worden opgelost tot de handen ontsmet zijn. 
## MQTT
De MQTT callback functie wordt in de loop opgeroepen via client.loop(). Als er een bericht is op een kanaal waarop gesubscribed is dan zal de callback dit bericht verwerken. Eerst wordt gekeken op welk kanaal het bericht is. 

Als een bericht verstuurd is naar ons controle dan moet gecontroleerd worden of het bericht  "1","2" of "3" is. Als er een "0" binnenkomt moet de ESP32 herstart worden, bij "1" wordt de werking stopgezet. Bij "2" mag de werking verdergaan, de boolean send_to_broker wordt op true gezet en de timers worden opnieuw ingesteld, een twee betekent dus dat de handen van de overtreders ontsmet zijn.

Als een bericht verstuurd is naar het interne piepkanaal van de ESP, dan moet het alarm afgaan ,als de cooldown is verstreken.




