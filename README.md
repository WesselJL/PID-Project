# PID-Project
PID ball balancer project 

Het PID bal balanceer project is een project waarbij een balletje wordt gebalanceerd in een goot door middel van een proportioneel-integraal-differentiaal regelaar. De hardware van het project bevat een ESP32 WROOM, VL530IX time of flight sensor en een servo motor. Deze elektro componenten zijn geinstalleerd op een 3D geprinte opstelling die ontworpen is (zie afbeelding van de opstelling hieronder).

![WhatsApp Image 2024-01-15 at 14 33 00](https://github.com/WesselJL/PID-Project/assets/80854689/4aa8ec0a-fa02-4ce8-8ac0-2f3b26bc0288 = 250x250)

## **Uitleg ontwerpkeuzes 3D print**

Het ontwerp van de 3D modellen is zo simpel mogelijk gemaakt. Het gehele model is opgedeeld in 4 delen zodat het op elke gemiddelde 3D printer geprint kan worden en zodat wanneer een onderdeel vervangen of opnieuw ontworpen moet worden het per deel gedaan kan worden. Het ontwerp bestaat uit een goot/ balk waar een balletje in kan rollen. Deze goot/ balk kan in elkaar worden geschoven en wordt bij elkaar gehouden door het middelste kantelpunt waar een bout door heen zit. Naast het middelste kantelpunt is er nog een ander kantelpunt, dit kantelpunt is het punt waar de servo motor de goot/ balk mee laat bewegen.

Op de 3D geprinte delen zijn bevestigings punten voor zowel de servo motor als de time of flight sensor. Deze motor en sensor zijn vastgemaakt met standaar maat boutjes en moeren. Het vierde en laaste deel van het model is de voetsteun. Deze steun zit vast aan het middelste kantelpunt, het kantelpunt van de servo motor en de servo motor zelf zit ook vast aan deze voetsteun. 

## **Uitleg Componentkeuze**

Voor dit PID bal balanceer project is er gekozen voor de MG90-servo, de VL53L0X ToF-sensor en de ESP32 WROOM. Deze keuzes zijn gebaseerd op de eigenschappen en functionaliteiten van elk component, deze eigenschappen en functionaliteit zorgen ervoor dat de bal gebalanceerd kan worden door middel van de gecodeerde PID regelaar.

### *MG90 Servo*

![MG90S-180-1](https://github.com/WesselJL/PID-Project/assets/80854689/1b33b8b9-5ac1-4ebd-b266-8300f48a2a51)

De MG90-servo is gekozen vanwege de volgende eigenschappen: nauwkeurig, betrouwbaar en licht van gewicht. Met een hoog koppel voor de afmeting van de servo motor en de nauwkeurige positioneringsmogelijkheden zorgt de MG90 voor het balanceren van de bal. Bovendien is het compacte formaat van de servo gunstig voor het toepassen in ons project zonder dat er extra gewicht wordt toe gevoegd.

### *VL530IX tof sensor*

### *ESP32 WROOM*

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
