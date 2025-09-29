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