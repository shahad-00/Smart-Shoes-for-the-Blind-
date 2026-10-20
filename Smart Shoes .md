# Smart Shoes for the Blind

## Overview

An Arduino-based smart shoe prototype designed to detect nearby obstacles and provide audible alerts for blind and visually impaired users.

An ultrasonic sensor measures the distance to nearby objects, while a buzzer warns the user when an obstacle is within the detection range.

## Objectives

- Build a wearable obstacle-detection prototype.
- Measure the distance to nearby objects using an ultrasonic sensor.
- Provide audible warnings that become more frequent as obstacles get closer.
- Explore embedded systems in assistive technology.

## Hardware Components

### Arduino Uno

Processes sensor readings and controls the audible alerts.

<img width="637" height="356" alt="Screenshot 2026-10-20 183711" src="https://github.com/user-attachments/assets/93684759-7e58-41fb-9cab-96bf9f4c700d" />


### Battery

Provides power to the prototype.

<img width="638" height="365" alt="Screenshot 2026-10-20 183722" src="https://github.com/user-attachments/assets/feec94ed-ad81-433a-ba80-edd3b92052ff" />


### HC-SR04 Ultrasonic Sensor

Measures distance using ultrasonic pulses and their returning echoes.

<img width="772" height="433" alt="Screenshot 2026-10-20 183740" src="https://github.com/user-attachments/assets/8ca3f08b-f817-4669-a8fb-32c2a87541e6" />


### Buzzer

Produces audible warnings when nearby obstacles are detected.

<img width="518" height="297" alt="Screenshot 2026-10-20 183803" src="https://github.com/user-attachments/assets/283c3b8c-440c-4445-a072-62699ee71d3d" />


### Power Switch

A power switch was included but did not function in the assembled prototype. The prototype remained active while connected to power.

<img width="595" height="331" alt="Screenshot 2026-10-20 183753" src="https://github.com/user-attachments/assets/8e02173d-46b0-439f-bdb1-36e2aa915544" />


## Software and Tools

- Arduino IDE
- Arduino programming language (C/C++)
- Serial Monitor

## How It Works

1. The Arduino sends a trigger pulse to the ultrasonic sensor.
2. The sensor returns an echo signal.
3. The program converts the echo duration into a distance in centimeters.
4. If the distance is greater than 0 cm and no more than 100 cm, the alert outputs are activated.
5. The pause between alerts becomes shorter as the obstacle gets closer.
6. Distance readings are displayed in the Serial Monitor.

## Arduino Code

The following sketch reads one ultrasonic sensor and controls two alert outputs. It is the available project sketch; it has not been confirmed as the exact version used in the prototype demonstration.

### Pin Assignments

| Connection | Arduino Pin |
| --- | --- |
| Ultrasonic Trigger | A4 |
| Ultrasonic Echo | A5 |
| Alert Output 1 | 13 |
| Alert Output 2 | 9 |

```cpp
const int pingTrigPin = A4;
const int pingEchoPin = A5;

int buz = 13;
int buz1 = 9;

void setup() {
  Serial.begin(9600);

  pinMode(buz, OUTPUT);
  pinMode(buz1, OUTPUT);
}

void loop() {
  long duration, cm;

  pinMode(pingTrigPin, OUTPUT);
  digitalWrite(pingTrigPin, LOW);
  delayMicroseconds(2);
  digitalWrite(pingTrigPin, HIGH);
  delayMicroseconds(5);
  digitalWrite(pingTrigPin, LOW);

  pinMode(pingEchoPin, INPUT);
  duration = pulseIn(pingEchoPin, HIGH);

  cm = microsecondsToCentimeters(duration);

  if (cm <= 100 && cm > 0) {
    int d = map(cm, 1, 100, 20, 2000);

    digitalWrite(buz, HIGH);
    digitalWrite(buz1, HIGH);
    delay(50);

    digitalWrite(buz, LOW);
    digitalWrite(buz1, LOW);
    delay(d);
  }

  Serial.print(cm);
  Serial.print("cm");
  Serial.println();

  delay(40);
}

long microsecondsToCentimeters(long microseconds) {
  return microseconds / 29 / 2;
}
```

### Code Explanation

- The trigger pin sends a short pulse to start a distance measurement.
- pulseIn measures the duration of the returning echo.
- microsecondsToCentimeters converts the measured duration into centimeters.
- The alert threshold is 100 cm.
- map adjusts the pause between alerts according to distance.
- The Serial Monitor displays distance readings at 9600 baud.

## Prototype Demonstration


https://github.com/user-attachments/assets/636531bc-b5ef-40e0-af2a-e8acaee52924



## Conclusion

The goal of this project was to build a smart shoe prototype that combines distance sensing and audible alerts, demonstrating a practical application of Arduino in assistive technology.

## Reference

[Smart Shoes Tutorial](https://youtu.be/qIQvXeuyjlo?si=hLHQ3iM1qagm8LYr)
