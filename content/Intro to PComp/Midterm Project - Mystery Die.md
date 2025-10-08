
For my midterm, I’m making a blank die that _talks_.  

It’s inspired by the Magic 8 Ball and decision dice — but with a twist. Unlike normal dice covered with numbers or dots which are made for people who can see, this one has **nothing on its faces**.

The idea is that **whether you have vision or not, everyone gets the result together**. You ask a question, roll the die, and when it settles, it responds with **sound** instead of visuals.

The idea came from thinking about **accessibility in play and interaction design** — how to make something as simple as rolling a die feel fair, shared, and a little bit magical for everyone in the room.

#### Conversation with David Rios

I talked with David Rios to get a sense of how to start my project — what to research first, what tools to learn, and how to plan the overall workflow. I wasn’t sure how to approach this project from start to finish, so I asked him to help me map out a general pipeline.

#### What to Do

**1. Start with the Code (Electronics)**
- Learn how the **IMU module** works. Try simple Arduino examples to read acceleration and orientation data.
- Test whether you can detect which side of the die is facing upward.
- Figure out the code part for connecting the pre-recorded .wav file to arduino.
	- Lab: Playing .WAV Files from an Arduino using I2S and SPI 
		- https://itp.nyu.edu/physcomp/lab-playing-wav-files-from-an-arduino/

**2. Prototype the Form**

- Use **cardboard boxes or Styrofoam** to create quick, lightweight prototypes.

- Check whether rolling the die actually gives readable IMU data and stable orientation detection.
    
- This helps confirm that the sensing logic is reliable before moving into fabrication.
    

**3. Move on to Fabrication**  
Once the electronics and sensing logic are proven:

- **Mounting and structure:**
    - Figure out how to fix the Arduino, speaker, and battery securely inside the die.
    - Look up **“standoffs”** for mounting, and check **Arduino hole size** (M2.5 or M3 screws).
        
- **Power:**
    - Decide on the power source — **LiPo battery** or **9V battery**.
    - The battery connects to the **VIN pin** on the Arduino.
        
- **Wiring:**
    - Plan how to connect everything neatly. Consider using a **solderable breadboard (perfboard)** for stability.
- **Speaker:
	-  
- **3D Printing
	- figure out 3D Modeling in Blender for 3D print
	- 


**Things to buy
1. Bread board mini
2. Solderable breadboard
3. Batteries
4. battery adapter
5. Speakers
6. standoffs

### 10/6/2025 - understanding IMU Module

### 1)
##### Accessing Accelerometer Data on Nano 33 IoT
https://docs.arduino.cc/tutorials/nano-33-iot/imu-accelerometer/?queryID=53eda9b0f085f7cf68001ef566077757

IMU stands for: inertial measurement unit.
IMU is an electronic device that measures and reports a body's specific force, angular rate and the orientation of the body, using a combination of accelerometers(가속도계), gyroscopes(회전), and oftentimes magnetometers.(자기장측정장치). it's a 3D digital linear acceleration sensor and a 3D digital angular rate sensor
![[Pasted image 20251006232851.png]]

##### Accelerometer
![[Pasted image 20251006234409.png]]
By using the accelerometer as a "level" that will provide information about the position of the board, we will be able to read what the relative position of the board is, as well as the degrees by tilting the board up, down, left or right.

```cpp
#include <Arduino_LSM6DS3.h>
float x, y, z;
int degreesX = 0;
int degreesY = 0;

void setup() {
  Serial.begin(9600);
  while (!Serial);
  Serial.println("Started");

  if (!IMU.begin()) {
    Serial.println("Failed to initialize IMU!");

    while (1);
  }

  Serial.print("Accelerometer sample rate = ");
  Serial.print(IMU.accelerationSampleRate());
  Serial.println(" Hz");

}

void loop() {


  if (IMU.accelerationAvailable()) {
    IMU.readAcceleration(x, y, z);
    
if(x > 0.1){
    x = 100*x;
    degreesX = map(x, 0, 97, 0, 90);
    Serial.print("Tilting up ");
    Serial.print(degreesX);
    Serial.println("  degrees");
    }
    
  if(x < -0.1){
    x = 100*x;
    degreesX = map(x, 0, -100, 0, 90);
    Serial.print("Tilting down ");
    Serial.print(degreesX);
    Serial.println("  degrees");
    }
    
  if(y > 0.1){
    y = 100*y;
    degreesY = map(y, 0, 97, 0, 90);
    Serial.print("Tilting left ");
    Serial.print(degreesY);
    Serial.println("  degrees");
    }
    
  if(y < -0.1){
    y = 100*y;
    degreesY = map(y, 0, -100, 0, 90);
    Serial.print("Tilting right ");
    Serial.print(degreesY);
    Serial.println("  degrees");
    }
 delay(1000);
  }
}
```

