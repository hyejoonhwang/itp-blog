This week, I dedicated most of my time to thoroughly reviewing all the readings and videos to fully understand the material.
![[WhatsApp Image 2025-10-05 at 17.05.57_95975925.jpg]]
![[WhatsApp Image 2025-10-05 at 17.05.57_3bfb697b.jpg]]
![[WhatsApp Image 2025-10-05 at 17.05.58_dbb42073.jpg]]
![[WhatsApp Image 2025-10-05 at 17.05.58_db3a8936.jpg]]
![[WhatsApp Image 2025-10-05 at 17.05.59_5661d435.jpg]]
![[WhatsApp Image 2025-10-05 at 17.06.00_120c39f4.jpg]]
![[WhatsApp Image 2025-10-05 at 23.44.26_f890eb62.jpg]]
![[WhatsApp Image 2025-10-05 at 23.44.26_a75cd6a5.jpg]]
![[WhatsApp Image 2025-10-05 at 23.44.28_10315a46.jpg]]
![[WhatsApp Image 2025-10-05 at 23.44.28_0c09737d.jpg]]
![[WhatsApp Image 2025-10-05 at 23.44.28_1f7c448d.jpg]]
![[WhatsApp Image 2025-10-05 at 23.44.29_e95af84a.jpg]]




## Questions from the Labs this week:
### First Lab: Using a Transistor to Control High Current Loads with an Arduino
![[Pasted image 20251005233424.png]]![[Pasted image 20251005233431.png]]
In the schematics above the diode is connecting between the collector and the emitter. but the breadboard image shows that it's connecting the collector to the ground. Why is this?

oh wait i think I know. in the schematics shown above the diode is actually connecting from the collector to the ground. but isn't the schematics confusing..? i think it also makes sense to read it as the diode is connected between the collector and the emitter. Maybe it's just a basic knowledge to think that diode is connected to ground that we never get mistaken.

![[WhatsApp Image 2025-10-05 at 23.44.25_13f4ab5e.jpg]]
![[WhatsApp Video 2025-10-06 at 00.01.09_2c1cc4cb.mp4]]I'm not sure if this sounds about right.  the motor is turning on and off but the sound is going crazy,,,,
```cpp
const int transistorPin = 9;    // connected to the base of the transistor
 
 void setup() {
   // set  the transistor pin as output:
   pinMode(transistorPin, OUTPUT);
 }
 
 void loop() {
   digitalWrite(transistorPin, HIGH);
   delay(1000);
   digitalWrite(transistorPin, LOW);
   delay(1000);
 }
```

### Second Lab: DC Motor Control Using an H-Bridge

![[Pasted image 20251006184443.png]]
![[Pasted image 20251006220615.png]]

I connected all the wires but I couldn't understand what was going on .. so I asked Arjun.

```cpp
const int switchPin = 2;    // switch input
const int motor1Pin = 3;    // Motor driver leg 1 (pin 3, AIN1)
const int motor2Pin = 4;    // Motor driver leg 2 (pin 4, AIN2)
const int pwmPin = 5;       // Motor driver PWM pin

void setup() {
    // set the switch as an input:
    pinMode(switchPin, INPUT); 
 
    // set all the other pins you're using as outputs:
    pinMode(motor1Pin, OUTPUT);
    pinMode(motor2Pin, OUTPUT);
    pinMode(pwmPin, OUTPUT);
 
    // set PWM enable pin high so that motor can turn on:
    digitalWrite(pwmPin, HIGH);
  }
  
  void loop() {
    // if the switch is high, motor will turn on one direction:
    if (digitalRead(switchPin) == HIGH) {
      digitalWrite(motor1Pin, LOW);   // set leg 1 of the motor driver low
      digitalWrite(motor2Pin, HIGH);  // set leg 2 of the motor driver high
    }
    // if the switch is low, motor will turn in the other direction:
    else {
      digitalWrite(motor1Pin, HIGH);  // set leg 1 of the motor driver high
      digitalWrite(motor2Pin, LOW);   // set leg 2 of the motor driver low
    }
  }
```

Timelapse of Arjun explaining how this circuit works with motor.
![[WhatsApp Video 2025-10-06 at 22.11.18_dea5d1ed.mp4]]

Below are the materials that we looked at from arjun's blog post. Thank you Arjun.
![[Pasted image 20251006221535.png]]![[Pasted image 20251006221633.png]]
![[Pasted image 20251006221643.png]]

![[WhatsApp Video 2025-10-06 at 22.24.03_086ba0ad.mp4]]





I explained to Sophia how transistors work and how they accelerate the Voltage.

![[WhatsApp Video 2025-10-07 at 00.12.32_adcdb76c.mp4]]