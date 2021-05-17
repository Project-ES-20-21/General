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
## Setup
In de setup methode die eenmaal wordt uitgevoerd, zorgen we ervoor dat alles klaar staat om vervolgens RSSI signalen uit te sturen, te ontvangen en te verwerken. 
### Wi-Fi
Als eerste wordt de Wi-Fi verbinding opgesteld, hiervoor wordt de methode setup_wifi() gebruikt. In deze methode probeert de ESP32 te verbinden met het Wi-Fi netwerk dat wordt meegegeven in de variabele ssid met het paswoord in de variabele password. Vervolgens wordt gecontroleerd of de ESP32 verbonden is of niet. Zolang de status van de Wi-Fi niet verbonden is zullen er "." geprint worden terwijl er gewacht wordt. 
### MQTT
Nadat de ESP32 verbonden is met het netwerk, wordt de MQTT client opgesteld. Er wordt wel nog geen verbinden gemaakt met de MQTT server. Dit gebeurt pas in de loop methode. De callback functie wordt ook toegewezen aan de client om de MQTT ontvangen berichten te verwerken.
### BLE
Vervolgens wordt in de setup() methode alles ingesteld om BLE signalen te verzenden en ontvangen. We maken nieuwe scan aan. Aan deze scan linken we via de setAdvertisedDeviceCallbacks methode een object van de klasse AdvertisedDeviceCallBack met de methode in die de resultaten van de scan continu verwerkt. We zetten de boolean wantDuplicates op true, we willen heel de tijd resultaten ontvangen van dezelfde apparaten, zodat we continu de afstand kunnen bepalen. Er worden een scantijd van 99 milliseconden ingesteld met een pauze van 1ms tussen de scans door. Hoe meer vermogen de BLE signalen hebben des te better de verwerking van de resultaten, daarom wordt het scan_type ingesteld op active via de methode setActiveScan(true) van de BLEScan klasse.
### Buffers
Omdat er veel waarden worden gemeten op korte tijd met uitschieters die niet gewenst zijn wordt een circulaire buffers gebruikt zodat het gemiddelde van de buffer kan worden genomen. De buffers worden geinitialiseerd in de setup via de methode initBuffers(size), met size als instelbare variabele. 
### Buzzer
We stellen ook de correcte pin in voor de buzzer. Dit is pin 15 voor het pcb-ontwerp.

## Loop
De loop methode die heel de tijd wordt overlopen heeft drie functies, zorgen dat de verbinding met de MQTT server actief blijft , BLE scans starten en stoppen, en als de andere twee functies niet uitgevoerd worden en er is een MQTT bericht, wordt de MQTT callback automatisch opgeroepen in de loop methode. 
### BLE scan
Terwijl een scan wordt uitgevoerd voor een duratie van 1 seconde, kan de BLE callback die in de setup gekoppeld werd aan de scan opgeroepen worden. Terwijl een scan wordt uitgevoerd kan de MQTT callback niet opgeroepen worden.
### MQTT reconnect




