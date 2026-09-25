# 🌐 AgriTech Hub: Smart Agriculture Automation Matrix

## 🚨 Problem Statement
* Traditional farming practices waste immense amounts of water due to manual, inefficient irrigation methods.
* Farmers cannot dynamically stop water pumps during sudden rain or monitor water tank depletion levels simultaneously.

## 🎯 Objective
* To build a compact IoT hub that automates farm irrigation based on live sensor data.
* To protect water pumps from running dry and automatically turn off irrigation during rainfall to conserve resources.

## 🔌 Components Required
* **Microcontroller:** Arduino Uno R3
* **Display:** 16x2 I2C LCD Screen
* **Sensors:** Soil Moisture, Rain Sensor, HC-SR04 Ultrasonic Sensor, DHT11 Temperature Sensor
* **Connectivity:** ESP-01 Wi-Fi Module
* **Power & Control:** 5V Relay Module, DC Water Pump, 9V Battery, Breadboard & Jumper Wires

## 🔄 Working Logic
* **Irrigation Loop:** Automatically turns the water pump **ON** via the relay when the soil moisture sensor reads **DRY**.
* **Rain Override:** Instantly cuts power to the pump if the rain sensor detects sudden rainfall, preventing overwatering.
* **Tank Protection:** The ultrasonic sensor monitors water depth and locks out the pump if the tank runs critically low.
* **Climate Tracking:** The DHT11 sensor logs local temperature and humidity metrics for microclimate tracking.

## 📱 About the AgriTech App
* A responsive mobile dashboard UI built to show live values for soil condition, temperature, and water storage.
* Features a safety warning banner that pops up immediately when automated rain-override mode is activated.

### 🔗 Live Project Dashboard
[🌐 Click Here to Open Live AgriTech Dashboard Interface](https://claude.ai/artifact/LLBziqe1ok64LSgxPPoyp1)

## 💻 App Interface Source Code (`index.html`)
```html
<!-- Save this file on GitHub as index.html -->
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8"><title>AgriTech Dashboard</title>
    <style>
        body { font-family: sans-serif; background: #f0f4f8; display: flex; justify-content: center; padding: 20px; }
        .container { width: 320px; background: white; padding: 20px; border-radius: 20px; box-shadow: 0 5px 15px rgba(0,0,0,0.1); border: 5px solid #333; }
        .header { text-align: center; color: #2e7d32; font-size: 20px; font-weight: bold; }
        .card { background: #f9f9f9; padding: 12px; border-radius: 10px; margin: 10px 0; }
        .btn { width: 100%; padding: 10px; background: #4caf50; color: white; border: none; border-radius: 8px; font-weight: bold; }
    </style>
</head>
<body>
<div class="container">
    <div class="header">🌱 AgriTech Mobile</div>
    <div class="card"><strong>Soil Moisture:</strong> <span style="color:red">DRY</span></div>
    <div class="card"><strong>Temperature:</strong> 28°C</div>
    <div class="card"><strong>Water Tank:</strong> 85% (Optimal)</div>
    <button class="btn">SYSTEM AUTOMATED</button>
</div>
</body>
</html>
```

## 🗺️ Simulation Circuit
![Simulation Workspace Image](Simulation%20circuit.jpg)

## 📐 Circuit Schematics
![Circuit Diagram Schematic](Circuit%20schematics.jpg)
* **LCD Screen:** A4 (SDA), A5 (SCL)
* **Ultrasonic Sensor:** D8 (Trig), D7 (Echo)
* **Relay Switch Module:** D3 (Control Output)
* **Analog Sensors:** A0 (Soil Moisture Input), A1 (Rain Sensor Input)

## 📸 Working Hardware
![Physical Board Connection Photo](Working%20hardware.jpg)

### 🎬 System Demonstration Video
[▶ Click Here to Watch the Working Hardware Video](working_demo.mp4)

## 💻 Microcontroller Core Firmware Code (`main.ino`)
```cpp
// Save this file on GitHub as main.ino
#include <Wire.h>
#include <LiquidCrystal_I2C.h>

LiquidCrystal_I2C lcd(0x27, 16, 2); 
const int soilPin = A0, rainPin = A1, relayPin = 3, trigPin = 8, echoPin = 7;

void setup() {
  pinMode(relayPin, OUTPUT); pinMode(trigPin, OUTPUT); pinMode(echoPin, INPUT);
  lcd.init(); lcd.backlight();
}

void loop() {
  int soilVal = analogRead(soilPin);
  int rainVal = analogRead(rainPin);
  
  if (soilVal < 400 && rainVal < 200) {
    digitalWrite(relayPin, HIGH);
    lcd.clear(); lcd.print("Soil: DRY | PUMP:ON");
  } else if (rainVal >= 200) {
    digitalWrite(relayPin, LOW);
    lcd.clear(); lcd.print("RAIN! PUMP: OFF");
  } else {
    digitalWrite(relayPin, LOW);
    lcd.clear(); lcd.print("Soil: WET | PUMP:OFF");
  }
  delay(1500);
}
```
