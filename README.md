# Automatic-Hand-Sanitizer-Dispenser-using-Arduino

This project demonstrates the development of a contactless automatic hand sanitizer dispenser using an ultrasonic sensor and a microcontroller (such as Arduino) to detect the presence of a user's hands and dispense sanitizer. This solution helps prevent the spread of germs by eliminating the need for physical touch, making it suitable for homes, offices, and public places.

## Table of Contents

- [Features](#features)
- [Components](#components)
- [Circuit Diagram](#circuit-diagram)
- [Code](#code)
- [Installation](#installation)
- [Usage](#usage)

## Features

- **Touchless Operation:** Uses ultrasonic sensors to detect hand presence.
- **Adjustable Range:** Customize the detection distance based on requirements.
- **Energy Efficient:** Dispenses sanitizer only when needed, saving energy.
- **Easy to Use and Portable:** Compact design and low-cost components make it practical for various locations.

## Components

- **Microcontroller**: Arduino Uno or similar
- **Ultrasonic Sensor**: HC-SR04
- **Power Supply**: 5V power supply for Arduino and pump (adjust according to your setup)
- **Switch**
- **Jumper Wires**


## Circuit Diagram

- Connect the ultrasonic sensor (HC-SR04) to the Arduino.
  - VCC to Arduino 5V
  - GND to Arduino GND
  - Trig to Arduino digital pin (e.g., pin 10)
  - Echo to Arduino digital pin (e.g., pin 9)
    
- Connect the battery to the arduino
  - positive terminal to Vin
  - negative terminal to GND
    
- Connect Servo motor to the arduino
  - Connect the power(orange colour) wire to Arduino 5V
  - Connect the yellow wire to pin no. 5
  - Connect the brown/black colour wire to GND
    
## Code

```cpp
#include <Servo.h>

Servo Myservo;
#define trigPin 10           // Trig Pin of HC-SR04
#define echoPin 9            // Echo Pin of HC-SR04
#define trigout 8            // Left motor 1st pin

int pos = 60;                // Starting position of the servo
long duration, distance;
bool rotationDone = false;   // Flag to track rotation status

void setup() {
  Myservo.attach(5);          // Attach servo to pin 5
  Serial.begin(9600);
  pinMode(trigPin, OUTPUT);   // Set Trig Pin as O/P to transmit waves
  pinMode(echoPin, INPUT);    // Set Echo Pin as I/P to receive reflected waves
  pinMode(trigout, OUTPUT);
}

void loop() {
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);
  digitalWrite(trigPin, HIGH); // Transmit waves for 10us
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);

  duration = pulseIn(echoPin, HIGH);  // Receive reflected waves
  distance = duration / 58.2;         // Calculate distance in cm

  if (distance > 0 && distance < 10 && !rotationDone) { 
    for (pos = 60; pos <= 150; pos += 2) {
      Myservo.write(pos);
      delay(15);
    }
    for (pos = 150; pos >= 60; pos -= 2) {
      Myservo.write(pos);
      delay(15);
    }
    rotationDone = true; 
  } 
  else if (distance >= 10) {
    Myservo.write(60);  // Reset position to 60 degrees if no object detected
    rotationDone = false; // Reset the flag when the object moves away
  }

  Serial.println(distance);   // Print distance for debugging
  delay(500);                // Add delay to avoid too frequent readings
}

```

## Installation

1. **Build the Circuit**: Connect all components based on the circuit diagram.
2. **Upload Code**: Install the Arduino IDE, connect the Arduino board to your computer, and upload the code.
3. **Power Up**: Provide power to the circuit and ensure the ultrasonic sensor and pump motor are correctly set up.

## Usage

1. Place your hand near the dispenser.
2. The ultrasonic sensor detects your hand and activates the servo motor.
3. Sanitizer is dispensed automatically without contact.

### Adjusting Detection Distance
You can change `THRESHOLD_DISTANCE` in the code to set how close a hand should be before the sanitizer is dispensed.

