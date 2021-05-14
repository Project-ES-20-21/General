---
layout: default
title: Hardware
parent: Morse
grand_parent: Onderdelen
nav_order: 2
---

## Software
Voor het project wordt gebruik gemaakt van een ESP32-chip. 

### void setup()
Zoals elk Arduino programma zal de code vertrekken vanuit de setup()-methode. Deze methode wordt 1 keer gerunt, het bevat informatie voor de opstart van het programma. Variabelen, pin modes, wifi-instellingen ... worden hier geïnitialiseerd. 

Eerst en vooral verhogen we hier de baud rate. Voor ons project verhogen we het maximum aantal symbolen per seconde naar 115200. Het ROM bootloader van de ESP32 communiceert namelijk met deze snelheid. Wanneer we de baud rate niet zouden aanpssen, kan de inkomende data niet correct gelezen worden door de UART.

In beide onderdelen wordt ook een debounce tijd voorzien voor de button, hiervoor wordt het commando _setDebounceTime(x)_ gebruikt. Deze methode wordt bij elk gebruik van de button toegepast. Bij de activatie van de button (van lown naar high of andersom), zal deze elk binnenkomend signaal voor x milliseconden negeren. 

Om te kunnen werken met de broker via MQTT, moet ook een wifi-setup() doorlopen worden. Deze methode wordt ook opgeroepen in de setup()-methode. _setup_wifi()_ wordt dus 1 keer uitgevoerd. Verder wordt ook de MQTT-server en MQTT-poort geïnitialiseerd via de methode _client.setServer(MQTT_SERVER, MQTT_PORT)_

### void setup_wifi()
In deze setup gebeuren de wifi-instellingen. Via de methoden _WiFi.begin(SSID, PWD)_ wordt de Service Set IDentifier meegegeven (dit is de naam voor een bepaald netwerk met een IP-adres), én het bijhorende paswoord van dit WiFi-signaal. 
Ter controle vragen we aan het einde van de methode het IP-adres waarmee werd verbonden via de methode _WiFi.localIP()_.

Na deze setups kan het programma beginnen aan de loop()-methode. Deze methode zal telkens opnieuw herhaald worden. Het eerste wat wordt gedaan is nakijken of nog steeds verbonden is met de MQTT-server. Als dit (nog) niet het geval is, zal via _reconnect()_ (opnieuw) worden geconnecteerd met de MQTT-server. Deze methode zal blijven doorlopen worden zolang er niet geconnecteerd werd. Wanneer er connectie gelegd werd, zal de client ook onmiddellijk subscriben op de nodige kanalen. 

Vanaf dat deze connectie op punt staat, kan de rest van de code worden uitgevoerd. Het verdere verloop is verschillend voor de speaker en de micro. Deze onderdelen worden verder dus apart besproken.

### Speaker
### Micro
Het blokschema geeft een overzicht van het verloop van de code.

![](https://raw.githubusercontent.com/BachMorse/Documentatie/master/BlokschemaCodeMicro.JPG)



Bij de micro maken we gebruik van de waarde 4095. Dit is de maximum waarde die de microfoon kan doorsturen wanneer het een luid signaal detecteert. Het komt overeen met 3.3V en het is een 12bit-signaal. Er wordt gekeken naar de som van de vorige 100 samples, wanneer deze een bepaalde grens overschrijdt, zal de code dit zien als een kort/lang signaal. De detectie van een kort of een lang signaal wordt toegevoegd aan een andere array. 
Omdat we de som nemen, zal er bij een lang signaal eerst een kort signaal gedetecteerd worden. Ook kan het zijn dat een kort signaal van de speler iets langer duurt dan het opgegeven kort signaal, zonder controlevoorwaarde, worden er dan meerdere korte signalen gedetecteerd. Om dit te vermijden voerden we een controlevoorwaarde in. Ook de stiltes moeten dan gedetecteerd worden om 2 korte signalen na elkaar mogelijk te maken.


