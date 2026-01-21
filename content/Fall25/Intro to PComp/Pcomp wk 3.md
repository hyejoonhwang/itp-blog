###### Going throughthe videos/readings

[**#1 Pseudo – analog output  
**](https://vimeo.com/380180086?fl=pl&fe=sh)practicing loop() function and understanding forloop.

![](https://hh3683-myeta.wordpress.com/wp-content/uploads/2025/09/adobe-express-kakaotalk_20250919_210554496.gif?w=720)

```cpp
int pwmVal;

void setup() {
  pinMode(11, OUTPUT);

}
void loop() {
for(int pwmVal=0; pwmVal<=255; pwmVal++){
analogWrite(11,pwmVal);
delay(20);

}

}
```
it took a while to fully understand this code , especially the for loop.
**#2 servo motor**

[Servo Control using Pulse Width Modulation  
](https://vimeo.com/372278570?fl=pl&fe=sh)

![](https://hh3683-myeta.wordpress.com/wp-content/uploads/2025/09/adobe-express-kakaotalk_20250919_214748479.gif?w=740)

```cpp
include <Servo.h> 
Servo myServo; 
void setup() { 
myServo.attach(9); 
} 
void loop(){
int sensor = analogRead(A0);
int angle = map(sensor, 0, 1023, 0, 180);
myServo.write(angle);
delay(20); }
```

[**#3 speaker tone output**](https://vimeo.com/372279218?fl=pl&fe=sh)
https://youtu.be/8PVqjp8jmf4

```cpp
void setup(){
Serial.begin(9600);
}
void loop(){
int sensor = analogRead(A0);
int frequency = map(sensor, 0, 1023, 100, 880);
tone(9, frequency);
delay (50);
}
```
at first I didn’t put ==Serial.begin(9600);== in the setup() so the sound was not gradually changing.

==I guess you have to put ==Serial.begin(9600);== in the setup() in order to deal with the analog value . (but in the video they didnt have ==Serial.begin(9600);== but it still worked so I’m confused.

#4 [analog output intro](https://vimeo.com/372276550#t=3m59s)

![](https://hh3683-myeta.wordpress.com/wp-content/uploads/2025/09/adobe-express-kakaotalk_20250920_132353263.gif?w=720)
```cpp
void setup(){ //pin the LED output to D9
 pinMode(9, OUTPUT); 
 } 
 void loop(){
  //potentiometer output is plugged into A0 so that it can read analog input 
int onTime = analogRead(A0); //when LEDis turned on 
digitalWrite(9,HIGH); //duration of the LED being on 
delayMicroseconds(onTime);
digitalWrite(9,LOW);//duration of the LED being off
delayMicroseconds(1023 - onTime);
    }
```
###### Lab Activity for Wk3

**#1 Check the sensor input range  
**

![](https://hh3683-myeta.wordpress.com/wp-content/uploads/2025/09/kakaotalk_20250920_191709913.gif?w=720)
```cpp
void setup() { Serial.begin(9600); // initialize serial communications 
} 
void loop() {
int analogValue = analogRead(A0);
// read the analog input 
Serial.println(analogValue);
// print it 
}
```
take away point :

the Arduino NANO 3.3IoT that we have cannot get voltage from 5V pin.

I had the + bus on the side wired from 5V and it didn’t work at first. but when I changed the output wire connected from 5V to 3.3V, it worked.

**#2 Check that the Speaker Works**
```cpp
void setup() {
// nothing to do here 
} void loop() {
// play the tone for 1 second:
tone(8, 440,1000);// do nothing else for the one second you're playing:
delay(1000);
}
```
the code above will make the sound continuously, to make it sound like a beep-beep pattern I made the duration of the sound shorter than the delay duration.

```cpp
void loop() {
tone(8, 440, 700); // 0.7 s on
delay(1000); // 1.0 s total cycle → ~0.3 s off 
}

void loop() { 
tone(8, 440); // start 
delay(700); // on
noTone(8); // stop
delay(300); // off 
}
```

**#3 Play Tones**
https://youtu.be/TQ9j_0GM8xs
```cpp
void setup() {
 // nothing to do here
 Serial.begin(9600);
 } void loop() {// get a sensor reading: 
int sensorReading = analogRead(A0);
Serial.println(sensorReading);// map the results from the sensor reading's range // to the desired pitch range: float frequency 
map(sensorReading,10, 900, 100, 1000); // change the pitch, play for 10 ms:
tone(8, frequency, 10);
delay(10);
}
```
ㄴ I added the highlighted lines above from the given code to find out the range of my FSR values and adjust the mappping.

---

**“If you want to know how to generate a tone without the `tone()` command, here’s the basic algorithm to play one wavelength:”**

```cpp
void makeTone(float frequency) {
 // set the period in microseconds: 
 int period = (1 / frequency) * 1000000; // turn the speaker on: 
 digitalWrite(speakerPin, HIGH); // delay half the period: 
 delayMicroseconds(period / 2); // turn the speaker off: 
 digitalWrite(speakerPin, LOW); // delay half the period: 
 delayMicroseconds(period / 2); }
```
my take away from this code above :

frequency = cycle per second (Hz)

period = 1 / frequency

multiplying by 1,000,000 converts seconds into microseconds

---
https://youtu.be/2VHrh-mjAlQ

```cpp
int speakerPin = 8;
float f = 0;
void setup() {// nothing to do here 
Serial.begin(9600);
pinMode(speakerPin, OUTPUT);// pinMode(speakerPin, HIGH); 
} void loop() {
int millisec = millis();
int seconds = millisec / 1000; 
       
if (seconds % 5 == 0) { 
f = random(260.00, 1000.00); 
} 

Serial.println(f); 
makeTone(f); // tone(speakerPin, f, 1000);
} 

void makeTone(float frequency) {
         // set the period in microseconds: 
int period = (1 / frequency) * 1000000;
          // turn the speaker on:, 
digitalWrite(speakerPin, HIGH);
           // delay half the period: 
delayMicroseconds(period / 2); // turn the speaker off: 
digitalWrite(speakerPin, LOW); // delay half the period: 
delayMicroseconds(period / 2); }
```

**#4 Play it Loud  
**

![](https://hh3683-myeta.wordpress.com/wp-content/uploads/2025/09/kakaotalk_20250921_000953418.jpg?w=768)

![](https://hh3683-myeta.wordpress.com/wp-content/uploads/2025/09/image-6.png?w=1024)

this excercise was confusing for me to understand the electricity flow of this circuit.

![](https://hh3683-myeta.wordpress.com/wp-content/uploads/2025/09/image-8.png?w=1024)

---

**#5 Musical Instrument**

![](https://hh3683-myeta.wordpress.com/wp-content/uploads/2025/09/image-10.png?w=319)

![](https://hh3683-myeta.wordpress.com/wp-content/uploads/2025/09/dddddddddddddd.png?w=579)

![](https://hh3683-myeta.wordpress.com/wp-content/uploads/2025/09/kakaotalk_20250923_143946912.jpg?w=766)
https://www.youtube.com/shorts/Ak8WeItg8K8

```cpp
include "pitches.h" 
const int threshold = 40; 
// minimum reading of the sensors that generates a note 
const int speakerPin = 8; // pin number for the speaker 
const int noteDuration = 20; // play notes for 20 ms //notes to play, corresponding to the 3 sensors: 
int notes[] = {
 NOTE_A4, NOTE_B4,NOTE_G4 };
 
void setup() { 
pinMode(speakerPin, OUTPUT); 
} 
void loop() {
for (int thisSensor = 0; thisSensor < 3; thisSensor++) 
} 
int sensorReading = analogRead(thisSensor);
Serial.println(sensorReading);// if the sensor is pressed hard enough: 
if (sensorReading > threshold) { 
// play the note corresponding to this sensor: 
tone(speakerPin, notes[thisSensor], noteDuration); delay(noteDuration); 
} 
} 
}
```

==NOTE_A4, NOTE_B4,NOTE_G4  
==ㄴ at first it included NOTE_C3 but it wasnt playing the note. and Arjun guess was right that NOTE_C3 have low frequency that the speaker couldn’t handle. so we changed it to NOTE_G4 and now it works.

==Serial.println(sensorReading);  
==ㄴ at first I had the threshold set to 10 which was too low to get rid of all the noise when not pressing any sonsors. So we added this ==Serial.println(sensorReading);== to figure out the maximum value it gives when not pressing any sensor. Then we adjusted the value to 40 and that’s how we removed the noise from the speaker.

==for (int thisSensor = 0; thisSensor < 3; thisSensor++) {  
// get a sensor reading:  
int sensorReading = analogRead(thisSensor);==

ㄴ Arjun helped me understand how a for loop works. now I understand fully how this code works.

![](https://hh3683-myeta.wordpress.com/wp-content/uploads/2025/09/kakaotalk_20250923_145732457.jpg?w=768)