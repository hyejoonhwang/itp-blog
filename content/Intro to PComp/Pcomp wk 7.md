ASCII table
![[Pasted image 20251103172114.png]]
ASCII stands for American Standard Code for Information Interchange. Computers can only understand numbers, so an ASCII code is the numerical representation of a character such as 'a' or '@' or an action of some sort. ASCII was developed a long time ago and now the non-printing characters are rarely used for their original purpose. Below is the ASCII character table and this includes descriptions of the first 32 non-printing characters. ASCII was actually designed for use with teletypes and so the descriptions are somewhat obscure. If someone says they want your CV however in ASCII format, all this means is they want 'plain' text with no formatting such as tabs, bold or underscoring - the raw format that any computer can understand. This is usually so they can easily import the file into their own applications without issues. Notepad.exe creates ASCII text, or in MS Word you can save a file as 'text only'

https://www.ascii-code.com/

That’s why `Serial.print()` is human-readable text, but `Serial.write()` is raw binary data — meant for machine-to-machine communication.


Think of it like this:

- **Serial.write(226)** → you’re sending **a single LEGO block** (raw byte).
    
- **Serial.print(226)** → you’re sending **three LEGO blocks shaped as “2”, “2”, and “6”** — readable but takes more space.
    

### 🔹 When raw binary is useful

You’d use `Serial.write()` when:

- Sending sensor data to another microcontroller or computer program that expects binary input.
    
- Communicating over a binary protocol (like MIDI, or a custom byte-based format).
    

You’d use `Serial.print()` when:

- You want to debug, log, or display readable text in the Serial Monitor.


```cpp
void setup() {

   // start serial port at 9600 bps:

  Serial.begin(9600);

}

void loop() {

// read analog input, map it to make the range 0-255:

  int analogValue = analogRead(A0);

  int mappedValue = map(analogValue, 0, 1023, 0, 255);

  Serial.println(mappedValue);

// print different formats:

  Serial.write(mappedValue);  // Print the raw binary value

  Serial.print('\t');             // print a tab

// print ASCII-encoded values:

  Serial.print(mappedValue, BIN); // print ASCII-encoded binary value

  Serial.print('\t');             // print a tab

  Serial.print(mappedValue);      // print decimal value

  Serial.print('\t');             // print a tab

  Serial.print(mappedValue, HEX); // print hexadecimal value

  Serial.print('\t');             // print a tab

  Serial.print(mappedValue, OCT); // print octal value

  Serial.println();               // print linefeed and carriage return

}
```

```cpp
const int switchPin = 2;

  

void setup() {

   // start serial port at 9600 bps:

  Serial.begin(9600);

  pinMode(switchPin, INPUT);

}

void loop() {

   // read the sensor:

   int sensorValue = analogRead(A0);

   // print the results:

   Serial.print(sensorValue);

   Serial.print(",");

   // read the sensor:

   sensorValue = analogRead(A1);

   // print the results:

   Serial.print(sensorValue);

   Serial.print(",");

   // read the sensor:

   sensorValue = digitalRead(switchPin);

   // print the results:

   Serial.println(sensorValue);

}
```


```cpp
const int switchPin = 2;

int sensorValue ;

  

void setup() {

   Serial.begin(9600);

   while (Serial.available() <= 0) {

     Serial.println("hello"); // send a starting message

     delay(300);              // wait 1/3 second

   }

}

  

void loop() {

   if (Serial.available()) {

      // read the incoming byte:

      int inByte = Serial.read();

      // read the sensor:

      sensorValue = analogRead(A0);

      // print the results:

      Serial.print(sensorValue);

      Serial.print(",");

      // read the sensor:

      sensorValue = analogRead(A1);

      // print the results:

      Serial.print(sensorValue);

      Serial.print(",");

      // read the sensor:

      sensorValue = digitalRead(switchPin);

      // print the results:

      Serial.println(sensorValue);

   }

}


```
Open the serial port
Wait for a Hello
Send a byte to request data
Begin loop:
  Wait for one set of data
  Send a byte to request new data
end loop
﻿

