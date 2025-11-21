### Nature’s Pulse 
I'm teaming up with Alua for the final project. 

We want to make a wall covered with fake moss and LEDs. When the person touches a certain part of the moss, the surrounding LEDs light up and flare in a heartbeat pattern.

**When you touch the moss, the wall breathes back. The heartbeat you see is not just light — it’s the Earth responding to human connection.**

#### Theme : Reconnecting humans with the living rhythm of nature.  
Meaning : The wall represents the planet - When a person touches it, the LEDs pulse like a heartbeat, suggesting that human touch revives or affects the Earth’s vitality.

#### Theme : Our presence leaves ripples in nature.
Meaning: The heartbeat flares visualize how even the smallest human interaction creates energetic responses in the environment. The project becomes a metaphor for how we affect ecosystems — gently or harmfully.

![[Screenshot 2025-11-11 at 1.44.34 PM.png]]

This is the link with the touch sensor DIY 
https://blog.arduino.cc/2019/11/13/rapidly-create-your-own-capacitive-multi-touch-sensors-with-this-kit/
below is our inspiration. 
![[Pasted image 20251111151218.png|]]



### 11/11 NOTES
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

### 11/13/2025 
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

### 11/14 office hour with Pedro
-  NeoPixel LED Outdoor Netting - 20 x 20 LEDs - 1x1 Meter Sizing
https://www.adafruit.com/product/6159
- Pedro mentioned there is a LED Net that you can use that will make our project coding easier if we want to go with the LED route. 

### 11/14 office hour with David
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

### 11/17 PROTOTYPE session 1
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

### 11/20 PROTOTYPE session 2

Adafruit NeoPixel LED Net and the Moss are delievered!
![[Pasted image 20251120221404.png]]

![[Pasted image 20251120221441.png]]


Today we explored how to wire the Neopixel Net to the Arduino and have it light up.
![[Pasted image 20251120221917.png]]
The power code we had was 12V 2A which is way less than what the description says for the full display (12A). We might be fine with getting a few of the LEDs light up but we will definitely be needing a power source that can carry close to 12A. **David Rios** recommended taking a look at **terminal block** which will keep the 12V off the breadboard(carries only 1A) and will allow thick wires to carry more Amps.

*document for LED net*
![[lighting-led-nets-with-wled-and-xlights.pdf]]


After so many versions of wiring we finally got the right way to wire the circuit that don't fry the breadboard, arduino, but still getting enough voltage to light up some LEDs.

The circuit had to have
1. **Common Ground**(Arduino GND = LED net GND)
2.  ** 330 Ω resistor** 
	1. Placed in series between Arduino pin 2 → LED `DI`
3.  **1000 µF capacitor**
	1. Across 12 V + and – right at the LED net input  
			→ stabilizes inrush current
	2. Put the capacitors:
		- Between **12 V +** and **GND**
		- **As close as possible** to the LED net’s power input
		This helps absorb inrush current when the LED drivers wake up.
	3. In the shop we had 470 µF so we connected two 470 µF capacitor parallel
4. connect 5V to Arduino through usb from Laptop
5. connect 12V2A power directly to LED net.
6. must **never** connect the LED net’s 12 V to Arduino 5 V pin.  
		Only connect _ground_.


```cpp
#include <Adafruit_NeoPixel.h>

// ----- LED net config -----
#define PIN_LED    2          // Data pin to LED net
#define WIDTH      20
#define HEIGHT     20
#define NUMPIXELS  (WIDTH * HEIGHT)

// ----- Switch pin -----
#define SW_PIN     3          // only one switch now

Adafruit_NeoPixel strip(NUMPIXELS, PIN_LED, NEO_GRB + NEO_KHZ800);

// ======== HELPERS ========

// Map (x,y) to index
int xyToIndex(int x, int y) {
  if (x < 0 || x >= WIDTH || y < 0 || y >= HEIGHT) return 0;
  return y * WIDTH + x;
}

void clearStrip() {
  for (int i = 0; i < NUMPIXELS; i++) {
    strip.setPixelColor(i, 0, 0, 0);
  }
}

// Fill a rectangular region
void fillRegion(int x0, int y0, int x1, int y1, uint32_t color) {
  if (x0 < 0) x0 = 0;
  if (y0 < 0) y0 = 0;
  if (x1 >= WIDTH)  x1 = WIDTH - 1;
  if (y1 >= HEIGHT) y1 = HEIGHT - 1;

  for (int y = y0; y <= y1; y++) {
    for (int x = x0; x <= x1; x++) {
      int idx = xyToIndex(x, y);
      strip.setPixelColor(idx, color);
    }
  }
}

// ======== SETUP ========

void setup() {
  strip.begin();
  strip.show();

  pinMode(SW_PIN, INPUT_PULLUP); // pressed = LOW
  Serial.begin(115200);
}

// ======== MAIN LOOP ========

void loop() {
  bool sw = (digitalRead(SW_PIN) == LOW);

  clearStrip();

  // ----- Example zone triggered by the ONE switch -----
  if (sw) {
    uint32_t c = strip.Color(0, 150, 0);  // green
    fillRegion(0, 0, 19, 19, c);          // entire net (change to any zone)
  }

  // Also react to p5 mapping messages
  handleSerialFromP5();

  strip.show();
  delay(20);
}

// ======== SERIAL HANDLER ========
// p5 sends: "x,y\n" to light a pixel

void handleSerialFromP5() {
  static char line[16];
  static byte idx = 0;

  while (Serial.available() > 0) {
    char c = Serial.read();

    if (c == '\n' || c == '\r') {
      if (idx > 0) {
        line[idx] = '\0';
        int x, y;
        if (sscanf(line, "%d,%d", &x, &y) == 2) {
          int index = xyToIndex(x, y);
          strip.setPixelColor(index, strip.Color(150, 150, 150));
        }
      }
      idx = 0;
    } else {
      if (idx < sizeof(line) - 1) {
        line[idx++] = c;
      } else {
        idx = 0;
      }
    }
  }
}

```
####  Switch Wiring
Since you're using ONE switch:
- One leg → **Pin 3**
- Other leg → **GND**
Because the sketch uses `INPUT_PULLUP`:
- Not pressed → HIGH
- Pressed → LOW (your trigger)


Our very first time running the code with the LED with a default switch wired. FAILED.
![[IMG_1196.mov]]

One wire was not connected prooerly! 
Now it works! yay!
![[IMG_1199.MOV.mov]]

11/22
we are going to make a part of the whole panel with all the elements in. 
starting with making the switch with moss on top. 