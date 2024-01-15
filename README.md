# PID-Project
PID ball balancer project 

Het PID bal balanceer project is een project waarbij een balletje wordt gebalanceerd in een goot door middel van een proportioneel-integraal-differentiaal regelaar. De hardware van het project bevat een ESP32 WROOM, VL530IX time of flight sensor en een servo motor. Deze elektro componenten zijn geinstalleerd op een 3D geprinte opstelling die ontworpen is (zie afbeelding van de opstelling hieronder).

![WhatsApp Image 2024-01-15 at 14 33 00](https://github.com/WesselJL/PID-Project/assets/80854689/4aa8ec0a-fa02-4ce8-8ac0-2f3b26bc0288)

## **Uitleg ontwerpkeuzes 3D print**

## **Uitleg Componentkeuze**

### *MG90 Servo*

### *VL530IX tof sensor*

### *ESP32 WROOM*

## **Uitleg Code**

### *Code zonder PID*

### *PID berekening*

### *Code met PID*

-----------------------               --------------------------                 -----------------------------                  ----------------------                    -----------------
Format voorbeelden hieronder

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
