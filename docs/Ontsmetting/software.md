---
layout: default
title: Software
parent: Ontsmetting
grand_parent: Onderdelen
nav_order: 1
---

# Software

Het gehele project wordt bestuurd via een ESP32 module. Deze module kan geprogrammeerd worden via Arduino flavoured C en C++. Alle geschreven software staat opgeslaan op de [algemene reopository](https://github.com/Ontsmettinator3000/main) van onze [organisatie](https://github.com/Ontsmettinator3000). Bij de start van de ontwikkeling kozen we er voor om het geheel "modulair" maken. Hierbij splitsten we de verschillende onderdelen op in aparte klasses. Dit deden we om de main loop leesbaar en overzichtelijk te maken. Ook is het hierdoor makkelijk om dingen uit te breiden of te overbruggen bij foutieve werking.

# Setup en main loop

Zoals elk Arduino programma, zal ook onze code lopen vanuit een loop. Hierbij zal eenmaal de setupmethode overlopen worden alvorens de loop te starten. In de setup zullen we de verschillende onderdelen juist configureren. Zo zullen we de wifi en MQTT verbinding instellen, alsook enkele pinModes definiëren. Ook de OTA connectie wordt hier ingesteld.

De main loop is een vertaling van de [algemene flowchart](index.md#Blokschema) in code. Eerst en vooral zullen we loop functies hebben voor verschillende klasses. Deze zullen zeer frequent opgeroepen worden. Vaak zullen deze functie bepaalde informatie ophalen of dingen updaten. Hierna zullen we eerste test doen. Merk op dat in deze fase alle andere onderdelen uitgeschakeld zijn door de setup. Indien een alarm ontvangen is, zullen we het scherm updaten en de NFC handler inschakelen. We bevinden ons nu in fase twee van het proces. Hierbij zal een gescande kaart de login functie oproepen. Deze zal testen of de tag al dan niet correct is en eventueel reeds geregistreerd. Als dit onderdeel doorlopen is, rest er nog enkel de alcohol te laten lopen. Dit zal gebeuren nadat de scanner een hand geregistreerd heeft via de scanner klasse. Indien deze cyclus doorlopen is voor iedere besmette speler, zal een OK signaal verzonden worden naar de broker.

Doorheen de code mag er geen gebruik gemaakt worden van blocking code, deze zal namelijk zorgen voor een vertraagde/foute OTA. Het gebruik van millis() bij tijdsmetingen is dus vereist. Ook zal de code niet blokkeren bij een fout in de setup. Er zal dus steeds vanuit de setup naar de loop gegaan worden, onafhankelijk of deze voorafgaande setup succesvol was. Hierdoor is programmeren via WiFi op elk moment mogelijk.

# Config

Tijdens het hele ontwikkelingsproces moeten er veel parameters bepaald worden. We kozen ervoor om alle instellingen te centraliseren in één document. Hierdoor konden we in één oogopslag alle instellingen bekijken en snel aanpassen. Dit zorgt er ook voor dat we verschillende parameters in meerdere klasses tegelijk kunnen gebruiken zonder deze meerdere keren te moeten definiëren.

# IR sensor

# Scherm
# Login
# Monitor
# MQTT
# NFC
# Speaker