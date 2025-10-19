** Create a sketch that asks people to make choices that have consequences.**

https://editor.p5js.org/hh3683/sketches/c1TSFtKBC

For this week’s homework, I built a simple interactive sketch in **p5.js** that lets the user choose between three activities: **Work, Sleep, and Play**. Each option has its own little animation when clicked. The goal was to practice using state variables, mouse input, and timed animations.

I designed three “cards” laid out horizontally:
 -**Work card**: A laptop flashes its screen.
-**Sleep card**: Floating “zzz” letters drift upward above a head on a bed.
-**Play card**: A game controller shakes left and right.

The animations only happen when the user clicks on the respective card, and they reset after finishing. 

**The Code Breakdown**

I kept the structure simple by using **boolean state variables** (`workFlash`, `sleepZZZ`, `playShake`) and counters like `workFrames` or `shakeFrames`.

- **Work Flash**:  
    When the card is clicked, `workFlash = true` and `workFrames` is set to a countdown. In `draw()`, I used `frameCount % 20` to make the laptop’s screen blink yellow every half-second. When the counter reaches zero, the flash stops.
- **Sleep ZZZ**:  
    Clicking the sleep card activates `sleepZZZ = true`. This makes “zzz” text rise upwards (`sleepZZZy -= 1`) until it goes off the card, then it resets.
- **Play Shake**:  
    Clicking the play card sets `playShake = true` and gives it a `shakeFrames` timer. While active, I offset the controller’s x-position with a sine wave to create a quick shaking motion.
**What Worked**

- The **click detection** worked right away once I defined the bounding boxes for each card.
- The **animations triggered correctly**—each one starts only when clicked and ends after a set duration.
- The use of **simple states** (true/false switches plus timers) kept everything easy to understand and debug.

**What Didn’t Work (and How I Fixed It)**

At first, I tried to make the laptop flash just by setting `workFlash = true` without a frame counter. The result was the screen blinking forever. I realized I needed a **timer variable** (`workFrames`) to control how long the animation lasted. Once I added `workFrames--` and turned `workFlash` off at zero, it worked as expected.

For the sleep animation, the text kept floating forever off-screen. To fix that, I reset `sleepZZZy` back to its starting position after it finished rising.

**Reflection**

This homework helped me understand how **state machines** work in interactive sketches. Even though the animations were simple, the pattern of “click → change state → animate → reset” is something I’ll be reusing a lot in future projects. It also showed me how important it is to limit animations with counters so they don’t run infinitely.