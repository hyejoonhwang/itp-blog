 **Light Turn on/off Animation**
 [https://editor.p5js.org/hh3683/sketches/iEiTtFEor](https://editor.p5js.org/hh3683/sketches/iEiTtFEor)

**Q1. What did I do?**

ㄴBuilt a simple p5.js sketch that **toggles a light** with a mouse click. When the switch is **ON**, a large warm circle expands to brighten the whole canvas; when **OFF**, the circle shrinks back and the canvas returns to dark.

**Q2. What worked?**

ㄴ **Toggle logic:** `mousePressed()` flipping a `lightOn` boolean worked immediately. [p5.js](https://p5js.org/reference/p5/mousePressed/?utm_source=chatgpt.com)

**Q3. What didn’t work, and what steps did you take to try to solve the issue?**

ㄴ **Initial “static” light:** My first version only switched colors—no motion. I fixed it by **updating `r` every frame**: `r = r + 10` when ON; `r = r - 10` when OFF.

---
My original Idea was **earth's rotation/ revolution** and ended up just vibe coding the whole thing. It was fun playing around in 3D space in p5js.

[https://editor.p5js.org/hh3683/sketches/8fYc1wkBr](https://editor.p5js.org/hh3683/sketches/8fYc1wkBr)
