# Temp-And-Humidity-Sensor-with-IoT-app-control-
My first IoT project, where I used multiple sensors to create an integrated climate control system. (that is controlled via an app made with the help of  BlynkIoT)
//c / c++ code
// uses arduino ide
/*
---------------------------------------------------------
 Project: ESP32 Smart Climate Controller
 Author: Aryan Sharma
 Platform: ESP32
 Sensors: DHT11 / DHT22
 Actuators: Relay Module (Fan), LED Indicator
 Description:
   - Reads temperature & humidity
   - Turns ON fan (via relay) and LED when temperature
     exceeds a user-defined threshold
   - Turns OFF both when temperature falls below threshold
---------------------------------------------------------
 Open Source | Educational | IoT Ready
---------------------------------------------------------
*/

#include <DHT.h>

/* ----------- PIN DEFINITIONS ----------- */
#define DHTPIN 4          // DHT data pin
#define DHTTYPE DHT11     // Change to DHT22 if needed

#define RELAY_PIN 26      // Relay control pin
#define LED_PIN 27        // LED indicator pin

/* ----------- OBJECT CREATION ----------- */
DHT dht(DHTPIN, DHTTYPE);

/* ----------- USER SETTINGS ----------- */
float thresholdTemp = 23.0;   // Temperature threshold (°C)

/* ----------- RELAY LOGIC ----------- */
// Most relay modules are ACTIVE LOW
#define RELAY_ON  LOW
#define RELAY_OFF HIGH

void setup() {
  Serial.begin(115200);

  pinMode(RELAY_PIN, OUTPUT);
  pinMode(LED_PIN, OUTPUT);

  // Ensure everything starts OFF
  digitalWrite(RELAY_PIN, RELAY_OFF);
  digitalWrite(LED_PIN, LOW);

  dht.begin();

  Serial.println("ESP32 Climate Controller Started");
}

void loop() {
  delay(2000);  // Sensor stability delay

  float temperature = dht.readTemperature();
  float humidity = dht.readHumidity();

  // Check for sensor errors
  if (isnan(temperature) || isnan(humidity)) {
    Serial.println("Failed to read from DHT sensor!");
    return;
  }

  Serial.print("Temp: ");
  Serial.print(temperature);
  Serial.print(" °C | Humidity: ");
  Serial.print(humidity);      
  Serial.println(" %");

  /* ----------- CONTROL LOGIC ----------- */
  if (temperature >= thresholdTemp) {
    // Turn ON fan + LED
    digitalWrite(RELAY_PIN, RELAY_ON);
    digitalWrite(LED_PIN, HIGH);

    Serial.println("STATUS: FAN & LED ON");
  } else {
    // Turn OFF fan + LED
    digitalWrite(RELAY_PIN, RELAY_OFF);
    digitalWrite(LED_PIN, LOW);

    Serial.println("STATUS: FAN & LED OFF");
  }
}
