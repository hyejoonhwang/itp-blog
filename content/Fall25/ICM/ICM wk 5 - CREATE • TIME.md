
**Theme:** Organization of Time  
**Sketch:** Cardiogram Animation
https://editor.p5js.org/hh3683/sketches/KGINx6wkV

#### What I Did

I created a **cardiogram animation** using arrays to represent the rhythm of time as a heartbeat. Each x-coordinate represents the passage of time, and each y-coordinate stores the “pulse” value at that moment. The graph moves forward like a living timeline.

#### What Worked

Using an **array to store and push y-values** allowed me to precisely define the waveform of the heartbeat and repeat it to simulate ongoing time.  
I also discovered that using `line()` instead of `ellipse()` produces a smoother, more natural motion for the cardiogram without visual jitter.

#### What Didn’t Work (and What I Learned)

Initially, I drew **circles (`ellipse`)** for each frame, but it caused jitter and an uneven trace due to overlapping draws at high frame rates.  
Switching to **`line()`** eliminated that problem and created a continuous flow.  

#### Reflection

This sketch represents **time as a heartbeat**.  
Each pulse is a reminder that time is both mechanical and emotional: measured by machines yet experienced as something deeply human. The continuous red line mirrors both a machine’s clock and a living pulse — **a meeting point between data and life.**

#### How I Could Have Done Better

If I had more time, I would refine both the **logic and visual rhythm** of the cardiogram. Right now, the waveform is entirely predefined — every point is hardcoded into the array — so it repeats identically forever. A more dynamic approach would be to **generate the heartbeat procedurally**, using sine waves or Perlin noise to create natural variation, like how real heartbeats subtly change over time.