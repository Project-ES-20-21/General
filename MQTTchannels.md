# Gebruikte channels op de broker
enkele channels zijn voorbehouden voor gebruik voor de algemene aansturing: <br />
esp32/+/control <br />
"+" wordt hier vervangen door de naam van de proef in kwestie <br />
(fitness, morse, 5g, vaccin, afstand, ontsmetten, alohomora) <br />
Alle andere channels mogen (voorlopig) gebruikt worden <br />
Om het overzicht te bewaren is het wel handig als de channels van de vorm "esp32/proef/vrij te kiezen" zijn.<br />

# Controle signalen
Werkwijze momenteel:
1 controle topic waar elke ESP op moet subscriben: esp32/-proef-/control (-proef- met naam vervangen)
op deze topic zijn momenteel 5 codes mogelijk waar elke ESP dan een iplementatie voor moet voorzien:
- '0' RESET
- '1' STOP (afstand)
- '2' START (ontsmetten)
- '3' POWEROFF (stop voor fitness)
- '4' POWERON (start voor fitness)

Op deze manier kan fitnesstracker alles stoppen en starten, vanop de broker kan alles gereset worden indien nodig, en voor afstand houden en ontsmetten is er dan nog een speciale situatie voorzien die ook fitness onderbreekt. Verdere uitbreiding hier is zeker mogelijk.

