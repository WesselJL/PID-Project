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

De code voor dit bal balanceer project d.m.v. PID is gestart door eerst een systeem te maken wat de bal kon balanceren met de sensor waardes die de tof sensor als output gaf zonder PID. Toen de bal succesvol gebalanceerd werd is de PID regeling geprogrammeerd en getest. De uitleg van de 2 programma's en de PID berekeningen staat hieronder.

### *Code zonder PID*

Dit onderstaande gedeelte is het toevoegen van de gebruikte libaries. Zoals de comments het al zeggen zijn het de libaries voor de I2C communicatie, time of flight sensor en de servo motor.

```ruby
#include <Wire.h> // libary voor I2C-communicatie
#include <Adafruit_VL53L0X.h> // VL53L0X sensor libary 
#include <ESP32Servo.h> // libary voor Servo motor
```
Dan als volgende is het de pin initialisatie van de servo motor, initialisatie van de VL53LOX variabele en de servo variabele.

```ruby
#define SERVO_PIN 12 // Definitie van de pin (PIN 12) voor de uitvoer van de aansturing van de Servo-motor
Adafruit_VL53L0X lox = Adafruit_VL53L0X(); // Initialisatie van de sensor variabele
Servo servo; // Initialisatie van de Servo motor
```
In de setup van het programma wordt de seriële communicatie opgesteld en wordt er gecontroleerd of de VL53LOX juist is opgestart. Wanneer dit niet het geval is dan wordt er een foutmelding in de seriële monitor geprint. Daarnaast wordt ook de servo variabele gekoppeld aan de Servo pin en wordt de positie van de servo motor ingesteld op 90 graden, dit is de start positie.

