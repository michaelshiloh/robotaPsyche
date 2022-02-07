
- Class [Zoom](https://nyu.zoom.us/j/92332634151)
- Shortcut to [today's assignment](weeklySchedule.md/#todays-assignment)    
- Shortcut to [today's lecture](lectureNotes.md/#todays-lecture)    

## New York University Abu Dhabi  
## Interactive Media Program
## Course title: Robota Psyche  
Course number: IM-UH 3313  
Credit Hours: 4       
Prerequisites: One of  
- Introduction to Interactive Media (IM-UH 1010)
- Mashups (IM-UH 2110)
- Politics of Code (IM-UH 3110)
- Introduction to Computer Science (CS-UH 1001)
- IM-UH 2310 Mashups—Creating with Web APIs
- IM-UH 2318 Decoding Nature
- ENGR-UH 1000 Computer Programming for Engineers
- or instructor approval  

Course website:
[https://github.com/michaelshiloh/robotaPsyche](https://github.com/michaelshiloh/robotaPsyche)    
Instructor: Michael Shiloh mshiloh@nyu.edu    
Office hours: By appointment  
Meeting times:        
10:25 AM - 1:05 PM Monday      
11:50 AM - 1:05 PM Wednesday      
Session: Spring 2022     

This document: Lecture Notes

**This is subject to change**

### Week 1

### January 24

Download Processing from [here](https://processing.org/download). If you
prefer, you may use [p5.js](https://editor.p5js.org/)

#### Introduction

- Who am I?
- [Syllabus](syllabus.md)
- Summary:
  - Be present and participate in class
  - No electronic distractions (**No notifications**)
  - Be proactive: 
		- Communicate with me regarding difficulties, problems, illness, etc.
  - Allow lots of time for homework 
		as debugging programs can be very time consuming.
- All class material will be on this Github repository 
	with the exception of some reading material which will be
	accessible through the Brightspace
- [weekly schedule](weeklySchedule.md)
- This class is a work in progress; your input is welcome and desired
- Who are you?
	- What attracted you to this class?
	- If you could design an ideal robot friend, what would they be like?
- What is this class about?
	- What is synthetic psychology?

#### Homework

- Look at homework which is always in [weeklySchedule.md](weeklySchedule.md)

### January 26

#### Announcements

- I made a mistake and had to change the Zoom meeting URL. I have updated all
	the pages on Github with the new Zoom URL 
- Since the first meeting was with the old link, you won't be able to
	find that meeting recording in Brightspace. However you can access
	the first meeting recording
	[here](https://nyu.zoom.us/rec/share/tQRkXacgvLkqUDX6ad8ynEdxvA9r6r4ykmuTULpRTFogmlWFajUc7UPNm7gM-CtC.GV4Cue80NDdWXG3i)

#### Questions

Did the readings (Vehicles) inspire any questions?

#### Review

- [PVectors](https://natureofcode.com/book/)

### January 31

#### Announcements

- I made a mistake **again**and had to change the Zoom meeting URL. 
	I have updated all
	the pages on Github with the new Zoom URL 
- Since the first **and second**
	meeting was with the old link, you won't be able to
	find that meeting recording in Brightspace. 
	- You can access the first meeting recording
	[here](https://nyu.zoom.us/rec/share/tQRkXacgvLkqUDX6ad8ynEdxvA9r6r4ykmuTULpRTFogmlWFajUc7UPNm7gM-CtC.GV4Cue80NDdWXG3i)
	- You can access the second meeting recording
	[here](https://nyu.zoom.us/rec/play/d9_XmF2KN0wsrsEqFM3tRGX8d-0FFoZnqoEe56Hy8StgeHqdmbbKeHq8NHC8B4MRcCc4Ehy5RX8zim8J.i6GxNEJkcjl959_T)
- Next week we will start with discussions and presentations. 
	- Discussions and presentations will be due on Wednesdays
	- We will have two discussions and one presentation per meeting
	- Mondays will be for lecture


#### Homework (maybe after the break)
- Let's take a quick look at your ideas

#### Review

- [Object Oriented Programming](https://github.com/michaelshiloh/resourcesForClasses/tree/master/src/p5jsSketches/simpleP5jsClassExample)

- [PVectors](https://natureofcode.com/book/)
	- I think we got to vector subtraction, and the example that draws
		a line from center of the canvas to the mouse
	- Continue with Vector multiplication (and division)
	- 1.5 Vector Magnitude
	- 1.6 Normalizing Vectors
- Finally let's put this to use
	- 1.7 Vector Motion: Velocity (we looked at this last week)
	- 1.8 Vector Motion: Acceleration
	- 1.9 Static vs. Non-Static Functions

Here is the code I developed at the end of the lecture:

````

// An array of objects
Mover[] movers = new Mover[20];
// Try making 100 or even 1000 robots!

void setup() {
  size(640,360);
  background(255);
  for (int i = 0; i < movers.length; i++) {
    // Initialize each object in the array.
    movers[i] = new Mover(i*.05);
  }
}

void draw() {
  background(255);

  for (int i = 0; i < movers.length; i++) {
    //[full] Calling functions on all the objects in the array
    movers[i].update();
    movers[i].checkEdges();
    movers[i].display();
    //[end]
  }
}

class Mover {

  PVector location;
  PVector velocity;
  PVector acceleration;
  float topspeed;
  float hunger;

  Mover(float _hunger) {
    location = new PVector(random(width),random(height));
    velocity = new PVector(0,0);
    topspeed = 4;
    hunger = _hunger;
  }

  void update() {

    // <b><u>Our algorithm for calculating acceleration</b></u>:

    //[full] Find the vector pointing towards the mouse.
    PVector mouse = new PVector(mouseX,mouseY);
    PVector dir = PVector.sub(mouse,location);
    //[end]
    // Normalize.
    dir.normalize();
    // Scale.
    dir.mult(hunger);
    // Set to acceleration.
    acceleration = dir;

    //[full] Motion 101! Velocity changes by acceleration.  Location changes by velocity.
    velocity.add(acceleration);
    velocity.limit(topspeed);
    location.add(velocity);
    //[end]
  }

  // Display the Mover
  void display() {
    stroke(0);
    fill(175);
    ellipse(location.x,location.y,16,16);
  }

  // What to do at the edges
  void checkEdges() {

    if (location.x > width) {
      location.x = 0;
    } else if (location.x < 0) {
      location.x = width;
    }

    if (location.y > height) {
      location.y = 0;
    }  else if (location.y < 0) {
      location.y = height;
    }
  }
}

````

### February 2

#### Admin
- Record

#### Homework 
What ideas did you have for other movers 
and what rules would govern them?

#### Adding forces to our simulation tools

Jump down to 2.2 Forces and Processing—Newton’s Second Law as a Function in
[chapter 2](https://natureofcode.com/book/chapter-2-forces/)


##### todays-lecture
### February 9

#### Admin
- Record

#### Discussion
- Casey Reas
- Isaac Asimov

#### Attractors

We're going to skip over friction for now and introduce Attractors

Jump down to 2.9 Gravitational Attraction

````
// The direction of the attraction force that object 1 exerts on object 2 
PVector dir = PVector.sub(location1,location2);
dir.normalize();
````

The magnitude, according to the gravitational force equation:

````
float magnitude  = (G * mass1 * mass2) / (distance * distance);
````

We had the distance after we did the subtraction, but before we did the
normalization, so let's grab it and complete the calculation:

````
// The direction of the attraction force that object 1 exerts on object 2 
PVector dir = PVector.sub(location1,location2); // Note use of static function!
float distance = dir.magnitude(); // save the distance before we normalize dir

// calculate the magnitude of the force
float magnitude = (G * mass1 * mass2) / (distance * distance);

// The force is a vector in the direction of *dir* of magnitude *magnitude*
dir.normalize(); // normalize the direction
dir.mult(magnitude); // now dir is actually the force so the name is misleading
````

Since the vector `dir` becomes the vector `force`, in the book they just call 
it `force` from the beginning

Next: implement an `Attractor` class!

The attractor needs to tell us how much force it will exert on a mover:

````
PVector f = a.attract(m); // Force the attractor exerts on a mover
m.applyForce(f); // Apply that force to the mover
````

And of course we have to implement that `Attractor.attract()` function.

Once done we have:

````
// A Mover and an Attractor
Mover m;
Attractor a;

void setup() {
  size(640,360);
  m = new Mover();
  a = new Attractor();
}

void draw() {
  background(255);

  // Apply the attraction force from the Attractor on the Mover.
  PVector force = a.attract(m);
  m.applyForce(force);
  m.update();

  a.display();
  m.display();
}

class Attractor {
  float mass;
  PVector location;
  float G;

  Attractor() {
    location = new PVector(width/2,height/2);
    mass = 20;
    G = 0.4;
  }

  PVector attract(Mover m) {
    PVector force = PVector.sub(location,m.location);
    float distance = force.mag();
    // Remember, we need to constrain the distance
    // so that our circle doesn’t spin out of control.
    distance = constrain(distance,5.0,25.0);


    force.normalize();
    float strength = (G * mass * m.mass) / (distance * distance);
    force.mult(strength);
    return force;
  }

  void display() {
    stroke(0);
    fill(175,200);
    ellipse(location.x,location.y,mass*2,mass*2);
  }
}


// Mover class copied from section 2.1:
// with some modifications in checkEdges
class Mover {

  PVector location;
  PVector velocity;
  PVector acceleration;
  // The object now has mass!
  float mass;

  Mover() {
    // And for now, we’ll just set the mass equal to 1 for simplicity.
    mass = 1;
    location = new PVector(30,30);
    velocity = new PVector(0,0);
    acceleration = new PVector(0,0);
  }

  // Newton’s second law.
  void applyForce(PVector force) {
    //[full] Receive a force, divide by mass, and add to acceleration.
    PVector f = PVector.div(force,mass);
    acceleration.add(f);
    //[end]
  }

  void update() {
    //[full] Motion 101 from Chapter 1
    velocity.add(acceleration);
    location.add(velocity);
    //[end]
    // Now add clearing the acceleration each time!
    acceleration.mult(0);
  }

  void display() {
    stroke(0);
    fill(175);
    //[offset-down] Scaling the size according to mass.
    ellipse(location.x,location.y,mass*16,mass*16);
  }

  // With this code an object bounces when it hits the edges of a window.
	// Alternatively objects could vanish or reappear on the other side
	// or reappear at a random location or other ideas

  void checkEdges() {
    if (location.x > width) {
      location.x = width;
      velocity.x *= -1;
    } else if (location.x < 0) {
      location.x = 0;
      velocity.x *= -1;
    }

    if (location.y > height) {
      location.y = height;
      velocity.y *= -1;
    } else if (location.y < 0) {
      location.y = 0;
      velocity.y *= -1;
    }
  }
}

````

In-class exercises

1. Move the Attractor with the mouse
2. Add multiple movers
3. Add a second Attractor
4. Have the movers leave a trail behind them
