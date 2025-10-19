
1. what is simulation? 
	At its core, **simulation** is the process of **imitating the behavior or operation of a real-world system, process, or environment**—usually through a computer model. It lets us explore **“what would happen if…”** scenarios without needing to test them in the real world.


 **Core Components**
1. **Model** – abstraction of reality (rules, objects, parameters)
2. **Input Data** – initial conditions (e.g., number of users, speed, temperature)
3. **Simulation Engine** – the logic or algorithm that evolves the system over time
4. **Output** – results, visualizations, or performance metrics
























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