```ruby
void setup() {
  Serial.begin(115200); // Initialisatie van de seriële communicatie met een baudrate van 115200 bps
  Serial.println("Ping Pong Ball Balancing"); // Uitvoer van een start bericht

  if (!lox.begin()) { // Controle op het starten van de VL53L0X-sensor
    Serial.println(F("Failed to boot VL53L0X")); // Uitvoer van een foutmelding bij mislukken en stoppen van het programma
    while (1);
  }

  servo.attach(SERVO_PIN); // Koppeling van de Servo variabele aan de gedefinieerde pin
  servo.write(90); // Instellen van de initiële positie van de Servo-motor op 90 graden

  delay(500); // Wachten om de Servo-motor naar de initiële positie te laten bewegen
}
```
In de loop van het programma wordt de afstand tot de bal gemeten door de sensor en wordt er gecontroleerd of deze meting een geldige meting is. Daarna wordt de de minimale sensor waarde en de maximale waarde omgezet naar een servo waarde van 0 tot 180. Dit wordt gedaan met de "map" functie. Vervolgens wordt de variabele "servoPosition" naar de Servo gestuurd. In de volgende video kan je zien hoe de bal gebalanceerd wordt zonder PID, [Video: bal balanceren zonder PID](https://youtu.be/PQ2CX1DTkAU)

```ruby
void loop() {
  VL53L0X_RangingMeasurementData_t measure; // Definitie van de bal voor de VL53L0X-sensor
  
  lox.rangingTest(&measure, false); // Uitvoeren van een afstandsmeting met de VL53L0X-sensor

  if (measure.RangeStatus != 4) {  // Controleren of de meting geldig is
    int distance = measure.RangeMilliMeter; // Uitlezen van de gemeten afstand in millimeters
    Serial.print("Distance (mm): "); // Uitvoer van het label "Distance (mm): "
    Serial.println(distance); // Uitvoer van de gemeten afstand
    
    // Aanpassen van de positie van de Servo-motor op basis van de gemeten afstand
    int servoPosition = map(distance, 20, 270, 0, 180); // Aanpassen van de mapping-bereik op basis van de opstelling
    servo.write(servoPosition); // Aansturen van de Servo-motor naar de berekende positie
    
    delay(50); // Aanpassen van deze vertraging om de responsiviteit van het systeem te regelen
  } else {
    Serial.println("Out of range"); // Uitvoer van een melding wanneer de gemeten afstand buiten bereik is
  }
}
```
> [!IMPORTANT]
> De waardes 20 & 270 in de 'int servoPosition = map(distance, 20, 270, 0, 180);' line moeten worden aangepast naar de minimale en maximale sensor waarde van je eigen sensor. Deze zijn te verkrijgen door je object wat gebalanceerd moet worden op zijn positie die het dichtsbij de sensor zit en het verste weg van de sensor zit. Deze zijn uit te lezen door het uitvoeren van onderstaande code:

```ruby
#include <Wire.h> // libary voor I2C-communicatie
#include <Adafruit_VL53L0X.h> // libary voor VL53L0X sensor

Adafruit_VL53L0X lox = Adafruit_VL53L0X(); // Initialisatie van de VL53L0X variabele

void setup() {
  Serial.begin(115200); // Initialisatie van de seriële communicatie met een baudrate van 115200 bps
  Serial.println("Ping Pong Ball Balancing"); // Uitvoer van een begroetingsbericht

  if (!lox.begin()) { // Controle op het starten van de VL53L0X-sensor
    Serial.println(F("Failed to boot VL53L0X")); // Uitvoer van een foutmelding bij mislukken en stoppen van het programma
    while (1);
  }

  delay(500); // Wachten om de sensor op te starten
}

void loop() {
  VL53L0X_RangingMeasurementData_t measure; // Definitie van de bal voor de VL53L0X-sensor
  
  lox.rangingTest(&measure, false); // Uitvoeren van een afstandsmeting met de VL53L0X-sensor

  if (measure.RangeStatus != 4) {  // Controleren of de meting geldig is
    int distance = measure.RangeMilliMeter; // Uitlezen van de gemeten afstand in millimeters
    Serial.print("Distance (mm): "); // Uitvoer van het label "Distance (mm): "
    Serial.println(distance); // Uitvoer van de gemeten afstand
    
    delay(50); // Aanpassen van deze vertraging om de responsiviteit van het systeem te regelen
  } else {
    Serial.println("Out of range"); // Uitvoer van een melding wanneer de gemeten afstand buiten bereik is
  }
}
```

### *PID berekening*

Een volledige PID output kan beschreven worden door de volgende formule:

$$ u(t) = K_p e(t) + K_i \int_{0}^{t} e(\tau) d\tau + K_d \frac{de(t)}{dt} $$ 

$u(t)$ : Dit is de output van de berekening

$e(t)$ : Dit is de error die zich nog in de output bevindt ten opzichte van het setpoint

$K_p$ : Dit is de proportionele factor van het systeem

$K_i$ : Dit is de integrale factor van het systeem

$K_d$ : Dit is de afgeleide factor van het systeem

Deze formule berekent op basis van de fout tussen het gewenste punt/ waarde en de huidige waarde een output waarde die bijvoorbeeld de aansturing van een motor, de positie van een servo of de tempratuur in je huis aan geeft. $e(t)$ is de fout op een bepaald moment in de tijd en het verschil tussen de waarde waar je wilt eindigen en de werkelijke waarde op dat moment. $K_p$ is de proportionele factor van het systeem, deze waarde bepaalt hoe sterk de uitvoer reageert op de directe fout. $K_i$ is de integrale factor. Deze factor neemt de afwijkingen uit het verleden en telt de waardes op en corrigeert daarmee binnen I-tijd (in seconden) de aansturing/ output waarde. $K_d$ Dit is de afgeleide factor van het systeem en meet de snelheid waarmee de fout veranderd. Dit zorgt er voor dat overshoot wordt verminderd (het voorbij de vooraf ingestelde waarde/ setpoint gaan). Voor meer uitgebreide informatie en voorbeelden van PID systemen zie [deze link naar de uitleg](https://nl.wikipedia.org/wiki/PID-regelaar).

### *Code met PID*

In de code voor het balanceren van de bal met een PID regeling worden er een aantal variabelen in de code toegevoegd. Deze variabelen zijn de huidige tijd en de laatste tijd, de waardes voor P, I en D en de error die het systeem heeft. Daarnaast is er ook nog een variabele om de tijd in op te slaan tussen de metingen en een variabele voor de vorige foutwaarde.

```ruby
double sensorValue; // Variabele voor het opslaan van de gelezen sensorwaarde
double currentTime; // Huidige tijdvariabele voor het regelen van de meetfrequentie
double lastTime; // Variabele voor het opslaan van de laatste tijd
double P = 2; // Proportionele term voor de PID-regelaar
double I = 0.1; // Integratieve term voor de PID-regelaar
double D = 0.1; // Differentiërende term voor de PID-regelaar
double error; // Variabele voor het opslaan van de fout in de regeling
double dt; // Tijd tussen metingen voor de PID-regelaar
double setpoint = 550; // Gewenste sensorwaarde
double previous; // Vorige foutwaarde voor de differentiërende term in de PID-regelaar
```
De code in de setup blijft het zelfde. De seriële communicatie wordt nog steeds geïnitialiseerd en de servo positie wordt weer naar een "start" waarde gezet.  

> [!WARNING]
> De goeie waardes voor P, I en D om een stabiel systeem te krijgen verschillen voor elke opstelling. Door middel van trial en error zijn de waardes voor een verschillend systeem vinden (denk aan een zwaardere bal of dergelijk).

De functie 'calculatePID' berekend de juiste output om de servo aan te sturen en returnt deze. 

*Proportioneel*: De fout 'error' wordt vermenigvuldigd met de proportionele factor 'P'.

*Integraal*: De fout 'error' wordt vermenigvuldigd met de *dt* sampletijd, de vorige berekende waarde van de integraal wordt hierbij opgeteld. Vervolgens wordt dit nog vermenigvuldigd met de integrale factor 'I'.

*Differentiaal*: De fout 'error' wordt afgetrokken van de vorige fout en gedeeld door de *dt* sampletijd. Deze uitkomst wordt vermenigvuldigd met de differentiale factor 'D'.

Uiteindelijk geeft de functie de opgetelde som van deze drie waardes, dus Proportioneel + Integraal + Differentiaal.

```ruby
double calculatePID() {
  double proportioneel = error * P; // Bereken de proportionele term
  double integraal = (integraal + error * dt) * I; // Bereken de integratieve term
  double differentiaal = ((error - previous) / dt) * D; // Bereken de differentiërende term
  previous = error; // Bijwerken van de vorige foutwaarde
  return (proportioneel + integraal + differentiaal); // Bereken de totale PID-uitvoer
}
```
De loop in deze code ziet er als volgt uit: 

```ruby
void loop() {
  if (millis() - currentTime >= 50) { // Regel de meetfrequentie
    currentTime = millis();
    double now = millis();
    dt = (now - lastTime) / 1000.0; // Bereken de tijd tussen metingen
    lastTime = now;

    sensorValue = readSensor(); // Lees de sensorwaarde
    sensorValue = map(sensorValue, 20.0, 350.0, 0.0, 1000.0); // Mapping van de sensorwaarde naar een specifiek bereik
    error = setpoint - sensorValue; // Bereken de fout in de regeling
    double output = calculatePID(); // Bereken de PID-uitvoer

    output = map(output, -1000, 1000, 210, 60); // Mapping van de PID-uitvoer naar een geschikt bereik

    // Begrens de servo-uitvoer binnen bepaalde grenzen zodat de servo niet extreme waardes binnen krijgt wanneer er een PID waarde tussen zit die extreem anders is dan de gemiddelde waarde, hierdoor blijft de servo binnen de gestelde waardes van 60 en 220
    if (output < 60) {
      output = 60;
    }
    if (output > 220) {
      output = 220;
    }

    myServo.write(output); // Stuur de Servo-motor aan naar de berekende positie

    Serial.println(output); // Uitvoer van de PID-uitvoer
    Serial.println(sensorValue); // Uitvoer van de gemeten sensorwaarde
  }
}
```
De loop begint met een if statement die ervoor zorgt dat het programma onder de if elke 50ms wordt uitgevoerd. 50ms is gekozen omdat de servo niet sneller kan reageren.

Vervolgens wordt de exacte $d(t)$ berekend die tussen de cycles in zit, dit is nodig om precies de Integraal en Differentiaal te kunnen regelen. 

Daaronder wordt de Sensor uitgelezen, gemapt naar 0 tot 1000, de error berekend en de calculatePID berekening aangeroepen. 

Uiteindelijk wordt de output gemapt naar een geschikt bereik voor de servo. 210 is de meest lage stand en 60 is de meest hoge stand.

Met *myServo.write(output);* wordt de servo nog geüpdate. 

De code met PID kan je zien in de volgende [video](https://youtu.be/Alv6lQySq_s)

### *Code voor het veranderen van PID waardes in web interface* 

Om de PID waardes te veranderen in een web interface wordt er in de setup van de code een deel toegevoegd wat er voor zorgt dat de ESP32 verbind met een wifi netwerk en dat de ESP32 wordt geconfigureerd als acces point. Vervolgens geeft de code een ip address die gebruikt kan worden om naar de web interface te navigeren. De code hiervan staat hieronder uitgelegd:

```ruby
  // Verbind met het Wi-Fi-netwerk met SSID en wachtwoord
  Serial.print("Setting AP (Access Point)…");
  // Verwijder het wachtwoordparameter als je wilt dat het AP (Access Point) open is
  WiFi.softAP(ssid, password);

  IPAddress IP = WiFi.softAPIP();
  Serial.print("AP IP address: ");
  Serial.println(IP);

  server.begin();
```
In de void executeWiFi worden een heel aantal dingen gedaan en hieronder worden de belangrijkste dingen toegelicht:

om de HTML webpagina te tonen met 3 invoer velden van de PID waardes en 3 bevestigings knoppen wordt de volgende code gebruikt
```ruby
            // Toon de HTML-webpagina
            client.println("<!DOCTYPE html><html>");
            client.println("<head>");
            client.println("<meta name=\"viewport\" content=\"width=device-width, initial-scale=1\">");
            client.println("</head>");
            client.println("<body>");

            // Werk de formulieractie bij om de POST-methode te gebruiken
            client.println("<form action=\"/update\" method=\"post\">");

            // Werk de waarde van de tekstvelden bij met de huidige waarden van P, I en D
            client.println("<p>P: <input type=\"text\" name=\"P\" class=\"text-field\" pattern=\"[+-]?([0-9]*[.])?[0-9]+\" value=\"" + String(P) + "\"></p>");
            client.println("<p>I: <input type=\"text\" name=\"I\" class=\"text-field\" pattern=\"[+-]?([0-9]*[.])?[0-9]+\" value=\"" + String(I) + "\"></p>");
            client.println("<p>D: <input type=\"text\" name=\"D\" class=\"text-field\" pattern=\"[+-]?([0-9]*[.])?[0-9]+\" value=\"" + String(D) + "\"></p>");

            // Voeg een verzendknop toe voor elke variabele om de formulierinzending te activeren
            client.println("<p><input type=\"submit\" name=\"updateP\" value=\"Update Values\"></p>");
```
Hierin worden de de PID waardes bijgewerkt op basis van de input, daarnaast zorgt dit deel ook voor de layout van de webpagina. Nadat er waardes zijn ingevuld voor P, I en D worden de waardes verstuurd naar de vaiabeles die eerder zijn uitgelegd. Dit wordt gedaan met de volgende regels code:

```ruby
// Werk de PID-parameter bij op basis van de POST-gegevens
void updateParameter(String postBody, String paramName, float &param) {
  int paramIndex = postBody.indexOf(paramName + "=");
  if (paramIndex != -1) {
    String paramValue = postBody.substring(paramIndex + paramName.length() + 1);
    param = paramValue.toFloat(); // Zet de string om naar een float en werk de parameter bij
    Serial.print("Updated " + paramName + ": ");
    Serial.println(param);
  }
}
```


---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
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
