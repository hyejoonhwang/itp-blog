
## Bouncing Cube Simulation
https://editor.p5js.org/hh3683/sketches/jzgUjtY6S
#### Simulating Physics: Building a Bouncing Cube
This week I built a bouncing cube simulation. I wanted to understand how to model real-world physics using code. The cube falls, bounces, and eventually stops. It sounds simple, but I had to think about gravity, speed, friction, and collisions. I had to program all of that into my code.

#### The Physics in My Code
I used four main ideas to make the cube move realistically.

**Gravity** pulls the cube down. Every frame, I add 0.3 to the downward speed. This makes the cube fall faster and faster, just like in real life.

**Bouncing** happens when the cube hits the ground. The speed reverses direction and gets weaker each time. I keep track of how many bounces happened so each bounce can be a little weaker than the last one.

**Friction** slows the cube down. Every frame, I multiply the speed by 0.98. Over time, this makes the cube slower and slower.

**Walls** bounce the cube back when it hits the edges. This keeps the cube from flying off the screen.

When I wrote this code, I had to make choices. How strong should gravity be? How much energy should be lost when the cube bounces? How much friction should there be? There's no one right answer. Different answers create different results.

--------------------------------------------

Also this week, I learned about Objects so I went back to the vibe code that I had for week 4 and took some time to understand what was actually happening in the code. After that I added another object colour to enhance the code. 
https://editor.p5js.org/hh3683/sketches/muL86lPGD

![[WhatsApp Image 2025-10-20 at 01.46.27_6e3bffa4.jpg]]
![[WhatsApp Image 2025-10-20 at 01.46.27_bcc5a6af.jpg]]










### class notes:
	-array.splice()
	- objects
	- class


```js
let names = ["summer", "aram", "jane", "guy", "mina", "siddhi"];

function setup() {
  createCanvas(400, 400);
  background(200);

  let button = createButton("Getname");
  button.mousePressed(getName);
}

function getName() {
  let indexPos = int(random(0, names.length));

  console.log("indexPos" + indexPos);
  console.log(names[indexPos]);
  names.splice(indexPos, 1);
}

```



```js
let moonBall;
let moonBalls = [];

function setup() {
  createCanvas(400, 400);

  // //create one moonBall
  // moonBall = new Moon(size);
  // console.log(moonBall);

  //make 30 moonBalls
  for (let i = 0; i < 30; i++) {
        let size = random(5,15);
    moonBall = new Moon(size);
    moonBalls.push(moonBall);
  }
}

function draw() {
  background(220);

  //call the object moonBall from the class Moon
  moonBall.exist();
  moonBall.moving();
  moonBall.bouncing();

  //access 30 moonBalls in the array moonBalls
  for (let i = 0; i < moonBalls.length; i++) {
    moonBalls[i].exist();
    moonBalls[i].moving();
    moonBalls[i].bouncing();
  }
}

class Moon {
  //set up variable of the moon objects
  constructor(size) {
    this.x = width / 2;
    this.y = height / 2;
    this.xspeed = random(1, 10);
    this.yspeed = random(1, 10);
    this.size = size;
  }

  //functions within class Moon
  exist() {
    circle(this.x, this.y, this.size);
  }

  moving() {
    this.x += this.xspeed;
    this.y += this.yspeed;
  }

  bouncing() {
    if (this.x > width || this.x < 0) {
      this.xspeed *= -1;
    }
    if (this.y > height || this.y < 0) {
      this.yspeed *= -1;
    }
  }
}

```