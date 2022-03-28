
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
  // Note that this get() function is part of the ArrayList
  // and not the PVector get()
  println("at index 5, x = " + myVectors.get(5).x);
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
      // Note that this get() function is part of the ArrayList
      // and not the PVector get()
      PVector v = myVectors.get(i);
      println("index = " + i + " x = " + v.x + " y = " + v.y);
    }
  }

  // Another way to loop through an ArrayList:
  if (key == 'P') {
    for (PVector v : myVectors) {
      println("For this PVector x = " + v.x + " y = " + v.y);
    }
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

// In the constructor for example, we might place one vehicle in the center
vehicles.add(new Vehicle(width, height/2));

// And then 
void mouseClicked() {
  vehicles.add(new Vehicle(mouseX, mouseY));
}
````

### February 23

##### Agenda for today
- Presentation
- Complexity
- Flocking

##### complexity

A complex system is typically defined as a system that is “more than the sum
of its parts.” While the individual elements of the system may be incredibly
simple and easily understood, the behavior of the system as a whole can be
highly complex, intelligent, and difficult to predict. Here are three key
principles of complex systems:

- Simple units with short-range relationships. This is what we’ve been
  building all along: vehicles that have a limited perception of their
  environment.
- Simple units operate in parallel. This is what we need to simulate in code.
  For every cycle through Processing’s draw() loop, each unit will decide how
  to move (to create the appearance of them all working in parallel).
- System as a whole exhibits emergent phenomena. Out of the interactions
  between these simple units emerges complex behavior, patterns, and
  intelligence (we hope).

Following are three additional features of complex systems that will help
frame the discussion, as well as provide guidelines for features we will want
to include in our software simulations. It’s important to acknowledge that
this is a fuzzy set of characteristics and not all complex systems have all of
them.

- Non-linearity. This aspect of complex systems is often casually referred
    to as “the butterfly effect,” the theory is that a single butterfly
    flapping its wings on the other side of the world could cause a massive
    weather shift and ruin our weekend at the beach. We call it “non-linear”
    because there isn’t a linear relationship between a change in initial
    conditions and a change in outcome. A small change in initial conditions
    can have a massive effect on the outcome. Non-linear systems are a
    superset of chaotic systems. 

-Competition and cooperation. One of the things that often makes a complex
system tick is the presence of both competition and cooperation between the
elements. In our system, we will have:
  - alignment (cooperation)
  - cohesion (cooperation)
  - separation (competition) Competition and cooperation are found in living
    complex systems, but not in non-living complex systems like the weather.

- Feedback. Complex systems often include a feedback loop where the 
    output of the system is fed back into the system to influence its behavior
    in a positive or negative direction. 

Let's have vehicles like each other but not want to get too close:

````
    v.separate(vehicles);
````

Once again, each vehicle needs to see where everyone else is.

what does the `separate()` function do?
- If a given vehicle is too close to you, steer away from that vehicle. How do
  we do this?
  - We already know how to steer towards a target. Reverse that force and we
    have the 'separate' behavior.  
- If more than one vehicle is too close, we’ll define separation as the
  average of all the vectors pointing away from any close vehicles.

Let's get to it:

We know that `separate()` gets the entire ArrayList:

````
void separate (ArrayList<Vehicle> vehicles) {
````

Now check separation:

````
  float desiredseparation = 20; // how close is too close.

  for (Vehicle other : vehicles) {

    // What is the distance between me and another Vehicle?
    float d = PVector.dist(location, other.location);

    if ((d > 0) && (d < desiredseparation)) {
      // Here we avoid any vehicle that is too close
    }
  }
````

Why did we also check that `d > 0`?

This part calculates the way to go to avoid a vehicle that is too close:

````
  if ((d > 0) && (d < desiredseparation)) {
    // A PVector pointing away from the other’s location
    PVector diff = PVector.sub(location, other.location);
    diff.normalize();
  }
````

What if more than one is close? How do we take the average?

````
PVector sum = new PVector();  // Start with an empty PVector.
int count = 0;  // We have to keep track of how many Vehicles are too close.

for (Vehicle other : vehicles) {
float d = PVector.dist(location, other.location);
  if ((d > 0) && (d < desiredseparation)) {
    PVector diff = PVector.sub(location, other.location); 
    diff.normalize();
    // Add all the vectors together and increment the count.
    sum.add(diff); 
    count++;
  }
}

// now calculate the average:
if (count > 0) { // mustn't divide by zero
  sum.div(count); 
} 

// sum now contains the average vector away from neighbors
````

How do we apply this? It's the same thing as before; this vector represents
a *desire* to avoid neighbors, so we treat it as a **force**.

````
if (count > 0) {
  sum.div(count);

  // Scale average to maxspeed
  // (this becomes desired).
  sum.setMag(maxspeed);

  // Apply Reynolds’s steering formula:
  // error is our current velocty minus our desired velocity
  PVector steer = PVector.sub(sum,vel);
  steer.limit(maxforce);

  return(steer); // let `draw()` apply the force
}
````

[Here](https://github.com/michaelshiloh/resourcesForClasses/blob/master/src/processingSketches/robotaPsyche/separate/separate.pde)
is a program putting this all together

##### Reorganize the code again to be consistent:

Vehicle
- Our `Vehicle` class may have a lot of different behaviors
  - Seek (target) 
  - Follow (flowfield)
  - Separate (ArrayList of all objects)
- all behaviours do this:
  - Calculate the force
  - Return the force but do not apply it
- The `Vehicle update()` function does:
  - apply acceleration to velocity
  - apply velocity to location
  - clear the acceleration for the next frame
- `draw()` does:
  - calls the the desired `vehicle` behavior(s) which returns a force
  - apply the force returned by the behavior
  - As before, `update()`, `checkEdges()` (or maybe not?), and `display()`


````
void applyBehaviors(ArrayList<Vehicle> vehicles) {
  // seek() and separate() do not apply the force
  PVector separate = separate(vehicles);
  PVector seek = seek(new PVector(mouseX,mouseY));
  applyForce(separate);
  applyForce(seek);
}
````

so how do `seek()` and `separate()` change?

````
// note that now this function returns a PVector which is the force
PVector seek(PVector target) {
  PVector desired = PVector.sub(target,loc);
  desired.normalize();
  desired.mult(maxspeed);
  PVector steer = PVector.sub(desired,vel);
  steer.limit(maxforce);

  // Instead of applying the force we return the PVector.
  // applyForce(steer); // No longer done here
  return steer;
}
````

So now we can do this:

````
void applyBehaviors(ArrayList<Vehicle> vehicles) {
  // Apply the behaviours to get the component forces
  PVector separate = separate(vehicles);
  PVector seek = seek(new PVector(mouseX,mouseY));
  // Possibly other behaviours

  // These values can be whatever you want them to be!
  // They can be variables that are customized for
  // each vehicle, or they can change over time, over location,
  // or whatever you can imagine (and code).
  separate.mult(1.5); 
  seek.mult(0.5); 

  applyForce(separate);
  applyForce(seek);
}
````

In-class exercise: Modify this function so that the behaviour weights are not
constants. What happens if they change over time (according to a sine wave or
Perlin noise)? Or if some vehicles are more concerned with seeking and others
more concerned with separating? Can you introduce other steering behaviours as
well?

##### Flocking

Now we'll take another step towards autonomy. Can we describe individual 
behaviours which will cause a group?

The three rules of flocking.

- Separation (also known as “avoidance”): Steer to avoid colliding with your
  neighbors.
- Alignment (also known as “copy”): Steer in the same direction as your
  neighbors.
- Cohesion (also known as “center”): Steer towards the center of your
  neighbors (stay with the group).

````
void flock(ArrayList<Boid> boids) {
  // The three flocking rules
  PVector sep = separate(boids);
  PVector ali = align(boids);
  PVector coh = cohesion(boids);

  // Arbitrary weights for these forces
  // (Try different ones!)
  sep.mult(1.5);
  ali.mult(1.0);
  coh.mult(1.0);

  // Applying all the forces
  applyForce(sep);
  applyForce(ali);
  applyForce(coh);
}
````

Why *boids*?

Separation we did.

Alignment is steering in the same direction as your neighbors:
the boid’s desired velocity is the average velocity of its neighbors.

````
PVector align (ArrayList<Boid> boids) {
  // Add up all the velocities
  // and divide by the total
  // to calculate the average velocity.
  PVector sum = new PVector(0,0); // Initialize to zero
  for (Boid other : boids) {
    sum.add(other.velocity);
  }
  sum.div(boids.size());

  // We desire to go in that
  // direction at maximum speed.
  sum.setMag(maxspeed); // sum is our desired velocity

  // Reynolds’s steering
  // force formula
  PVector steer = PVector.sub(sum,velocity);
  steer.limit(maxforce);
  return steer;
}
````

But this differs from how real insects move. Can you spot how?

````
PVector align (ArrayList<Boid> boids) {
  // This is an arbitrary value and could
  // vary from boid to boid.
  float neighbordist = 50;
  PVector sum = new PVector(0,0);
  int count = 0;
  for (Boid other : boids) {
    float d = PVector.dist(location,other.location);
    if ((d > 0) && (d < neighbordist)) {
      sum.add(other.velocity);
      // For an average, we need to keep track of
      // how many boids are within the distance.
      count++;
    }
  }
  if (count > 0) {
    sum.div(count);
    sum.normalize();
    sum.mult(maxspeed);
    PVector steer = PVector.sub(sum,velocity);
    steer.limit(maxforce);
    return steer;
  // If we don’t find any close boids,
  // the steering force is zero.
  } else {
    return new PVector(0,0);
  }
}
````

This is is purely by distance and could be different, e.g. only boids
that are in front of us (as if we had eyes) or some other rule

Now what about cohesion. What does that mean exactly?
How would you implement it?

````
PVector cohesion (ArrayList<Boid> boids) {
  float neighbordist = 50;
  PVector sum = new PVector(0,0);
  int count = 0;
  for (Boid other : boids) {
    float d = PVector.dist(location,other.location);
    if ((d > 0) && (d < neighbordist)) {
      // Adding up all the others’ locations
      sum.add(other.location);
      count++;
    }
  }
  if (count > 0) {
    sum.div(count);
    // Here we make use of the seek() function we
    // wrote in Example 6.8.  The target
    // we seek is the average location of
    // our neighbors.
    return seek(sum); 
  } else {
    return new PVector(0,0);
  }
}
````

Now that we have a whole colony of beings, it might be worth writing a class
to collect them:

````
class Flock {
  ArrayList<Vehicle> vehicles;

  Flock() {
    vehicles = new ArrayList<Vehicle>();
  }

  void run() {
    for (Vehicle b : vehicles) {
      // Each Boid object must know about
      // all the other Boids.
      b.run(boids); 
    }
  }

  void addVehicle(Vehicle v) {
    vehicles.add(v);
  }
}
````

### February 28
##### Agenda for today

- Presentation (Adina, Abdul)
- Discussion
- Homework feedback
- Review boids
	- Note that you could have multiple flocks with different properties
- Debugging
- Look at homework assignment
- Jack and Ume visit

##### Homework feedback

Some of you received less than the full 10 points on the work you have done
so far. Generally this was due to one or more of:
- Insufficient complexity
- Too much complexity, and results appear random and chaotic
- Insufficient evidence that the ecology is exhibiting the behavior you
	were trying to produce. See if you can figure out why, and make it more
	obvious
- Poor visuals, making it difficult to understand who are the players and
	what they are trying to do. While you don't need to use pictures of birds
	(although you can if you want), at least make them different from
	triangles of the same size. Color is one step, but make it more obvious
	e.g. different shapes, different sizes.

##### Debugging

Add the ability to change frame rate, single step, stop, and inspect:

- [association.pde](https://github.com/michaelshiloh/resourcesForClasses/blob/master/src/processingSketches/robotaPsyche/association/association.pde)

### March 2

##### Agenda for today
- Presentation: Chinonyerem
- Course feedback 
- Look at more homework
- Review boids
- Work on your midterm projects

#### Course feedback 

- What was the more surprising thing you learned so far in this class?
- What was the more important thing you learned so far in this class?
- What would you like more of?
- What would you like less of?
- What do you wish we'd talk about, or learn, that we haven't?

#### Boids

- Review boids (if necessary)
- Note that you could have multiple flocks with different properties

### March 2

##### Agenda for today

- Midterm project status

### March 23
##### Agenda for today

- Rescheduled readings
- Plan for final project
- Examples

##### Rescheduled readings

##### Plan for final project

- One ecosystem that will have all your creatures in it
- This is an experiment so we will all learn together and figure out how to make this work. What this means is if you run into problems let me know!
- How will it work
	- Wrap your entire ecosystem in a class that will in contain everything in
		your ecosystem
	- Your class should have these functions:
		- `setup()`
		- `draw()`
		- `mouseClicked()` (even if empty)
		- `keyPressed()` (even if empty)
		- Others as necessary
	- Look at [example](https://github.com/michaelshiloh/resourcesForClasses/tree/master/src/processingSketches/robotaPsyche/allTogether)
	- Your class must not call these functions:
		- `size()`
		- `background()`
	- Your environment must not have any anything that is outside of the class
- How will we integrate them?
	- New Github repository for the class project
		- All of you will be contributors
		- Once you get your enviromnent working, copy it to the 
			class project repository and test it. If it works, you may commit it.
			If it doesn't work, you have to fix it first.
		- As each of you add your enviroment, the class project
			will get more and more complex but should always work

##### Examples

Examples of synthetic ecologies

- Heather's projects:
	- [One](https://deweyhagborg.com/projects/netlingua)
	- [Another](https://deweyhagborg.com/_archived/bugs/)
- The book "metacreation: art and artificial life" by Mitchell Whitelaw has some good examples along these lines
- Ken Rinaldo
- Stephen Wilson's information arts, he had a section on this topic I believe.
- VIDA art & artificial life competition always had things along these lines as well.
- Anicka Yi's installation in the Tate Modern

### March 28
##### todays-lecture

#### Admin
- **RECORD**
- Zoom in to make text larger
- Please put your presentations in your Github repositories

##### Agenda for today

- Discussion of *Reason*, Phillip and Ben to lead
- Presentatoin by Hassan
