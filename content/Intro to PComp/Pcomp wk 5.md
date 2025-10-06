This week, I dedicated most of my time to thoroughly reviewing all the readings and videos to fully understand the material.

![[WhatsApp Image 2025-10-05 at 17.05.57_5428032b.jpg]]
![[WhatsApp Image 2025-10-05 at 17.06.00_4deae1f9.jpg]
![[WhatsApp Image 2025-10-05 at 23.44.25_864ca168.jpg]]
![[WhatsApp Image 2025-10-05 at 23.44.26_1ede53ab.jpg]]
![[WhatsApp Image 2025-10-05 at 23.44.26_a4ab5f7a.jpg]]
![[WhatsApp Image 2025-10-05 at 23.44.28_611e2d4e.jpg]]
![[WhatsApp Image 2025-10-05 at 23.44.28_9fbf5af3.jpg]]
![[WhatsApp Image 2025-10-05 at 23.44.28_1f18560b.jpg]]
![[WhatsApp Image 2025-10-05 at 23.44.29_8df85a50.jpg]]


Questions from the Labs this week:
![[Pasted image 20251005233424.png]]![[Pasted image 20251005233431.png]]
In the schematics above the diode is connecting between the collector and the emitter. but the breadboard image shows that it's connecting the collector to the ground. Why is this?

oh wait i think I know. in the schematics shown above the diode is actually connecting from the collector to the ground.
![[WhatsApp Image 2025-10-05 at 23.44.25_13f4ab5e.jpg]]
![[WhatsApp Video 2025-10-06 at 00.01.09_2c1cc4cb.mp4]]I'm not sure if this sounds about right.  the motor is turning on and off but it's going crazy,,,,
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