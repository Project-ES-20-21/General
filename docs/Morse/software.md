## Software
### Speaker
### Micro
Bij de micro maken we gebruik van de waarde 4095. Dit is de maximum waarde die de microfoon kan doorsturen wanneer het een luid signaal detecteert. Het komt overeen met 3.3V en het is een 12bit-signaal. Er wordt gekeken naar de som van de vorige 100 samples, wanneer deze een bepaalde grens overschrijdt, zal de code dit zien als een kort/lang signaal. De detectie van een kort of een lang signaal wordt toegevoegd aan een andere array. 
Omdat we de som nemen, zal er bij een lang signaal eerst een kort signaal gedetecteerd worden. Ook kan het zijn dat een kort signaal van de speler iets langer duurt dan het opgegeven kort signaal, zonder controlevoorwaarde, worden er dan meerdere korte signalen gedetecteerd. Om dit te vermijden voerden we een controlevoorwaarde in. Ook de stiltes moeten dan gedetecteerd worden om 2 korte signalen na elkaar mogelijk te maken.

<p align="center">
  <img src=https://user-images.githubusercontent.com/78847177/116791979-47cc1f80-aabe-11eb-94c7-dcd034fb1ed0.png>
</p>
