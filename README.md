# Automatic-Hand-Sanitizer-Dispenser-using-Arduino

# Automatic Hand Sanitizer Dispenser

This project demonstrates the development of a contactless automatic hand sanitizer dispenser using an ultrasonic sensor and a microcontroller (such as Arduino) to detect the presence of a user's hands and dispense sanitizer. This solution helps prevent the spread of germs by eliminating the need for physical touch, making it suitable for homes, offices, and public places.

## Table of Contents

- [Features](#features)
- [Components](#components)
- [Circuit Diagram](#circuit-diagram)
- [Code](#code)
- [Installation](#installation)
- [Usage](#usage)
- [License](#license)

## Features

- **Touchless Operation:** Uses ultrasonic sensors to detect hand presence.
- **Adjustable Range:** Customize the detection distance based on requirements.
- **Energy Efficient:** Dispenses sanitizer only when needed, saving energy.
- **Easy to Use and Portable:** Compact design and low-cost components make it practical for various locations.

## Components

- **Microcontroller**: Arduino Uno or similar
- **Ultrasonic Sensor**: HC-SR04
- **Relay Module**: Controls the pump motor
- **Mini Submersible Pump**: Dispenses liquid sanitizer
- **Power Supply**: 5V power supply for Arduino and pump (adjust according to your setup)
- **Breadboard & Jumper Wires**

## Circuit Diagram

![Circuit Diagram](circuit-diagram-link-here)

- Connect the ultrasonic sensor (HC-SR04) to the Arduino.
  - VCC to Arduino 5V
  - GND to Arduino GND
  - Trig to Arduino digital pin (e.g., pin 9)
  - Echo to Arduino digital pin (e.g., pin 10)
- Connect the relay module to the Arduino and the pump motor.
  - Relay control pin to Arduino digital pin (e.g., pin 8)
  - Relay power pins to external power supply
  - Pump motor connects to relay output

## Code

Here is an example of Arduino code to operate the automatic dispenser:

```cpp
#define TRIG_PIN 9
#define ECHO_PIN 10
#define RELAY_PIN 8
#define THRESHOLD_DISTANCE 10  // distance in cm

void setup() {
  pinMode(TRIG_PIN, OUTPUT);
  pinMode(ECHO_PIN, INPUT);
  pinMode(RELAY_PIN, OUTPUT);
  digitalWrite(RELAY_PIN, HIGH);
  Serial.begin(9600);
}

void loop() {
  long duration, distance;
  
  digitalWrite(TRIG_PIN, LOW);
  delayMicroseconds(2);
  digitalWrite(TRIG_PIN, HIGH);
  delayMicroseconds(10);
  digitalWrite(TRIG_PIN, LOW);
  
  duration = pulseIn(ECHO_PIN, HIGH);
  distance = duration * 0.034 / 2;

  if (distance < THRESHOLD_DISTANCE && distance > 0) {
    digitalWrite(RELAY_PIN, LOW);  // Turn on pump
    delay(1000);                   // Dispense sanitizer for 1 second
    digitalWrite(RELAY_PIN, HIGH); // Turn off pump
    delay(2000);                   // Wait before rechecking
  }
}
```

## Installation

1. **Build the Circuit**: Connect all components based on the circuit diagram.
2. **Upload Code**: Install the Arduino IDE, connect the Arduino board to your computer, and upload the code.
3. **Power Up**: Provide power to the circuit and ensure the ultrasonic sensor and pump motor are correctly set up.

## Usage

1. Place your hand near the dispenser.
2. The ultrasonic sensor detects your hand and activates the pump motor.
3. Sanitizer is dispensed automatically without contact.

### Adjusting Detection Distance
You can change `THRESHOLD_DISTANCE` in the code to set how close a hand should be before the sanitizer is dispensed.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
