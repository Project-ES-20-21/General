---
layout: default
title: Communicatie
parent: Afstand
grand_parent: Onderdelen
nav_order: 3
---
# Communicatie
Alle communicatie verloopt via de broker met mqtt. Er is zowel interne communicatie voor synchronisatie tussen de modules als externe communicatie voor de nodige
controlesignalen te versturen en ontvangen van andere elementen in de escape room.

#### interne communicatie
Omdat er geen volledige zekerheid is dat beide speler een overtreding van de afstand meten is er een intern kanaal voor onderlinge communicatie tussen de nodes.
Bijvoorbeeld als speler 1 een overtreding meet t.o.v. speler 2 maar omgekeerd niet dan zou alleen speler 1 beginnen met piepen en speler 2 niet.
Doordat speler 2 niet weet dat hij/zij een overtreding heeft begaan, weet deze speler ook niet dat hij/zij zijn handen moet ontsmetten en zal de escaperoom vast lopen.
Via het kanaal "esp32/afstand/piep/2" (in dit geval is het speler 2) stuurt speler 1 een "1" die speler 2 dan kan verwerken.
Zo wordt ervoor gezorgd dat speler 2 ook zeker begint met piepen. 

#### externe communicatie
1. Controle <br>
  De module subscribet op het kanaal "esp32/afstand/control". Op volgende karakters wordt gereageerd:
    * '0': de module zal resetten
    * '1': pauzeer de werking totdat '2' wordt aangekregen
    * '2': hervat de werking van de schakeling
2. Ontsmetten <br>
   Langs het kanaal "esp32/ontsmetten/id" laat de module Afstand_0 weten welke spelers te dicht bij elkaar staan aan de ontsmettingspomp.
   Dit gebeurt aan de hand van id's gaande van 1 voor de centrale    module tot vier. Het overtredende koppel wordt dan doorgestuurd al een combinatie van de twee id's.
   Bijvoorbeel wanneer Afstand_0 te dicht komt bij Afstand_1, wordt "12" doorgstuurd.
