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


-------------------------------------------
Labs : Variables
https://itp.nyu.edu/physcomp/lessons/variables/
![[Pasted image 20250929031155.png]]
-------------------------------------------
Wk4 Labs
https://itp.nyu.edu/physcomp/labs/labs-arduino-digital-and-analog/lab-sensor-change-detection/