# PID-Project
PID ball balancer project 

Het PID bal balanceer project is een project waarbij een balletje wordt gebalanceerd in een goot door middel van een proportioneel-integraal-differentiaal regelaar. De hardware van het project bevat een ESP32 WROOM, VL530IX time of flight sensor en een servo motor. Deze elektro componenten zijn geinstalleerd op een 3D geprinte opstelling die ontworpen is (zie afbeelding van de opstelling hieronder).

![WhatsApp Image 2024-01-15 at 14 33 00](https://github.com/WesselJL/PID-Project/assets/80854689/4aa8ec0a-fa02-4ce8-8ac0-2f3b26bc0288)

## **Uitleg ontwerpkeuzes 3D print**

Het ontwerp van de 3D modellen is zo simpel mogelijk gemaakt. Het gehele model is opgedeeld in 4 delen zodat het op elke gemiddelde 3D printer geprint kan worden en zodat wanneer een onderdeel vervangen of opnieuw ontworpen moet worden het per deel gedaan kan worden. Het ontwerp bestaat uit een goot/ balk waar een balletje in kan rollen. Deze goot/ balk kan in elkaar worden geschoven en wordt bij elkaar gehouden door het middelste kantelpunt waar een bout door heen zit. Naast het middelste kantelpunt is er nog een ander kantelpunt, dit kantelpunt is het punt waar de servo motor de goot/ balk mee laat bewegen.

Op de 3D geprinte delen zijn bevestigings punten voor zowel de servo motor als de time of flight sensor. Deze motor en sensor zijn vastgemaakt met standaard maat boutjes en moeren. Het vierde en laaste deel van het model is de voetsteun. Deze steun zit vast aan het middelste kantelpunt, het kantelpunt van de servo motor en de servo motor zelf zitten ook vast aan deze voetsteun. 

> [!TIP]
> Zorg er voor dat de bouten van de kantelpunten niet te groot zijn, dit maakt het voor de servo motor moeilijk om te bewegen/ de bal te kantelen.

## **Uitleg Componentkeuze**

Voor dit PID bal balanceer project is er gekozen voor de MG90-servo, de VL53L0X ToF-sensor en de ESP32 WROOM. Deze keuzes zijn gebaseerd op de eigenschappen en functionaliteiten van elk component, deze eigenschappen en functionaliteit zorgen ervoor dat de bal gebalanceerd kan worden door middel van de gecodeerde PID regelaar.

### *MG90 Servo*

![MG90S-180-1](https://github.com/WesselJL/PID-Project/assets/80854689/1b33b8b9-5ac1-4ebd-b266-8300f48a2a51)

De MG90-servo is gekozen vanwege de volgende eigenschappen: nauwkeurig, betrouwbaar en licht van gewicht. Met een hoog koppel voor de afmeting van de servo motor en de nauwkeurige positioneringsmogelijkheden zorgt de MG90 voor het balanceren van de bal. Bovendien is het compacte formaat van de servo gunstig voor het toepassen in ons project zonder dat er extra gewicht wordt toe gevoegd.

### *VL530IX tof sensor*

![550x332](https://github.com/WesselJL/PID-Project/assets/80854689/281ce940-92aa-434b-9571-d8b94be0e529)

De VL53L0X Time-of-Flight-sensor is een belangrijk component in dit project vanwege de mogelijkheid om snel en nauwkeurig afstanden te meten. Het ToF-principe zorgt er voor dat in real-time de afastands gegevens worden verkregen over de afstand tussen de sensor en de bal die gebalanceerd moet worden. Deze gegevens zijn essentieel voor de PID-regeling om snel en effectief te reageren op veranderingen in de positie van de bal. Deze sensor kan afstanden meten tot 2 meter dus dat is ruim voldoende voor deze opstelling. De snelste modus van de sensor kan met een afstand van 1.2 meter en een nauwkeurigheid van +/- 5% elke 20ms een afstand meten. 

### *ESP32 WROOM*

![61o2ZUzB4XL _AC_UF894,1000_QL80_](https://github.com/WesselJL/PID-Project/assets/80854689/310c40ca-6b5b-4fcb-a35d-c50960a8601e)

De keuze voor de ESP32 WROOM als microcontroller is gemaakt omdat die een krachtige verwerkingscapaciteit, geavanceerde communicatiemogelijkheden en brede ondersteuning in de open source wereld heeft. De dual-core processor op 240 MHz biedt voldoende rekenkracht om de PID-regeling te implementeren en tegelijkertijd andere taken uit te voeren. Bovendien maakt de geïntegreerde Wi-Fi-connectiviteit van de ESP32 WROOM draadloze communicatie mogelijk, waardoor het gemakkelijk is om gegevens te verzenden en ontvangen voor verdere analyse en aanpassingen.

> [!NOTE]
> De componenten die hierboven genoemd en toegelicht zijn zitten aangesloten op een gaatjes print. De kabels dienen lang genoeg te zijn, dit omdat de sensor met de balk/ goot mee kantelt wanneer de bal gebalanceerd wordt.

## **Uitleg Code**

### *Code zonder PID*

### *PID berekening*

### *Code met PID*

----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
! FORMAT VOORBEELDEN HIERONDER !

> [!NOTE]
> Useful information that users should know, even when skimming content.

> [!TIP]
> Helpful advice for doing things better or more easily.

> [!IMPORTANT]
> Key information users need to know to achieve their goal.

> [!WARNING]
> Urgent info that needs immediate user attention to avoid problems.

> [!CAUTION]
> Advises about risks or negative outcomes of certain actions.

This site was built using [GitHub Pages](https://pages.github.com/)

$$ u(t) = K_p e(t) + K_i \int_{0}^{t} e(\tau) d\tau + K_d \frac{de(t)}{dt} $$ 

$u(t)$ : Dit is de output van de berekening

$e(t)$ : Dit is de error die zich nog in de output bevindt ten opzichte van het setpoint

$K_p$ : Dit is de proportionele factor van het systeem

$K_i$ : Dit is de integrale factor van het systeem

$K_d$ : Dit is de afgeleide factor van het systeem

| First Header  | Second Header |
| ------------- | ------------- |
| Content Cell  | Content Cell  |
| Content Cell  | Content Cell  |
