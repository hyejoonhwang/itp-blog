Nature’s Pulse 
I'm teaming up with Alua for the final project. 

We want to make a wall covered with fake moss and LEDs. When the person touches a certain part of the moss, the surrounding LEDs light up and flare in a heartbeat pattern.

**When you touch the moss, the wall breathes back. The heartbeat you see is not just light — it’s the Earth responding to human connection.**

Theme : Reconnecting humans with the living rhythm of nature.  
Meaning : The wall represents the planet - When a person touches it, the LEDs pulse like a heartbeat, suggesting that human touch revives or affects the Earth’s vitality.

Theme : Our presence leaves ripples in nature.
Meaning: The heartbeat flares visualize how even the smallest human interaction creates energetic responses in the environment. The project becomes a metaphor for how we affect ecosystems — gently or harmfully.

![[Screenshot 2025-11-11 at 1.44.34 PM.png]]

This is the link with the touch sensor DIY 
https://blog.arduino.cc/2019/11/13/rapidly-create-your-own-capacitive-multi-touch-sensors-with-this-kit/
below is our inspiration. 
![[Pasted image 20251111151218.png|]]
![[Pasted image 20251111151234.png]]
![[Pasted image 20251111151211.png]]

11/11 NOTES
questions and comment from class
- perhaps use screen made  with p5 that looks like LED panel 
- people wouldnt think to touch the wall. (looks like an art work) perhaps put it on the ground. 
- how would you guide the user to touch the wall. 
- how about make people step on it. 
- piezomicrophone ? for sensing the sound of plant real moss?     + plant + instrument
- there is a wall in the lobby in this building
- how would you place the sensor and light with the moss?

talk to 
prisha
ines
예슬 교수님 

media pipe https://ai.google.dev/edge/mediapipe/solutions/vision/hand_landmarker
ml5js
velostat : a pressure-sensitive, conductive plastic material that decreases in electrical resistance when squeezed or compressed

projector
kinnect
humidifier

![[Pasted image 20251111232110.png]]

11/13/2025 
Office Hour with Tom
- Try user testing first to see how people would interact with the piece. 
	- get moss
	- put the moss on the phone and let people interact with it. 
	
	OUTPUT
	- use projector or screen to see if you get the visual you are imagining.
	- if not LED is a way to go but it will be expensive.

	INPUT
		-force sensor with rubber nob/ wood 
		-vibrance sensor
		-piezomicrophone(arjun)
		-velostat(michael)

	PHYSICAL DESIGN
		- maybe have the design in a way that you don't have to map the whole panel into a grid of sensor

![[WhatsApp Image 2025-11-13 at 19.12.49_b74ebcba.jpg]]

11/14 office hour with Pedro
-  NeoPixel LED Outdoor Netting - 20 x 20 LEDs - 1x1 Meter Sizing
https://www.adafruit.com/product/6159
- Pedro mentioned there is a LED Net that you can use that will make our project coding easier if we want to go with the LED route. 

11/14 office hour with David
	-soft button
[https://kayitp.wordpress.com/2024/02/16/creating-textile-buttons/](https://kayitp.wordpress.com/2024/02/16/creating-textile-buttons/)

	- velostat
[https://www.instructables.com/O-mat/](https://www.instructables.com/O-mat/)

More sensors!
[https://www.kobakant.at/DIY/](https://www.kobakant.at/DIY/)
[https://www.instructables.com/member/Plusea/](https://www.instructables.com/member/Plusea/)

with more office hours with pcomp professors we concluded that
we want to make the sensors Digital input like switch than Analog input since we don't want the users to interact with force. 

So we decided to make a soft button with conductive materials like conductive fabric or copper tape. 

11/17 PROTOTYPE
for our first prototype, we wanted to get one NeoPixel LED turn on with one soft button. 

This is how our prototype softbutton looks like. 
![[KakaoTalk_20251118_130453875.jpg]]
Before pressing the button. Light turned OFF.
![[KakaoTalk_20251118_130453875_01.jpg]]

Soft button pressed. Light turned ON.
![[KakaoTalk_20251118_130453875_02.jpg]]
![[KakaoTalk_20251118_130502753.mp4]]
Below is the Code using the Neopixel library SIMPLE:
```cpp
// NeoPixel Ring simple sketch (c) 2013 Shae Erisson
// Released under the GPLv3 license to match the rest of the
// Adafruit NeoPixel library

#include <Adafruit_NeoPixel.h>
#ifdef __AVR__
 #include <avr/power.h> // Required for 16 MHz Adafruit Trinket
#endif

// Which pin on the Arduino is connected to the NeoPixels?
#define PIN        2 // NeoPixel data pin

// Button pin
#define BUTTON_PIN 3

// How many NeoPixels are attached to the Arduino?
#define NUMPIXELS 1

Adafruit_NeoPixel pixels(NUMPIXELS, PIN, NEO_GRB + NEO_KHZ800);

void setup() {
#if defined(__AVR_ATtiny85__) && (F_CPU == 16000000)
  clock_prescale_set(clock_div_1);
#endif

  pixels.begin(); // INITIALIZE NeoPixel strip object (REQUIRED)

  // Use internal pull-up resistor for the button
  pinMode(BUTTON_PIN, INPUT_PULLUP);
}

void loop() {
  int buttonState = digitalRead(BUTTON_PIN);

  if (buttonState == LOW) {
    // Button PRESSED -> turn pixel ON (green)
    pixels.setPixelColor(0, pixels.Color(0, 150, 0));
  } else {
    // Button NOT pressed -> turn pixel OFF
    pixels.clear();
  }

  pixels.show();    // Update the LED
  delay(10);        // Small delay for stability
}

```

This code is using the reversed logic with activating buttons and here's why:
![[Pasted image 20251118140842.png]]


11/18