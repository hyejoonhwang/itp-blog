[Lab: Digital Input and Output with an Arduino](https://itp.nyu.edu/physcomp/labs/labs-arduino-digital-and-analog/digital-input-and-output-with-an-arduino/)

**1.**

![](https://hh3683-myeta.wordpress.com/wp-content/uploads/2025/09/kakaotalk_20250913_202631517_02.png?w=1024)

![](https://hh3683-myeta.wordpress.com/wp-content/uploads/2025/09/kakaotalk_20250913_205255996.jpg?w=768)

###### Button Not Pressed (open switch)

- The switch does **not** connect to +3.3 V.
- The shared row (the “node” with D2 + resistor) is only tied to **ground** through the 10 kΩ resistor.
- Voltage at D2 = **0 V (LOW)**.

###### Button Pressed (closed switch)
- The switch connects +3.3 V directly to the shared row (node).
- The node rises to **+3.3 V**, so D2 = **HIGH**.
- A small current (~0.33 mA) flows from +3.3 V → node → resistor → ground.

2.
![](https://hh3683-myeta.wordpress.com/wp-content/uploads/2025/09/screenshot-2025-09-15-203235.png?w=1024)

![](https://hh3683-myeta.wordpress.com/wp-content/uploads/2025/09/adobe-express-kakaotalk_20250913_204937809.gif?w=768)**When the button is pressed**

- Pin 2 goes HIGH (reads 3.3V).
- Arduino turns **Red LED (D3) ON**.
- Arduino turns **Yellow LED (D4) OFF**.

**When the button is released**

- Pin 2 goes LOW (0V).
- Arduino turns **Red LED (D3) OFF**.
- Arduino turns **Yellow LED (D4) ON**.

The button is acting as a **digital switch**.

The Arduino is reading that switch (HIGH/LOW).

Depending on the input, it chooses which LED to light.

At any moment, **only one LED is ON** (they alternate).
3.

![](https://hh3683-myeta.wordpress.com/wp-content/uploads/2025/09/kakaotalk_20250913_202631517-1.png?w=1024)

![](https://hh3683-myeta.wordpress.com/wp-content/uploads/2025/09/adobe-express-kakaotalk_20250913_204207529-1.gif?w=768)

![](https://hh3683-myeta.wordpress.com/wp-content/uploads/2025/09/screenshot-2025-09-15-203604.png?w=664)

I understood the hardware part of it but not so much of the code.

I really wanted to understand it though…

Below is what my GPT teacher taught me. I think I kind of get what it means.

![](https://hh3683-myeta.wordpress.com/wp-content/uploads/2025/09/screenshot-2025-09-15-212943.png?w=1024)

![](https://hh3683-myeta.wordpress.com/wp-content/uploads/2025/09/screenshot-2025-09-15-212959.png?w=1024)

![](https://hh3683-myeta.wordpress.com/wp-content/uploads/2025/09/screenshot-2025-09-15-213012.png?w=1024)

What I can take out of this :

**analogValue** will take the potentiometer value information. that will range **0 – 1023**.

**brightness** will hold the scaled version of **analogValue** by dividing the **analogValue** by 4, it will range **0 – 255**.

**Serial.begin(9600);** – Starts the **serial connection** so the Arduino can send data to your computer at 9600 bits per second.

pinMode(ledPin, OUTPUT); – Without this, Arduino wouldn’t know whether to drive the pin or just read it.

analogWrite(ledPin, brightness); – Sends a PWM signal to pin 9 with duty cycle based on `brightness`.

4. using FSR : didnt not work as it supposed to .

![](https://hh3683-myeta.wordpress.com/wp-content/uploads/2025/09/adobe-express-kakaotalk_20250913_221331074.gif?w=768)

![](https://hh3683-myeta.wordpress.com/wp-content/uploads/2025/09/kakaotalk_20250913_221308955.jpg?w=768)

![](https://hh3683-myeta.wordpress.com/wp-content/uploads/2025/09/screenshot-2025-09-13-220951.png?w=1024)

```cpp
void loop() {

  rightSensorValue = analogRead(A0); // read the pot value

  // map the sensor value from the input range (400 – 900, for example)

  // to the output range (0-255). Change the values 400 and 900 below

  // to match the range your analog input gives:

  int brightness = map(rightSensorValue, 10, 900, 0, 255);

  brightness = constrain(brightness, 0, 255);

  analogWrite(whiteLED, brightness);  // set the LED brightness with the result

  serial.print(“sensor: “);

  Serial.println(rightSensorValue);   // print the sensor value back to the serial monitor

  serial.print(”  brightness: “);

  Serial.printIn(brightness);

  delay(1);
```