![[WhatsApp Video 2025-10-07 at 00.22.39_8bbae999.mp4]]



### 2) accelerometer orientation
This is the code that I got from Tom Igoe 
https://github.com/tigoe/SensorExamples/blob/main/Accelerometers/Arduino_LSM6DS3/AccelOrientation/AccelOrientation.ino

This code reads data from an **accelerometer** (a sensor that detects which way is "down" due to gravity) and figures out which way your Arduino board is oriented. It only prints the orientation when it's been stable for a while, so quick movements don't cause confusing output.

```cpp
/*
  Arduino LSM6DS3 orientation

  only prints out the accelerometer orientation if it is stable.
  avoids the problem of having to move through adjacent orientations,
  e.g. left -> top -> right. Instead, it will just print left -> right,
  if you move fast enough.

  created 29 Apr 2020
  by Tom Igoe

*/

#include <Arduino_LSM6DS3.h> //This line tells Arduino to include code that someone else wrote for talking to the LSM6DS3 sensor (the accelerometer chip). Think of it like importing a toolbox!

int lastOrientation = -1; // previous orientation of the accelerometer = stores what orientation we saw last time (starts at -1, meaning "no orientation yet")
int sameReading = 0;      // how many times you've gotten the same reading 
int threshold = 7;        // how many same readings make the reading stable = the magic number (7) - we need 7 identical readings before we trust it's stable

void setup() { // void means this function doesn't return any value - it just does stuff.
  // initialize serial communication:
  Serial.begin(9600); //This starts **serial communication** at 9600 baud (speed). Serial lets your Arduino send text messages to your computer so you can see what's happening!

  // start the IMU:
  if (!IMU.begin()) { //= "if the IMU fails to start..."
    Serial.println("Failed to initialize IMU!");
    while (true); //= infinite loop that does nothing - this stops the program if the sensor isn't working
  }
}

void loop() {
  // variables for accelerometer readings:
  float x, y, z; //= will store the acceleration in three directions (3D space!)
  // variable for orientation:
  int orientation = -1; //= will store which way the board is facing (0-5, or -1 for "unknown")
  
  // if the accelerometer's got readings,
  if (IMU.accelerationAvailable()) {
    // This checks: "Does the sensor have new data ready for us?" If yes, we go inside the `if` block.
    IMU.readAcceleration(x, y, z);

    // This reads the sensor and puts the values into our `x`, `y`, `z` variables. These values show how much acceleration (gravity) is pulling in each direction. Values are typically between -1.0 and 1.0.

For example, if the board is flat on a table with the top facing up, `z` will be close to 1.0 (gravity pulling down through the Z-axis).

//`abs()` = absolute value (removes negative signs). We want to know which axis has the **strongest** pull, regardless of direction. Example: `abs(-0.98)` = `0.98`
    int absX = abs(x);  
    int absY = abs(y);
    int absZ = abs(z);

    // if Z  is greatest, we must be on the Z axis:
    if ( (absZ > absX) && (absZ > absY)) {
      if (z > 0) {
        orientation = 0;  // Z up
      } else {
        orientation = 1;  // Z down
      }
      // if Y  is greatest, we must be on the Y axis:
    } else if ( (absY > absX) && (absY > absZ)) {

      if (y > 0) {
        // Y up:
        orientation = 2;
      } else {
        // Y down:
        orientation = 3;
      }
      // if X  is greatest, we must be on the X axis:
    } else if ( (absX > absY) && (absX > absZ)) {
      if (x < 0) {
        // X up:
        orientation = 4;
      } else {
        // X down:
        orientation = 5;
      }
    }

    // if we have a valid reading:
    if (orientation > -1 ) {
      // if the reading has been the same many times in a row,
      // then it is stable. print it:
      if (sameReading == threshold) {
        Serial.println(orientation);
      }

      // if the orientation has changed:
      if (orientation != lastOrientation) {
        // save the current for next time:
        lastOrientation = orientation;
        // clear sameReading:
        sameReading = 0;
      } else {
        // increment sameReading:
        sameReading++;
      }
    }
  }
}
```

![[WhatsApp Video 2025-10-07 at 00.37.24_d31fed42.mp4]]

### 3) Lab: Serial IMU Output to p5.js Using p5.webserial
https://itp.nyu.edu/physcomp/labs/lab-serial-imu-output-to-p5-js/

### 4)Lab: Playing .WAV Files from an Arduino using I2S and SPI
https://itp.nyu.edu/physcomp/lab-playing-wav-files-from-an-arduino/

