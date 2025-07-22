# Smart Dustbin Monitoring System using Arduino

## Project Overview

The **Smart Dustbin Monitoring System** uses an **Arduino UNO** along with an **Ultrasonic Sensor** and a **16x2 LCD** display to monitor the fill level of a dustbin. When the dustbin reaches a certain threshold of fullness, an alert is triggered using a **Buzzer**. The system can also display the current distance to the object on an LCD screen in real-time.

This system can be used in smart cities, waste management systems, or any location where dustbin fill levels need to be monitored to optimize waste collection.

## Features

- **Real-time Monitoring**: Continuously monitors the fill level of the dustbin.
- **LCD Display**: Shows the current distance and fill status on a 16x2 LCD screen.
- **Buzzer Alert**: The system triggers a buzzer when the dustbin is full.
- **Threshold Alert**: The dustbin is considered full when the distance is below a predefined threshold (e.g., 10 cm).

## Components Required

1. **Arduino UNO** - 1
2. **Ultrasonic Sensor (HC-SR04)** - 1
3. **16x2 LCD Display** - 1
4. **Buzzer** - 1
5. **Jumper Wires**
6. **Breadboard**
7. **Power Supply (5V)**

## Wiring Connections

- **Ultrasonic Sensor**:
  - VCC → 5V (Arduino)
  - GND → GND (Arduino)
  - TRIG → Digital Pin 9 (Arduino)
  - ECHO → Digital Pin 10 (Arduino)

- **LCD 16x2**:
  - VSS → GND
  - VDD → 5V
  - VO → Potentiometer for contrast (middle pin)
  - RS → Digital Pin 12 (Arduino)
  - RW → GND
  - E → Digital Pin 11 (Arduino)
  - D4 → Digital Pin 5 (Arduino)
  - D5 → Digital Pin 4 (Arduino)
  - D6 → Digital Pin 3 (Arduino)
  - D7 → Digital Pin 2 (Arduino)

- **Buzzer**:
  - Positive (+) → Digital Pin 8 (Arduino)
  - Negative (−) → GND

## Setup Instructions

### 1. Connect the Circuit

- Connect the **HC-SR04 ultrasonic sensor** to the Arduino.
- Connect the **16x2 LCD display** to the Arduino using the appropriate pins.
- Connect the **buzzer** to the designated digital pin.

### 2. Upload the Code

- Open **Arduino IDE** and select the appropriate **board** (Arduino UNO) and **port**.
- Copy and paste the provided **Arduino code** into the IDE.
- Click **Upload** to load the code into your Arduino UNO.

### 3. Monitor the System

- After uploading, the **LCD screen** will display the current distance from the ultrasonic sensor.
- If the distance falls below a threshold (e.g., 10 cm), the **buzzer** will be triggered, and the display will show "Dustbin Full".
- When the dustbin is not full, the display will show "Dustbin Empty" and the buzzer will be off.

## Code

```cpp
#include <LiquidCrystal.h>  // Include the LCD library

// Define pins for the ultrasonic sensor
const int trigPin = 9;   // Trigger pin of the ultrasonic sensor
const int echoPin = 10;  // Echo pin of the ultrasonic sensor

// Define pins for the LCD
LiquidCrystal lcd(12, 11, 5, 4, 3, 2);

// Define pin for the buzzer
const int buzzerPin = 8;

// Define the threshold distance (in cm) for full dustbin
const int fullThreshold = 10;  // Less than 10 cm indicates the dustbin is full

// Variables to store ultrasonic sensor readings
long duration;
int distance;

void setup() {
  // Start the serial monitor
  Serial.begin(9600);
  
  // Set up the LCD
  lcd.begin(16, 2);
  lcd.print("Smart Dustbin");

  // Set up the ultrasonic sensor pins
  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);
  
  // Set up the buzzer
  pinMode(buzzerPin, OUTPUT);
  
  // Wait for the system to be ready
  delay(2000);
}

void loop() {
  // Send a pulse to the ultrasonic sensor
  digitalWrite(trigPin, LOW);  
  delayMicroseconds(2);
  digitalWrite(trigPin, HIGH); 
  delayMicroseconds(10);  
  digitalWrite(trigPin, LOW);
  
  // Read the pulse duration from the echo pin
  duration = pulseIn(echoPin, HIGH);
  
  // Calculate the distance based on the time taken by the pulse to travel
  distance = duration * 0.0344 / 2; // Speed of sound = 0.0344 cm/us
  
  // Display the distance on the LCD screen
  lcd.clear();
  lcd.setCursor(0, 0);
  lcd.print("Distance: ");
  lcd.print(distance);
  lcd.print(" cm");

  // Check if the dustbin is full (distance less than the threshold)
  if (distance < fullThreshold) {
    // Turn on the buzzer
    digitalWrite(buzzerPin, HIGH);
    lcd.setCursor(0, 1);
    lcd.print("Dustbin Full!");
  } else {
    // Turn off the buzzer
    digitalWrite(buzzerPin, LOW);
    lcd.setCursor(0, 1);
    lcd.print("Dustbin Empty");
  }
  
  // Wait for a while before the next reading
  delay(1000);
}
