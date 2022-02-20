
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

#### Homework 
What ideas did you have for other movers 
and what rules would govern them?

#### Adding forces to our simulation tools

Jump down to 2.2 Forces and Processing—Newton’s Second Law as a Function in
[chapter 2](https://natureofcode.com/book/chapter-2-forces/)

### February 7

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

### February 9

#### Presentation 

#### 2.10 Everything Attracts (or Repels) Everything

In draw, we'd like to say “for every mover i, be attracted to every other mover j, and update and display yourself.”

````
void draw() {
  background(255);

  for (int i = 0; i < movers.length; i++) {
    for (int j = 0; j < movers.length; j++) {
      // Don’t attract yourself!
      if (i != j) {
        PVector force = movers[j].attract(movers[i]);
        movers[i].applyForce(force);
      }
    }
    movers[i].update();
    movers[i].display();
  }
}
````

So we need to add an `attract()` method to the `Mover` class.
But we already wrote such a method for the `Attractor` class, so just copy it!

````
class Mover {

  // All the other stuff we had before plus. . .

  // The Mover now knows how to attract another Mover.
  PVector attract(Mover m) {

    PVector force = PVector.sub(location,m.location);
    float distance = force.mag();
    distance = constrain(distance,5.0,25.0);
    force.normalize();

    float strength = (G * mass * m.mass) / (distance * distance);
    force.mult(strength);
    return force;
  }
}
````

What if we make vehicles unique in some way e.g. different colors, and what if
we made them attracted to vehicles of the same color but repeled by vehicles
of the other color?

````
// Movers and an Attractor
Mover[] movers = new Mover[100];
Attractor a;

void setup() {
  // I wanted to see what it looked like if it filled my screen (almost)
  size(1800, 1000);

  for (int i = 0; i < movers.length; i++) {
    // Each Mover is initialized randomly.
    movers[i] = new Mover(random(0.1, 2), // mass
      random(width), random(height)); // initial location
  }

  a = new Attractor();
}

void draw() {
  background(255);


  // For each mover
  for (int i = 0; i < movers.length; i++) {

    // This is the part that I forgot to add in class:
    // Apply the attraction force from the Attractor on this mover
    PVector aForce = a.attract(movers[i]);
    movers[i].applyForce(aForce);

    // Now apply the force from all the other movers on this mover
    for (int j = 0; j < movers.length; j++) {
      // Don’t attract yourself!
      if (i != j) {
        PVector force = movers[j].attract(movers[i]);
        movers[i].applyForce(force);
      }
    }
    movers[i].update();
    movers[i].checkEdges();
    movers[i].display();
  }
}


class Attractor {
  float mass;
  PVector location;
  float G;

  Attractor() {
    location = new PVector(width/2, height/2);

    // Big mass so the force is greater than vehicle-vehicle force
    // You can play with this number to see different results
    mass = 70;
    G = 0.4;
  }

  PVector attract(Mover m) {
    PVector force = PVector.sub(location, m.location);
    float distance = force.mag();
    // Remember, we need to constrain the distance
    // so that our vehicle doesn’t spin out of control.
    distance = constrain(distance, 5.0, 25.0);

    force.normalize();
    float strength = (G * mass * m.mass) / (distance * distance);
    force.mult(strength);
    return force;
  }

  void display() {
    stroke(0);
    fill(175, 200);
    ellipse(location.x, location.y, mass*2, mass*2);
  }
}


// Mover class from Monday with modifications:
// attract() method allows vehicles to attract or repel each other
// myColor allows vehicles to be either red or blue
// with some modifications in checkEdges
class Mover {

  PVector location;
  PVector velocity;
  PVector acceleration;
  float mass;
  float G = 0.4;
  int myColor;

  Mover(float _mass_, float _x_, float _y_) {
    mass = _mass_;
    location = new PVector(_x_, _y_);
    velocity = new PVector(0, 0);
    acceleration = new PVector(0, 0);
    myColor = round(random(1));
  }

  // Newton’s second law.
  void applyForce(PVector force) {
    // Receive a force, divide by mass, and add to acceleration.
    PVector f = PVector.div(force, mass);
    acceleration.add(f);
  }

  // The Mover now knows how to attract another Mover.
  PVector attract(Mover m) {

    PVector force = PVector.sub(location, m.location);
    float distance = force.mag();
    distance = constrain(distance, 5.0, 25.0);
    force.normalize();

    float strength = (G * mass * m.mass) / (distance * distance);
    force.mult(strength);

    // If the color is different then we will be repelled
    if (myColor != m.myColor) force.mult(-1);

    return force;
  }


  void update() {
    velocity.add(acceleration);
    location.add(velocity);
    acceleration.mult(0);
  }

  void display() {
    stroke(0);
    if (myColor == 1) fill(255, 0, 0);
    else fill (0, 0, 255);

    // Rotate the mover to point in the direction of travel
    // Translate to the center of the move
    pushMatrix();
    translate(location.x, location.y);
    rotate(velocity.heading());
    // It took lots of trial and error
    // and sketching on paper
    // to get the triangle
    // pointing in the right direction
    triangle(0, 5, 0, -5, 20, 0);
    popMatrix();
  }

  // With this code an object bounces when it hits the edges of a window.
  // Alternatively objects could vanish or reappear on the other side
  // or reappear at a random location or other ideas. Also instead of
  // bouncing at full-speed vehicles might lose speed. So many options!

  void checkEdges() {
    if (location.x > width) {
      location.x = width;
      velocity.x *= -1; // full velocity, opposite direction
      velocity.x *= 0.8; // lose a bit of energy in the bounce
    } else if (location.x < 0) {
      location.x = 0;
      velocity.x *= -1; // full velocity, opposite direction
      velocity.x *= 0.8; // lose a bit of energy in the bounce
    }

    if (location.y > height) {
      location.y = height;
      velocity.y *= -1; // full velocity, opposite direction
      velocity.y *= 0.8; // lose a bit of energy in the bounce
    } else if (location.y < 0) {
      location.y = 0;
      velocity.y *= -1; // full velocity, opposite direction
      velocity.y *= 0.8; // lose a bit of energy in the bounce
    }
  }
}


````



The color is a quality of each vehicle. You can imagine all sorts of other
qualities. Maybe some are rabbits, nothers are foxes; maybe some are outgoing
and others are reclusive. Maybe some have a different maximum speed than
others.
Each vehicle might have a whole set of these
qualities, which taken together might be considered a particular vehicle's
DNA (it's not really the right use of the word but pretend). 

What other qualities can you add to your vehicles, and how should they affect
a vehicle's behavior?

Later we will consider how vehicles might die off or reproduce, and this "DNA"
might have a role to play in the newly-created vehicle.

#### Look at homework!

### February 14

- Let's remember that his course is not merely technical. It is equally 
important
to engage with the philosophical, ethical, and psychological questions
of what robots mean to the human experience. This means the readings,
presentations, and discussions.
- At first I scheduled everyone's presentations for the rest of the semester.
Then, in conversation with other faculty, I felt that it was unfair that some
had a much shorter time than others to prepare. So I removed that list and
only scheduled 2 weeks in advance. However some of you had already read that
first list and proactively planned when to write your presentation. Let's
decide together what is fair and desirable. Options include:
- Stay with it as it is
	- Variation: Allow students to switch with classmates
- Go back to the original list
- Something else. Any suggestions?

##### Agenda for today

- Reading discussion
- Presentation
- Look at homework

### February 16

##### Agenda for today

- Presentation
- Autonomous Agents and The Steering Force (Chapter 6)

##### Autonomous Agents and The Steering Force (Chapter 6)

Our robots are autonomous agents:

- An autonomous agent has a limited ability to perceive environment.
- An autonomous agent processes the information from its environment 
	and calculates an action.
	- In our case, an action is a force
	- More specifically, a *steering* force
	- There may be multiple goals in our environment (find food, avoid danger,
		stay close to friends and hence multiple forces; as before, we will
		combine all the forces 
- An autonomous agent has no leader.

What is the Steering Force?

- A Steering Force is the difference between a desired velocity (run away from
	zombie) and our current velocity (heading towards the zombie). The steering
	force works to change our velocity in the desired direction.
	- steering force = desired velocity - current velocity
	- `PVector steer = PVector.sub(desired,velocity);`
- To be more 'lifelike', our steering forces are limited. An infinite steering
	force would allow us to instantly change direction. 
	Physical objects can't do that; due to their mass (inertia) it takes
	awhile to steer from one direction to another.

- There are many different actions we might describe: Seek, flee, follow a
	path, follow a flow field, flock with your neighbors, etc.  Behavior: What
	sort of action should your robot take in order if it has love, fear, hunger,
	etc.?

###### Seeking: Our First Steering Force

Our robot desires to move towards the target at the maximum speed of which it
is capable. We need to add this to our class:

````
class Vehicle {
  PVector location;
  PVector velocity;
  PVector acceleration;
  float maxspeed; // Maximum speed
````

to apply this to our desired velocity we normalize and multiply:

````
PVector desired = PVector.sub(target,location);
desired.normalize();
desired.mult(maxspeed);
````

All together:

````
  void seek(PVector target) {
    PVector desired = PVector.sub(target,location);
    desired.normalize();
    // Calculating the desired velocity
    // to target at max speed
    desired.mult(maxspeed);

    // Reynolds’s formula for steering force
    PVector steer = PVector.sub(desired,velocity);
    // Using our physics model and applying the force
    // to the object’s acceleration
    applyForce(steer);
  }
````

How can we simulate the effect of the robot's mass on steering?
Is it small and nimble, or heavy and lumbering? 
We limit the steering force:

````
class Vehicle {
  PVector location;
  PVector velocity;
  PVector acceleration;
  float maxspeed; // Maximum speed
	float maxForce; // Maximum force that we can use for steering
````

And applying this limitation gives us:

````
void seek(PVector target) {
	PVector desired = PVector.sub(target,location);
	desired.normalize();
	desired.mult(maxspeed);
	PVector steer = PVector.sub(desired,velocity);

	// Limit the magnitude of the steering force.
	steer.limit(maxforce);

	applyForce(steer);
}
````

Here is a very simple example using the seeking force. Note that I modified
this a little from the book as I wanted to be consistent with what we did
before: the action returns a force which is then applied in `draw()`:

````
class Vehicle {

  PVector location;
  PVector velocity;
  PVector acceleration;
  float r;
  float maxforce;
  float maxspeed;

  Vehicle(float x, float y) {
    acceleration = new PVector(0, 0);
    velocity = new PVector(0, 0);
    location = new PVector(x, y);
    r = 3.0;
    // Arbitrary values for maxspeed and
    // force; try varying these!
    maxspeed = 4;
    maxforce = 0.01;
  }

  // Our standard “Euler integration” motion model
  void update() {
    velocity.add(acceleration);
    velocity.limit(maxspeed);
    location.add(velocity);
    acceleration.mult(0);
  }

  // Newton’s second law; we could divide by mass if we wanted.
  void applyForce(PVector force) {
    acceleration.add(force);
  }

  // Our seek steering force algorithm
  PVector seek(PVector target) {
    PVector desired = PVector.sub(target, location);
    desired.normalize();
    desired.mult(maxspeed);
    PVector steer = PVector.sub(desired, velocity);
    steer.limit(maxforce);

		return (steer);
  }

  void display() {
    // Vehicle is a triangle pointing in
    // the direction of velocity; since it is drawn
    // pointing up, we rotate it an additional 90 degrees.
    float theta = velocity.heading() + PI/2;
    fill(175);
    stroke(0);
    pushMatrix();
    translate(location.x, location.y);
    rotate(theta);
    beginShape();
    vertex(0, -r*2);
    vertex(-r, r*2);
    vertex(r, r*2);
    endShape(CLOSE);
    popMatrix();
  }
}

PVector target = new PVector(0, 0);
void mouseClicked() {
  target.x = mouseX;
  target.y = mouseY;
}

void draw() {
  background(255);

  // Draw the target
  circle(target.x, target.y, 20);

  // Now tell the vehicle to go there
	PVector seek = v.seek(target);
	v.applyForce(seek);
  v.update();
  v.display();
}

Vehicle v;

void setup() {
  size (900, 900);

  v = new Vehicle(width/2, height/2);
}
````

###### Arriving: Our Second Steering Force

We want to slow down when we are close to our target. 
There are many ways to do this; let's say we want to go as
fast as possible if we are further than 100 pixels, 
and when closer than 100 pixels 
we want our speed to be half the distance:

````
PVector arrive(PVector target) {

	// First, calculate our desired velocity vector
	PVector desired = PVector.sub(target,location);

	// The distance is the magnitude of
	// the vector pointing from location to target.
	// Save this for later.
	float d = desired.mag();

	// As before, normalize the desired velocity vector
	desired.normalize();

	// Now apply the "arriving" logic

	// If we are closer than 100 pixels...
	if (d < 100) {

		// ...set the magnitude
		// according to how close we are.
		float m = map(d,0,100,0,maxspeed);
		desired.mult(m);

	// Otherwise, proceed at full speed
	} else {
		desired.mult(maxspeed);
	}

	// The usual steering = desired - velocity
	PVector steer = PVector.sub(desired,velocity);

	// Limit to our maximum ability to steer
	steer.limit(maxforce);

	// and finally return it
	return(steer);
}
````


Below the functions for both the seeking and arriving  behaviors are
integrated into last week's large program. 

**Notes**
- While both `seek()` and `arrive()` are present, only one is
used. Your Vehicle class might contain a multitude of different actions and
use them in different circumstances.
- The `attract()` function is also used, so the vehicles both 
attract each other and also try to arrive at the target.
- All three  behaviors return a force that is then applied in the `draw()`
function. The order doesn't matter because the forces are added 
and vector addition is *commutative*
- The `Mover` class is now renamed to `Vehicle`.
- The `Attractor` class is modified to follow the mouse
- I have made the repulsive force quite strong so every so often a vehicle
	is rejected from the attractor

````
// Vehicles and a single Attractor
Vehicle[] vehicle = new Vehicle[10];
Attractor a;

void setup() {
  // I wanted to see what it looked like if it filled my screen (almost)
  size(1800, 1000);

  for (int i = 0; i < vehicle.length; i++) {
    // Each Vehicle is initialized randomly.
    vehicle[i] = new Vehicle(random(0.1, 2), // mass
      random(width), random(height)); // initial location
  }

  a = new Attractor();
}

void draw() {
  background(255);

  // Now we have to update the attractor so that it follows the mouse
  a.update();
  a.display();

  // For each vehicle
  for (int i = 0; i < vehicle.length; i++) {

    // Calculate the attraction force from the Attractor on this vehicle
    // PVector attractForce = a.attract(vehicle[i]);

    // Calculate the arriving force
    PVector arriveForce = vehicle[i].arrive(a.location);

    // Apply the force
    vehicle[i].applyForce(arriveForce);

    // Now apply the force from all the other vehicle on this vehicle
    for (int j = 0; j < vehicle.length; j++) {
      // Don’t attract yourself!
      if (i != j) {
        PVector force = vehicle[j].attract(vehicle[i]);
        vehicle[i].applyForce(force);
      }
    }
    vehicle[i].update();
    vehicle[i].checkEdges();
    vehicle[i].display();
  }
}


class Attractor {
  float mass;
  PVector location;
  float G;

  Attractor() {
    // Will be updated to the mouse location
    location = new PVector(0,0);

    // Big mass so the force is greater than vehicle-vehicle force
    // You can play with this number to see different results
    mass = 20;
    G = 0.4;
  }

  // New function to have the Attractor follow the mouse
  void update() {
    location.x = mouseX;
    location.y = mouseY;
  }

  PVector attract(Vehicle m) {
    PVector force = PVector.sub(location, m.location);
    float distance = force.mag();
    // Remember, we need to constrain the distance
    // so that our vehicle doesn’t spin out of control.
    distance = constrain(distance, 5.0, 25.0);

    force.normalize();
    float strength = (G * mass * m.mass) / (distance * distance);
    force.mult(strength);
    return force;
  }

  void display() {
    stroke(0);
    fill(175, 200);
    ellipse(location.x, location.y, mass*2, mass*2);
  }
}


// Vehicle class from last week with the addition of the
// seek() and arrive() functions
class Vehicle {

  PVector location;
  PVector velocity;
  PVector acceleration;
  float mass;
  float G = 0.4;
  int myColor;
  float maxforce;
  float maxspeed;

  Vehicle(float _mass_, float _x_, float _y_) {
    mass = _mass_;
    location = new PVector(_x_, _y_);
    velocity = new PVector(0, 0);
    acceleration = new PVector(0, 0);
    myColor = round(random(1));
    maxspeed = 4;
    maxforce = 0.1;
  }

  // Newton’s second law.
  void applyForce(PVector force) {
    // Receive a force, divide by mass, and add to acceleration.
    PVector f = PVector.div(force, mass);
    acceleration.add(f);
  }

  // The Vehicle now knows how to attract another Vehicle.
  PVector attract(Vehicle m) {

    PVector force = PVector.sub(location, m.location);
    float distance = force.mag();
    distance = constrain(distance, 5.0, 25.0);
    force.normalize();

    float strength = (G * mass * m.mass) / (distance * distance);
    force.mult(strength);

    // If the color is different then we will be repelled strongly
    if (myColor != m.myColor) force.mult(-15);

    return force;
  }


  void update() {
    velocity.add(acceleration);
    velocity.limit(maxspeed);
    location.add(velocity);
    acceleration.mult(0);
  }

  void display() {
    stroke(0);
    if (myColor == 1) fill(255, 0, 0);
    else fill (0, 0, 255);

    // Rotate the vehicle to point in the direction of travel
    // Translate to the center of the move
    pushMatrix();
    translate(location.x, location.y);
    rotate(velocity.heading());
    // It took lots of trial and error
    // and sketching on paper
    // to get the triangle
    // pointing in the right direction
    triangle(0, 5, 0, -5, 20, 0);
    popMatrix();
  }

  // With this code an object bounces when it hits the edges of a window.
  // Alternatively objects could vanish or reappear on the other side
  // or reappear at a random location or other ideas. Also instead of
  // bouncing at full-speed vehicles might lose speed. So many options!

  void checkEdges() {
    if (location.x > width) {
      location.x = width;
      velocity.x *= -1; // full velocity, opposite direction
      velocity.x *= 0.8; // lose a bit of energy in the bounce
    } else if (location.x < 0) {
      location.x = 0;
      velocity.x *= -1; // full velocity, opposite direction
      velocity.x *= 0.8; // lose a bit of energy in the bounce
    }

    if (location.y > height) {
      location.y = height;
      velocity.y *= -1; // full velocity, opposite direction
      velocity.y *= 0.8; // lose a bit of energy in the bounce
    } else if (location.y < 0) {
      location.y = 0;
      velocity.y *= -1; // full velocity, opposite direction
      velocity.y *= 0.8; // lose a bit of energy in the bounce
    }
  }

  void seek(PVector target) {
    PVector desired = PVector.sub(target, location);
    desired.normalize();
    desired.mult(maxspeed);
    PVector steer = PVector.sub(desired, velocity);
    steer.limit(maxforce);
    applyForce(steer);
  }

  PVector arrive(PVector target) {
    PVector desired = PVector.sub(target, location);

    // The distance is the magnitude of the vector pointing
    // from location to target.
    float d = desired.mag();
    desired.normalize();

    // If we are closer than 100 pixels...
    if (d < 100) {
      //[ ...set the magnitude
      // according to how close we are.
      float m = map(d, 0, 100, 0, maxspeed);
      desired.mult(m);
    } else {
      // Otherwise, proceed at maximum speed.
      desired.mult(maxspeed);
    }

    // The usual steering = desired - velocity
    PVector steer = PVector.sub(desired, velocity);
    steer.limit(maxforce);
    return(steer);
  }
}
````

We may want to add that if we are within a smaller radius for more than a
certain amount of time we should stop. How do we do that?
- How do we know that a certain amount of time has passed?
- How do we stop?

### February 21
##### todays-lecture

#### Admin
- Record
- Zoom in to make text larger

##### Agenda for today

1. Presentation
1. Discussion
1. Perlin noise
1. Flow Fields

##### A little detour to Perlin noise

````
void draw() {
  background(204);
  float n = random(0, width);
  line(n, 0, n, height);
}
````

What if we wanted the line to move in a more organic, lifelike
fashion? Organic things (e.g. butterflys, leaves blowing in the wind, clouds) 
don't jump instantly from one place to another,
they tend to move close to where they were last time

````
float offset = 0.0;

void draw() {
  background(204);
  offset = offset + .01;
  float n = noise(offset) * width;
  line(n, 0, n, height);
}
````

##### Flow fields

A field of vectors which we might use in some way 
- the scent of food 
- slipperiness of the floor
- slope of hills
- aggressiveness of zombies
- difficulty of classes that you might want to avoid
- can you suggest other ideas?

First we need an array of PVectors to store our field:

````
FlowField() {
	pixelsPerSquare = 10;
	// Total columns equals width divided by pixelsPerSquare.
	cols = width/pixelsPerSquare;
	// Total rows equals height divided by pixelsPerSquare.
	rows = height/pixelsPerSquare;
	field = new PVector[cols][rows];
}
````

Now to create the force field, here are a couple of examples:

````
// A random force field
for (int i = 0; i < cols; i++) {
  for (int j = 0; j < rows; j++) {
    field[i][j] = PVector.2D(); // generate a random PVector, unit length
  }
}
````

````
// A random field using Perlin noise, 
// where the Perlin value is mapped to an angle
// so it changes in a natural way
float xoff = 0;
for (int i = 0; i < cols; i++) {
  float yoff = 0;
  for (int j = 0; j < rows; j++) {

    // Get the next value of Perlin noise 
		// and map it to an angle 
    float theta = map(noise(xoff,yoff), // generate a 2D Perlin value
			0,1,  // Noise is always between 0 and 1
			0,TWO_PI); // a full circle

		// The cosine of the angle is our x value
		// and the sine of the angle is our y value
    field[i][j] = new PVector(cos(theta), sin(theta));

    yoff += 0.1; // next y value
  }
  xoff += 0.1; // next x value
}
````

Now the vehicle needs to know where it is in this force field:

````
int column = int(location.x/pixelsPerSquare);
int row = int(location.y/pixelsPerSquare);
````

and our force field class should provide a method which return
the force vector at that that location:

````
 PVector lookup(PVector lookup) {

	// Using constrain()
	int column = int(constrain(lookup.x/pixelsPerSquare, 0, cols-1));
	int row = int(constrain(lookup.y/pixelsPerSquare, 0, rows-1));

	// Note the use of copy() to ensure
	// we return a copy of the PVector.
	return field[column][row].copy();
}
````

Next, let's make a vehicle follow it. How would we do that?

````
/*
Create a flow field and then have a vehicle follow it

 Michael Shiloh
 February 10 2021

 Based on examples from The Nature of Code by Daniel Shiffman

 This code is in the public domain
 */

// The flow field class, more or less straight from the book
// with the addition of the display() function and different
// initialization options (each of which is from the book)
class FlowField {

  PVector[][] field;
  int cols, rows;
  int pixelsPerSquare; // Size of each square in the grid, in pixels

  // Constructor takes the desired pixelsPerSquare
  FlowField(int _res) {
    pixelsPerSquare = _res;
    cols = width/pixelsPerSquare;
    rows = height/pixelsPerSquare;

    // Declare the array of PVectors which will hold the field
    field = new PVector[cols][rows];

    // Initialize the field using one of the three options below
    // or make up your own initialization function
    // uniformFlowField();
    // randomFlowField();
    perlinFlowField();
  }

  // Pretty boring; all vectors point to the right
  void uniformFlowField() {
    for (int i = 0; i < cols; i++) {
      for (int j = 0; j < rows; j++) {
        field[i][j] = new PVector(1, 0); // pointing to the right
      }
    }
  }

  void randomFlowField() {
    for (int i = 0; i < cols; i++) {
      for (int j = 0; j < rows; j++) {
        field[i][j] = PVector.random2D();
      }
    }
  }

  // Use perlin noise to determine the angle of each vector
  void perlinFlowField() {

    float xoff = 0;
    for (int i = 0; i < cols; i++) {
      float yoff = 0;
      for (int j = 0; j < rows; j++) {

        // Moving through the noise() space in two dimensions
        // and mapping the result to an angle between 0 and 360
        float theta = map(noise(xoff, yoff), 0, 1, 0, TWO_PI);

        // Convert the angle (polar coordinate) to Cartesian coordinates
        field[i][j] = new PVector(cos(theta), sin(theta));

        // Move to neighboring noise in Y axis
        yoff += 0.1;
      }

      // Move to neighboring noise in X axis
      xoff += 0.1;
    }
  }

  // Given a PVector which defines a location in the flow field,
  // return a copy of the value of the flow field at that location
  PVector lookup(PVector lookup) {

    // Convert x and y values to row and column, and constrain
    // to stay within the field
    int column = int(constrain(lookup.x/pixelsPerSquare, 0, cols-1));
    int row = int(constrain(lookup.y/pixelsPerSquare, 0, rows-1));

    return field[column][row].copy();
  }

  // Display the flow field so we can see if it looks 
	// like what we think it should
  //
  void display() {
    for (int i = 0; i < cols; i++) {
      for (int j = 0; j < rows; j++) {

				// I needed this to see that my equations generated
				// the right results
        // print("col " + i + " row " + j + "  ");
        // println(i*pixelsPerSquare, j*pixelsPerSquare, 
					// field[i][j].x, field[i][j].y);

        pushMatrix();

        // This translates to the top left corner of the grid, but really
        // it should center the vector in the middle of the grid
        translate(i*pixelsPerSquare, j*pixelsPerSquare);

        PVector f = field[i][j].copy();

				// Make the arrow as big as a square
        f.mult(pixelsPerSquare);

				// Draw a line
        line(0, 0, f.x, f.y);

				// Put a little dot at the end
        ellipse(f.x, f.y, 5, 5); // circle instead of arrow head

        popMatrix();
      }
    }
  }
}

// The vehicle class, more or less straight from the book
class Vehicle {

  PVector location;
  PVector velocity;
  PVector acceleration;
  float r;
  float maxforce;
  float maxspeed;

  Vehicle(float x, float y) {
    acceleration = new PVector(0, 0);
    velocity = new PVector(0, 0);
    location = new PVector(x, y);
    r = 3.0;
    maxspeed = 4;
    maxforce = 10;
  }

  // Update the velocity and location, 
	// based on the acceleration generated by the steering force
  void update() {
    velocity.add(acceleration);
    velocity.limit(maxspeed);
    location.add(velocity);
    acceleration.mult(0); // clear the acceleration for the next frame
  }

  // Newton’s second law; we could divide by mass if we wanted.
  // If there are multiple forces (e.g. gravity, wind) we use
  // this function for each one, and it is added to the acceleration
  void applyForce(PVector force) {
    acceleration.add(force);
  }

  /*
  What follows are different steering algorithms. A vehicle
   could use any one, and you could create addiotional ones.
   Each algorithm calculates the steering force and then
   applies it
   */

  // Calculate steering force to seek a target
  void seek(PVector target) {
    PVector desired = PVector.sub(target, location);
    desired.normalize();
    desired.mult(maxspeed);
    PVector steer = PVector.sub(desired, velocity);
    steer.limit(maxforce);
    applyForce(steer);
  }

  // Calculate the steering force to follow a flow field
  void follow(FlowField flow) {
    // Look up the vector at that spot in the flow field
    PVector desired = flow.lookup(location);
    desired.mult(maxspeed);

    // Steering is desired minus velocity
    PVector steer = PVector.sub(desired, velocity);
    steer.limit(maxforce);
    applyForce(steer);
  }

  void display() {
    // Vehicle is a triangle pointing in
    // the direction of velocity; since it is drawn
    // pointing up, we rotate it an additional 90 degrees.
    float theta = velocity.heading() + PI/2;
    fill(175);
    stroke(0);
    pushMatrix();
    translate(location.x, location.y);
    rotate(theta);
    beginShape();
    vertex(0, -r*2);
    vertex(-r, r*2);
    vertex(r, r*2);
    endShape(CLOSE);
    popMatrix();
  }
}

/*
Finally we can use these classes to make a vehicle and a flow field
 and watch the vehicle follow the flow field
 */

FlowField f;
Vehicle v;

void setup() {
  size (1200, 800);
  f = new FlowField(15);
  f.display(); // display the flow field

  // put the vehicle in the middle
  v = new Vehicle(width/2, height/2);
}


void draw() {

  v.follow(f); // Apply the steering force to follow the flow field

	// Update the velocity and location, based on the acceleration 
	// generated by the steering force
  v.update(); 

  v.display(); // display the vehicle
}
````
##### Field follows mouse

How would we make a flow field that follows the mouse?

````




















````

````
void fieldFollowsMouse() {
	for (int i = 0; i < cols; i++) {
		for (int j = 0; j < rows; j++) {
			// PVector of mouse location
			PVector mouseAt = new PVector(mouseX, mouseY);
			// PVector of current location
			PVector weAt = new PVector(i*pixelsPerSquare, j*pixelsPerSquare);
			// PVector from our current position to mouse
			PVector toMouse = PVector.sub( mouseAt, weAt);
			// Normalize
			toMouse.normalize();
			field[i][j] = toMouse;
		}
	}
}
````

````
void mouseClicked() {
  f = new FlowField(15);
  background(200);
  f.display(); // display the flow field
}
````

##### Adding vehicles during runtime

An `array` is a fixed size, but an `ArrayList` can grow and shrink:

````
// ArrayLists have a special syntax:
ArrayList<PVector> myVectors = new ArrayList<PVector>();

void setup() {

  // The ArrayList should be empty
  println(myVectors.size());

  for (int i = 0; i < 10; i++) {
    myVectors.add(new PVector(i, 0));
    println(myVectors.size()); // not that size() is a function!
  }

  // The value at index 5 should be 5
  // Just like arrays, the index starts at zero
  // so index 5 is the sixth item
  println("at index 5, x = " + myVectors.copy(5).x);
}

void mouseClicked() {
  myVectors.add(new PVector(mouseX, mouseY));
  println("added a new PVector at this mouse location");
}

void keyPressed() {
  if (key == 'd') {
    myVectors.remove(0); // remove the first vector
    println("removed the first PVector, size is now " + myVectors.size());
  }

  if (key == 'p') {
    for (int i = 0; i < myVectors.size(); i++) {
      PVector v = myVectors.copy(i);
      println("index = " + i + " x = " + v.x + " y = " + v.y);
    }

  // Another way to loop through the ArrayList:
  if (key == 'P') {
    for (Vehicle v : vehicles) { 
      PVector v = myVectors.copy(i);
      println("index = " + i + " x = " + v.x + " y = " + v.y);
    }
}

// Need to have a draw() function so that callbacks occur
// even though it is empty
void draw() {
}
````

Now we can use an `ArrayList` to store vehicles, and keep adding them
whenever we click the mouse: 

````
// Prepare an ArrayList of Vehicles
ArrayList<Vehicle> vehicles = new ArrayList<Vehicle>();

// In the constructor for example, place one vehicle in the center
vehicles.add(new Vehicle(width, height/2));

// And then 
void mouseClicked() {
	vehicles.add(new Vehicle(mouseX, mouseY));
}
````

