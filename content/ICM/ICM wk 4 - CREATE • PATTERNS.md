### HW CREATE • PATTERNS
https://editor.p5js.org/hh3683/sketches/97r9SoWH0 

#### **What didn’t work (and what I learned)**

At first, I really struggled with understanding how to use **three nested `for` loops**. It was hard to visualize how each level interacted—what runs first, and how values repeat. With help from **William Yao**, I started to think about it like layers of rhythm: the innermost loop completes fully before the next layer moves one step forward. That made everything click.

I also didn’t know how to **offset every other row**. William explained that I could check which row I’m on by using `(y / 50 % 2 == 1)`—and then apply a horizontal shift. Once I added that condition, the grid instantly looked more interesting.

What I **failed to do** was the interactive part: I wanted the circles to animate or react when the mouse hovers over them. I tried with some AI-generated suggestions, but most of the code felt too complex for me to fully understand yet. I decided not to include something I couldn’t explain—so for now, I kept it static. But I plan to revisit this idea once I learn more about detecting object positions and hover interactions.

vibe code with animation:
https://editor.p5js.org/hh3683/sketches/muL86lPGD

### class notes


**exercise: practicing direction change**
```js
let x;
let y;
let size = 20;
let xspeed = 2;
let sizeSpeed=2;

function setup() {
  createCanvas(400, 400);
  x = width / 2;
  y = height / 2;
}

function draw() {
  background(220);

  x += xspeed;
  circle(x, y, size);

  if (x >= width || x <= 0) {
    //xspeed= xspeed* -1
    xspeed *= -1;
  }
  
  
  size += sizeSpeed;
  if (size > 47 || size < 3) {
    sizeSpeed *= -1;
  }
}

```

 **Practicing for loop**
- parts of a for loop:
1. starting point
2. stopping point
3. change amount(positive or negative)

```js
for(let counter = 0; counter < 10; counter+= 1){
console.log("the loop ran");
console.log(counter);
}
```

```js
function setup() {
  createCanvas(400, 400);
  background(220);

  for (let i = 0; i < 100; i++) {
    let x = random(0, 400);
    let y = random(0, 400);

    let size = random(10, 50);

    let r = random(0, 255);
    let g = random(0, 255);
    let b = random(0, 255);
    let a = random(0, 255);
    noStroke();
    fill(r, g, b, a);

    circle(x, y, size);
    console.log(r, g, b);
  }

  function draw() {}
}
```

```js
let column = 13;
let columnWidth;

function setup() {
  createCanvas(400, 400);
  columnWidth = width / column;
}

function draw() {
  background(200);
  fill("red");

  for (let i = 0; i < column; i++) {
    let xpos = columnWidth * i;
    if (mouseX > xpos && mouseX < xpos + columnWidth) {
      rect(xpos, 0, columnWidth, height);
    }
  }
}

```