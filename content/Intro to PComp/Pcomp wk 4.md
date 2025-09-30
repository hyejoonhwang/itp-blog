![[KakaoTalk_20250928_185553995.jpg]]
**take away from this week's Analog In and Out Review Questions :**
- always measure voltage against the ground. 
- ground gives us the reference of 0V since voltage going into the ground should be close to 0.
- and that's why we have pull down resistor.

William and I were confused at first about the relationship between R1 and Vout, R2 and Vout, and why when R1 increases Vout decreases, but when R2 increases Vout increases. 

And at the end William had a great water metaphor that instantly made so much sense. 
![[KakaoTalk_20250928_205959822.jpg]]

![[KakaoTalk_20250928_185553995_01 2.jpg]]

-------------------------------------------
Also we reviewed last week's video of Pseudo-analog output:
https://vimeo.com/380180086?fl=pl&fe=sh
![[KakaoTalk_20250929_011317711_02.jpg|200]]

![[Pasted image 20250929024105.png]]


```cpp
void setup(){
pinMode(11, OUTPUT);
}

void loop(){
for (byte pwmVal = 0; pwmVal <= 255; pwmVal++){
	analogWrite(11, pwmVal);
	delay(10);
	}
}
```
0-255(8 bit) : a number to represent the percentage of time that the wave should spend on or off. 

PWM controls the width of the pulse with the analogWrite() command. 
0 to 255 meaning LED going from dim to bright. 

tone(); is a frequency control, or the space between adjacent waveforms.
```cpp
void setup(){
pinMode(11, OUTPUT);
}

void loop(){
tone(11, 400, 500);
//tone(pin, frequency, duration);
delay(2000);
}
```
![[Screenshot 2025-09-28 at 3.56.19 PM.png]]

-------------------------------------------
Labs : Variables
https://itp.nyu.edu/physcomp/lessons/variables/
![[Pasted image 20250929031155.png]]
-------------------------------------------
Wk4 Labs
https://itp.nyu.edu/physcomp/labs/labs-arduino-digital-and-analog/lab-sensor-change-detection/
In this lab, you’ll learn how to program your microcontroller to look for three common changes in sensor readings that give you information about events in the physical world: **state change detection on digital sensors**, and **threshold crossing and peak detection on analog sensors.**
![[Pasted image 20250930135046.png |300]]

![[Pasted image 20250930135201.png |300]]



**Program the Microcontroller to Read the Pushbutton’s State Change**
```cpp
int lastButtonState = LOW;

void setup() {
   Serial.begin(9600);
   pinMode(2, INPUT);
}

  
void loop() {
  int buttonState = digitalRead(2);
  if (buttonState != lastButtonState){
      if (buttonState == HIGH) {
        Serial.println("Button was just pressed.");
      }
  }
  lastButtonState = buttonState;
}
```

**Count Button Presses**![[KakaoTalk_20250930_143453559.mp4]]
```cpp
int lastButtonState = LOW; //state of button last time you checked

int buttonPresses = 0; // count of button presses

void setup() {
   Serial.begin(9600);
   pinMode(2, INPUT);
}

void loop() {
  int buttonState = digitalRead(2);
  if (buttonState != lastButtonState){
      if (buttonState == HIGH) {
        buttonPresses++;
        Serial.println("Button was just pressed.");
        Serial.print(buttonPresses);
        Serial.println("times");
      }
  }
  lastButtonState = buttonState;
}
```


**Long Press, Short Press**
![[KakaoTalk_20250930_143336233.mp4]]
```cpp
int lastButtonState = LOW; //state of button last time you checked
int buttonPin = 2; //the input pin

//the length of the presses in ms:
int longPress = 750;
int shortPress = 250;
long pressTime = 0; //variable for how long the user actually presses

void setup() {
  //initialize serial and I/O pin:
   Serial.begin(9600);
   pinMode(buttonPin, INPUT);
}

void loop() {
  //read the button:
  int buttonState = digitalRead(buttonPin);

//if the button has changed:
  if (buttonState != lastButtonState){
      if (buttonState == HIGH) {
        pressTime = millis();
      }
      //if it's released, stop the timer:
      if (buttonState == LOW){
        long holdTime = millis() - pressTime;
        if (holdTime > longPress){
          Serial.println("long press");
        } else if (holdTime > shortPress){
          Serial.println("short press");
        } else {
          Serial.println("Tap");
        }
      }
  }
  lastButtonState = buttonState;
}
```


### Analog Sensor Threshold Detection
![[Pasted image 20250930140409.png|300]]

**Program the Microcontroller to Read a Sensor Threshold Crossing**
```cpp
int lastSensorState = LOW; //sensor's previous state
int threshold = 100; //an arbitrary threshold value

void setup() {
   Serial.begin(9600);
}

void loop() {
  int sensorState = analogRead(A0);
  if (sensorState >= threshold){
    if (lastSensorState < threshold){
      Serial.println("Sensor crossed the threshold");
    }
  }

  lastSensorState = sensorState;
}
```

**Detecting a Peak**
**Peak** is when an analog sensor reaches its highest value in a given time period. To detect a peak, you first set an initial peak value at 0.Pick a threshold below which you don’t care about peak values. Any time the sensor value rises above the peak value, you set the peak value equal to the sensor value. When the sensor value starts to fall, the peak will remain with the highest value:
![[Untitled video - Made with Clipchamp.mp4]]
```cpp
int peakValue = 0;
int threshold = 50;

void setup() {
   Serial.begin(9600);
}

void loop() {
  int sensorValue = analogRead(A0);
  if (sensorValue > peakValue){
    peakValue = sensorValue;
    Serial.println(peakValue);
  }
}
```

**Dealing with Noise**
![[KakaoTalk_20250930_143315468.mp4]]
You can smooth out the noise and ignore some of these local peaks by adding in a noise variable and checking to see if the sensor’s change is different than the previous reading and the noise combined
```cpp
int peakValue = 0;
int threshold = 50;
int noise = 5;

void setup() {
   Serial.begin(9600);
}

void loop() {
  int sensorValue = analogRead(A0);
  if (sensorValue > peakValue){
  peakValue = sensorValue;
  }

  if(sensorValue <= threshold - noise){
    if (peakValue > threshold + noise){
      //you have a peak value:
      Serial.println(peakValue);
      //reset the peak value:
      peakValue = 0;
    }
  }
}
```
