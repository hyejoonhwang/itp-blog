##### Questions from the lessons W1

I started by going through all the video lessons for week 1, but there were some fundamental physics concepts I couldn’t fully understand. One of them was Ohm’s Law.

In particular, the equation: Vout = Vin * (R2 / (R1 + R2)).

https://vimeo.com/380178637?fl=pl&fe=vl

<!-- wp:image {"id":51,"width":"312px","height":"auto","aspectRatio":"1.2084241103848947","sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large is-resized"><img src="https://hh3683-myeta.wordpress.com/wp-content/uploads/2025/09/screenshot-2025-09-06-180047.png?w=145" alt="" class="wp-image-51" style="aspect-ratio:1.2084241103848947;width:312px;height:auto"/></figure>
<!-- /wp:image -->
After watching the video on how to read schematics, I tried to build my circuit using only the schematic instead of looking at the breadboard image. Then I compared it with my breadboard to check if I got it right, and if not, what I missed.
<!-- wp:image {"id":80,"sizeSlug":"large","linkDestination":"none"} -->
<figure class="wp-block-image size-large"><img src="https://hh3683-myeta.wordpress.com/wp-content/uploads/2025/09/kakaotalk_20250908_104700297_02.jpg?w=766" alt="" class="wp-image-80"/><figcaption class="wp-element-caption">Building my own circuit without relying on the breadboard image made the learning process more challenging, but also effective and fun.</figcaption></figure>
<!-- /wp:image -->


**Below are some meaningful takeouts throughout the process of wk1 activity.**
![](https://hh3683-myeta.wordpress.com/wp-content/uploads/2025/09/kakaotalk_20250908_104700297_06-1.jpg?w=768)

At first, I was confused because my instinct told me it wouldn’t work, but I couldn’t explain why. Then I realized that plugging the anode and cathode into the same rails causes a short circuit.
![](https://hh3683-myeta.wordpress.com/wp-content/uploads/2025/09/kakaotalk_20250908_104752543_04.jpg?w=768)

This was my first time using a multimeter to measure current. If I remember correctly, we measure current in Amps, resistance in Ohms, and voltage in Volts.
![](https://hh3683-myeta.wordpress.com/wp-content/uploads/2025/09/kakaotalk_20250908_104752543_06.jpg?w=768)

Without the resistor, I could see nearly 5 volts running through the circuit because of the regulator. (I had the 12V power plugged in.)
![](https://hh3683-myeta.wordpress.com/wp-content/uploads/2025/09/kakaotalk_20250908_104939629_02.jpg?w=768)

With the resistor, I was only getting around 2.78 volts.
![](https://hh3683-myeta.wordpress.com/wp-content/uploads/2025/09/kakaotalk_20250908_105015767_02.jpg?w=766)

Again, I tried recreating the breadboard just by looking at the schematic.
![](https://hh3683-myeta.wordpress.com/wp-content/uploads/2025/09/kakaotalk_20250908_105015767_01.jpg?w=766)

I thought I had it right, but I was actually wrong. The schematic showed the LEDs connected in series, but I connected them in parallel.
![](https://hh3683-myeta.wordpress.com/wp-content/uploads/2025/09/kakaotalk_20250908_105015767_05.jpg?w=766)

When the LEDs were connected in series, the light was dimmed. I'm wondering if this is because the two LEDs are sharing the voltage equally. _**I’m curious about the exact math behind it.**_
![](https://hh3683-myeta.wordpress.com/wp-content/uploads/2025/09/kakaotalk_20250908_104752543_12.jpg?w=768)

In the same context, even if more LEDs are added, connecting them in parallel would not reduce the voltage each one receives, which means they would all emit light properly.
![](https://hh3683-myeta.wordpress.com/wp-content/uploads/2025/09/kakaotalk_20250908_105015767_06-1.jpg?w=768)

I added three LEDs in series which almost made them not emit light at all. at least I couldn't see the slightest light.


Before I proceed to the DC motor, I want to challenge myself to build a breadboard with three lights, each with its own designated switch, so that I have individual control over the lights through switches.

And I made it! I just needed to connect the wires from the resistor to the top of the switches so that there is current in the specific switch whenever I click on it.

![](https://hh3683-myeta.wordpress.com/wp-content/uploads/2025/09/kakaotalk_20250909_135543507-1.jpg?w=768)
![](https://hh3683-myeta.wordpress.com/wp-content/uploads/2025/09/kakaotalk_20250909_135543507_01.jpg?w=768)![](https://hh3683-myeta.wordpress.com/wp-content/uploads/2025/09/kakaotalk_20250909_135543507_02.jpg?w=768)
But the three images below show that when I push two switches at once, it provides different results depending on which LEDs are affected. I think this also has to do with different LEDs having different amounts of voltage needed to light up. In this case, the red LED light seems to need more energy to light up, but also the current or voltage prioritizes the red LED light over the blue or white LEDs. I'm curious the exact math/science behind this situation.

![](https://hh3683-myeta.wordpress.com/wp-content/uploads/2025/09/kakaotalk_20250909_135543507_05.jpg?w=768)

White and blue LEDs light up together when their designated switch is activated.

![](https://hh3683-myeta.wordpress.com/wp-content/uploads/2025/09/kakaotalk_20250909_135543507_03.jpg?w=768)

Only the red light is lighting up when it's swich is activated along with the white LED.

![](https://hh3683-myeta.wordpress.com/wp-content/uploads/2025/09/kakaotalk_20250909_135543507_04.jpg?w=768)

Only the red light is lighting up when it's swich is activated along with the blue LED.

###### **Insightful points from reading.

Reading 1

[https://worrydream.com/ABriefRantOnTheFutureOfInteractionDesign/](https://worrydream.com/ABriefRantOnTheFutureOfInteractionDesign/)

"**Pictures Under Glass” aren’t visionary—they’re stifling.**  
Victor criticizes popular futuristic UI demos (like Microsoft’s touch-screen-based visions), calling them a timid extension of tech we already have—not true innovation. He labels the dominant paradigm “Pictures Under Glass”—flat touchscreens that look nice but lack depth.

**Hands are underestimated—interaction must be embodied.**  
He reminds us that hands aren't just for visuals—they feel and manipulate. We gain massive tactile and proprioceptive feedback from holding objects: weight, texture, pressure, volume. Screens ignore all of that nuance.

**Touchscreens numb and rob us of expressive capability.**  
Using only a single flat surface strips away the tactile richness of physical tools. Victor compares reliance on glass screens to permanent numbness—visual over touch—and argues it's a dead-end if we make it the future.

**We need dynamic mediums—tangible, expressive, and rich.**  
The future of interaction should be tools that can be seen, felt, and manipulated in a dynamic physical sense. Think deformable surfaces, haptic materials, shape-changing interfaces, or interfaces yet to be invented.

**This is a call to researchers, designers, and funders—not a polished solution.**  
Victor admits the rant doesn't offer a specific prototype. Instead, it's meant to spark awareness and attract thoughtful innovation into tangible, dynamic interaction paradigms beyond the flat touch screen.

**Vision matters—don’t be constrained by yesterday’s tech.**  
He argues that setting the vision shapes what's built. If you let the touchscreen paradigm define the future, you’ll only get incremental improvements. Aim higher by leveraging human capabilities, not limiting them.

Reading 2

[**https://archive.ph/nhyCI**](https://archive.ph/nhyCI)

**The “invisible design” movement is trendy but misleading.**  
Everyone’s been repeating “the best design is invisible” (manifestos, books, #NoUI hashtags, Golden Krishna’s “No Interface” work, etc.). Arnall agrees touchscreens are problematic—but says the “invisible” framing is flawed.

**Problem #1 – It hides material reality.**  
Tech is not immaterial. Clouds are servers, sensors fail, networks break, batteries die. Pretending interfaces can “disappear” erases the fact that systems are physical, fragile, and political. Hiding seams reduces user agency and makes critique impossible.

**Problem #2 – It traps us in “natural” / “intuitive” myths.**  
Calling interfaces “intuitive” is lazy. If systems hide too much, users get confused—like “Reality Clippy” moments (why is my phone muted? why is my car unlocked?). The Nest thermostat works because it’s _visible_ and legible—not because it disappears.

**Problem #3 – It denies cultural significance.**  
Interfaces aren’t just tools, they’re culture. They shape aesthetics, expression, participation—just like TV, typography, architecture. Saying “the best UI is no UI” ignores that UIs are cultural media in themselves.

**Problem #4 – It ignores history.**  
Designers and theorists (Don Norman, Adam Greenfield, Matthew Chalmers, Durrell Bishop, Paul Dourish, Dieter Rams, etc.) have long warned against seamlessness/invisibility. The better tradition is “legible” or “seamful” design: making systems understandable, exposing seams honestly, letting users see and control.

Conclusion

- **We should abandon invisibility as a design goal.**  
    It’s dishonest, unhelpful, and leads to confusing, disempowering systems.
- **The real goal should be legibility and evidency.**  
    Like typography: good design is _readable_ and gives users control. Interfaces may _feel_ invisible once familiar—but only if they were designed to be clear, understandable, and culturally meaningful first.