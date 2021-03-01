Shortcut to [today's lecture](lectureNotes.md/#todays-lecture)    
Class [Zoom](https://nyu.zoom.us/j/97015960666)  

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
Session: Spring 2021     

This document: Lecture Notes

**This is subject to change**

### Week 1

### January 18

#### Introduction

- Who am I?
- [Syllabus](syllabus.md)
- Summary:
  - Be present and participate in class
  - No electronic distractions (**No notifications**)
  - Be proactive: 
		Communicate with me regarding difficulties, problems, illness, etc.
  - Allow lots of time for homework 
		as debugging programs can be very time consuming.
- All class material will be on this Github repository 
	with the exception of some reading material which will be 
	accessible through our NYU Classes portal.
- [weekly schedule](weeklySchedule.md)
- this class is a work in progress; your input is crucial
- Who are you?
	- What attracted you to this class?
- What is this class about?
	- What is synthetic psychology?

#### Discussion

If you were building a physical robot to move towards light, what
are some simple approaches you might take?

What are some hardware components you might use?

(There are probably many ways to do this but I want one particular approach)

How might you do this in Processing?
- What features in Processing might be helpful?
	- [Classes](https://github.com/michaelshiloh/simpleProcessingClassExample)

#### Look at homework 

### January 20

#### Administration

#### When were robots first imagined?

https://www.youtube.com/watch?v=SNg4KZKG96o

#### Some more review

- [PVectors](https://processing.org/tutorials/pvector/)
- [Transformations](https://processing.org/tutorials/transform2d/)

### Week 2
### January 25

#### PVectors

- Concept was introduced last week, now we'll get into PVectors seriously
- Why do we care about PVectors?
	- Useful for describing motion, and we'll be doing lots of that
	- Especially when the motion is subject to many competing forces
		(e.g. how bright is the light, how close the vehicle is to the light,
		whether the ground is slippery or not)
- All of this material is in
	[this](https://natureofcode.com/book/chapter-1-vectors/) book
- Vectors 
	- A vector describes how to get from one point to another
		- From your bedroom to the kitchen
		- From campus to Yas Mall
		- From the origin (of the Processing canvas) to the mouse (from location 0,0
			to location mouseX, mouseY)
		- From one point on the Processing canvas to another (e.g. 3,5 to 82,9)
		- The location of the ball in the bouncing ball example (xLocation,
			yLocation)
	- Vectors do not need to be about space
		- The velocity of the ball (xVelocity, yVelocity)
			- (The velocity tells us how to get from the current location to the next
			location)	
		- If we were to add acceleration, that too would be a vector!
			- How to get from our current velocity to the next velocity)
	- Formally, a vector has both magnitude and direction
		- Magnitude tells you how much (e.g. 3 km/hr or 5 pixels/frame)
		- Direction tells you in which direction (e.g. 3km/hr slower or 5
			pixels/frame at a 39 degree angle)
		- wait, isn't a vector just a pair of coordinates? Yes! This specifies a
			magnitued and a direction!
	- ````PVector```` is a Processing class that stores this information and makes
		manipulating it easy
		- Basically, a PVector stores the two numbers that make up the vector e.g.
			xLocation and yLocation.
	- Vector (and ````PVector````) Math
		- A typical vector manipulation would be to add two vectors. This is exactly
					what we do when we add the velocity to the position to obtain the new
					position

````
	float xLocation, xSpeed, yLocation, ySpeed;

	xLocation = xLocation + xSpeed;
	yLocation = yLocation + ySpeed;
````

	is the same as

````
	PVector location, speed;

	location.add(speed);
````

Bouncing ball with PVectors:

````
//[full] Instead of a bunch of floats, we now just have two PVector variables.
PVector location; //[bold]
PVector velocity; //[bold]
//[end]

void setup() {
  size(640,360);
  location = new PVector(100,100); //[bold]
  velocity = new PVector(2.5,5); //[bold]
}

void draw() {
  background(255);

  location.add(velocity); //[bold]
  //[full] We still sometimes need to refer to the individual components of a PVector and can do so using the dot syntax: location.x, velocity.y, etc.
  if ((location.x > width) || (location.x < 0)) { //[bold]
    velocity.x = velocity.x * -1; //[bold]
  } //[bold]
  if ((location.y > height) || (location.y < 0)) { //[bold]
    velocity.y = velocity.y * -1; //[bold]
  } //[bold]
  //[end]

  stroke(0);
  fill(175);
  ellipse(location.x,location.y,16,16); //[bold]
}
````

Note the dot syntax to access variables that are within the objects. Why do we
need that?

- Other vector manipulations include:
	- Subtraction (the difference between two vectors)
	- Multiplication by a number (scaling)
	- Magnitude (what is the length of a vector without regard to direction)
	- Heading (direction without regard to length)
	- Normalize (keeping the same direction, scale to length of 1)

- Example of subtraction:

````
void setup() {
  size(640,360);
}

void draw() {
  background(255);
  //[full] Two PVectors, one for the mouse location and one for the center of the window
  PVector mouse  = new PVector(mouseX,mouseY);
  PVector center = new PVector(width/2,height/2);
  //[end]
  // PVector subtraction!
  mouse.sub(center);
    // Draw a line to represent the vector.
  translate(width/2,height/2);
  line(0,0,mouse.x,mouse.y);
}
````

(Look at other examples in book if interested)

Let's go back to our bouncing ball and put it in a class:

````
// Declare Mover object.
Mover mover;

void setup() {
  size(640,360);
  // Create Mover object.
  mover = new Mover();
}

void draw() {
  background(255);

  //[full] Call functions on Mover object.
  mover.update();
  mover.checkEdges();
  mover.display();
  //[end]
}

class Mover {

  //[full] Our object has two PVectors: location and velocity.
  PVector location;
  PVector velocity;
  //[end]

  Mover() {
    location = new PVector(random(width),random(height));
    velocity = new PVector(random(-2,2),random(-2,2));
  }

  void update() {
    // Motion 101: Location changes by velocity.
    location.add(velocity);
  }

  void display() {
    stroke(0);
    fill(175);
    ellipse(location.x,location.y,16,16);
  }

  void checkEdges() {
    if (location.x > width) {
      location.x = 0;
    } else if (location.x < 0) {
      location.x = width;
    }

    if (location.y > height) {
      location.y = 0;
    } else if (location.y < 0) {
      location.y = height;
    }
  }
}
````

- A little detour: Static vs. Non-Static Functions
	- Note that some of the ````PVector```` functions modify the vector.
		Sometimes we need to e.g. add two vectors without modifying either. For
		that we have static versions of 
		`PVector.add()`
		`PVector.sub()`
		`PVector.mult()`
		`PVector.div()`

- Now let's add acceleration
	- Fixed acceleration

````
class Mover {

  PVector location;
  PVector velocity;
  // Acceleration is the key!
  PVector acceleration;
  // The variable topspeed will limit the magnitude of velocity.
  float topspeed;

  Mover() {
    location = new PVector(width/2,height/2);
    velocity = new PVector(0,0);
    acceleration = new PVector(-0.001,0.01);
    topspeed = 10;
  }

  void update() {
    //[full] Velocity changes by acceleration and is limited by topspeed.
    velocity.add(acceleration);
    velocity.limit(topspeed);
    //[end]
    location.add(velocity);
  }

  void display() {
    stroke(0);
    fill(175);
    ellipse(location.x,location.y,16,16);
  }

  void checkEdges() {
    if (location.x > width) {
      location.x = 0;
    } else if (location.x < 0) {
      location.x = width;
    }

    if (location.y > height) {
      location.y = 0;
    } else if (location.y < 0) {
      location.y = height;
    }
  }
}

// Declare Mover object.
Mover mover;

void setup() {
  size(640,360);
  // Create Mover object.
  mover = new Mover();
}

void draw() {
  background(255);

  //[full] Call functions on Mover object.
  mover.update();
  mover.checkEdges();
  mover.display();
  //[end]
}
````

- Random acceleration
	- Replace fixed acceleration in the constructor with `acceleration = PVector.random2D();` in
		`update()`
- Acceleration towards the mouse
	- Aha! Finally we get to something that resembles our vehicles!
		Follow along in the last section "Interactivity with Acceleration"
	

### January 27

#### Review

Review the Mover class we created at the end of Monday's session

Multiple Mover objects in an array (did we do this already?) from the book

#### Forces

Why do we care about forces?

Newton's first law of mechanics:

- An object at rest stays at rest and an object in motion stays in motion at a
	constant speed and direction (i.e. velocity) **unless acted upon by an
	unbalanced force**.

(What's the difference between speed and velocity?)

Newton's second law:

- A force acts on an object by changing its acceleration: **F**=m**A** or
	**A**=**F**/m

Acceleration is directly proportional to force and inversely proportional to
mass. 

What does this mean in common language?

How do we apply this in our programs?

Let's start with the last example we did on Monday. We'd like to add a new
member function (method) so that in our main `draw()` loop we might say :

````
mover.applyForce(wind);
````

It might look like this:

````
void applyForce(PVector force) {
  acceleration = force; // Let's pretend mass = 1
}
````

There's a problem though. Suppose we have wind and gravity. Then in `draw()`
we would say

````
mover.applyForce(wind);
mover.applyForce(gravity);
mover.update();
mover.display();
````

The trouble is that applying the force of gravity would erase the force of
wind. We need to *accumulate* all the relevant forces. A more accurate
statement of Newton's third law is

- **Net** Force equals mass times acceleration

so now our function looks like this:

````
void applyForce(PVector force) {
  // Newton’s second law, but with force accumulation.
  // We now add each force to acceleration, one at a time.
    acceleration.add(force);
 }
````

 One more slight problem. If we keep adding all the accelerations with each
 iteration of `draw()`, the acceleration keeps getting bigger and bigger, but
 that's not really what happens in real life. In real life, each time we
 evaluate the total forces, we start from zero and add them one at a time.
 This means that each time through `draw()` we need to first zero out the
 acceleration. Is there a way to zero out an existing vector?


````
 void update() {
    velocity.add(acceleration);
    location.add(velocity);
    acceleration.mult(0);
 }
````

Why do this in`update()`?

Dan Shiffman suggested this exercise:

- Using forces, 
simulate a helium-filled balloon floating upward and bouncing off the ceiling

Here is my start at solving it in class:

````
/*
Example in class 27 Jan
First attempt at showing how forces affect acceleration

Based on the last example from class 25 Jan

I also removed the top speed
*/


class Mover {

  PVector location;
  PVector velocity;
  PVector acceleration;

  Mover() {
    location = new PVector(width/2, height/2);
    velocity = new PVector(0, 0);
    acceleration = new PVector(0, 0);
  }

  void update() {
    velocity.add(acceleration);
    acceleration.mult(0); // this makes sure the acceleration is zer0 for the next fram
    location.add(velocity);
  }

  void display() {
    stroke(0);
    fill(175);
    ellipse(location.x, location.y, 16, 16);
  }

  void checkEdges() {
    if (location.y > height) {
      velocity.y = -velocity.y;
    } else if (location.y < 0) {
      velocity.y = velocity.y*-1;
    }
  }

  void applyForce(PVector force) {
    // A = F/m
    // with m = 1
    acceleration.add(force);
  }
}

Mover mover;

PVector helium = new PVector(0, -0.09); // this force is up
PVector gravity = new PVector(0, 0.0001); // this force is down

void setup() {
  size(640, 360);
  mover = new Mover();
}

void draw() {
  background(255);

  mover.applyForce(helium);
  mover.applyForce(gravity);
  mover.update();
  mover.checkEdges();
  mover.display();
}
````

### Week 3
### February 1

##### Review

What have we learned so far?
- How to use PVectors to store location and velocity,
and how to make a simple ball bouncing off the walls using PVectors
- About **force** which causes acceleration (another PVector)
- How to combine multiple forces to calculate a total acceleration
- How to zero out the acceleration after each frame, and why that's necessary
- How to apply the acceleration to modify the velocity

We also made a simplification:
- Let the mass equal 1 so that Newton’s second law 
(**F**=m**a**) 
becomes
**F**=**a**

Review the helium balloon example from last week

##### Mass

Interesting things happen when objects can have different mass. How do we
incorporate that?
- Pretty easy: Just add a scalar variable *mass* to the class variables
and for now give it an arbitrary fixed number e.g. 10.
- How do we use this new value?

````
void applyForce(PVector force) {
   // Newton’s second law (with force accumulation and mass)
	 // Since F=ma
	 // then a=F/m
   force.div(mass);
   acceleration.add(force);
	 }
````

and then

````
Mover m1 = new Mover();
Mover m2 = new Mover();

PVector wind = new PVector(1,0);

m1.applyForce(wind); // apply wind force to object 1
m2.applyForce(wind);
````

Uh oh. Big problem. Can anyone see it?

Explain this:

````
void scalarFunction(int s) {
  s = 0;
}

void pvectorFunction(PVector pv) {
  pv.x = 0;
  pv.y = 0;
}

void setup() {
  int a = 9;
  scalarFunction(a);
  println("The scalar a is now " + a);
  
  PVector vectorA = new PVector(9, 7);
  pvectorFunction(vectorA);
  println("The vector x component is now = " + vectorA.x + " and the vector y component is now = " + vectorA.y);
}
````

Once we understand that, we see that we have to do this:

````
void applyForce(PVector force) {
  PVector f = force.get(); // Make a copy of the PVector before using it!
  f.div(mass);
  acceleration.add(f);
}
````

(Why didn't we have this problem last week?)

Ok, we're getting closer. Now modify the class to allow
objects to have different mass and different initial locations.
(Do this as an exercise in class)

Now we can make multiple objects of different masses 
and different initial locations. An array is a great way 
to manage them. Can you combine these things with what we have up to now to
make an array of bouncing objects?

````
Mover[] movers = new Mover[100];

void setup() {
  for (int i = 0; i < movers.length; i++) {
		// same starting location but differen masses
    movers[i] = new Mover(random(0.1,5),0,0);
  }
}

void draw() {
  background(255);

	// You can make up all kinds of forces!
  PVector wind = new PVector(0.01,0);
  PVector gravity = new PVector(0,0.1);


	// Loop through all objects and apply both forces to each object.
  for (int i = 0; i < movers.length; i++) {
    movers[i].applyForce(wind);
    movers[i].applyForce(gravity);

    movers[i].update();
    movers[i].display();
    movers[i].checkEdges();
  }
}
````

To make the different masses visible, we can draw them different sizes:

````
void display() {
    stroke(0);
    fill(175);
    // Scale the size according to mass
    ellipse(location.x,location.y,mass*16,mass*16);
  }
````

##### The general procedure for incorporating a force
We're going to learn about two new forces:
1. The force caused by friction. 
1. The force caused by gravitational attraction between two objects
You may not always need either of these, just as you may not always need
gravity or wind.  The point is to evaluate these as case
studies for the following general procedure:

- Understand the concept behind a force
- Deconstruct the force’s effect into two parts:
	- How do we compute the force’s direction?
	- How do we compute the force’s magnitude?
- Translating that force's effect into Processing code that calculates a
	`PVector` to be sent through our Mover's `applyForce()` function

##### Friction
Look at Figure 2.3 in Shiffman's book *The Nature of Code*
- Explain the forumula **Friction Force** = -1 * mu * **N** * **unit_v**
	- mu = coefficient of friction
	- **N** = normal force (force perpendicular to the surface)
	- **unit_v** = unit velocity vector (i.e. the direction of motion scaled to
		value of 1)
- Let's start with the -1 * **unit_v**:

````
PVector friction = velocity.get();
friction.normalize();
friction.mult(-1);
````

- What about the coefficient of friction? Let's pick an arbitrary value:

````
float c = 0.01;
````

- What about normal force? Again, simplify:

````
float normal = 1;
````

Putting it all together we have:

````
void draw() {
  background(255);

  PVector wind = new PVector(0.001,0);
  PVector gravity = new PVector(0,0.1);

  for (int i = 0; i < movers.length; i++) {

    float c = 0.01; 
    PVector friction = movers[i].velocity.get(); 
    friction.mult(-1); 
    friction.normalize(); 
    friction.mult(c); 

    // Apply the friction force vector to the object.
    movers[i].applyForce(friction); 
    movers[i].applyForce(wind);
    movers[i].applyForce(gravity);

    movers[i].update();
    movers[i].display();
    movers[i].checkEdges();
  }

}
````

##### Gravitational Attraction

Remember that gravity isn't just the earth pulling us down. We are also
pulling the earth with exactly as much force

**Gravitational Force** 
= (G * m1 * m2 **unit vector pointing from m1 to m2**)/r * r
- G = Gravitational constant
- m1, m2 = mass of the two bodies
- r = distance between objects

What does this mean intuitively?
- The more mass, the more force
- The more distance, the **less** force

(If the force is the same, why do we move towards the earth (e.g. if we jump)
and not the earth move towards us?)

We can also separate this into two components:

The magnitude (scalar valufe) of the force = (G * m1 * m2)/r * r
The direction (vector) of the force = **unit vector pointing from m1 to m2**

````
// The vector that points from one object to another
// Will give us the direction (vector) of the force 
PVector force = PVector.sub(location1,location2);

// The length (magnitude) of that vector
// conveniently gives us the distance
float distance = force.magnitude();

// Use the formula for gravity to compute the strength of the force.
float magnitude = (G * mass1 * mass2) / (distance * distance);

// Normalize the force vector
force.normalize();

// Scale the force vector to the appropriate magnitude.
force.mult(magnitude);
````

##### An Attractor class

Now let's apply this force in a simple example: A moving object (a Mover) and
a stationary attractive object (an Attractor)

(Figure 2.9 in book)

Here is a simple Attractor class:

````
class Attractor {
  // Our Attractor is a simple object that doesn’t move.
  // We just need a mass and a location.
  float mass;
  PVector location;

  Attractor() {
    location = new PVector(width/2,height/2);
    mass = 20;
  }

  void display() {
    stroke(0);
    fill(175,200);
    ellipse(location.x,location.y,mass*2,mass*2);
  }
}
````

And to use it:

````
Mover m;
Attractor a;

void setup() {
  size(640,360);
  m = new Mover();
  // Initialize Attractor object.
  a = new Attractor();
}

void draw() {
  background(255);

  // Display Attractor object.
  a.display();

  m.update();
  m.display();
}
````

The challenge now is how to get the Attractor object to have an effect on 
a Mover object. How does one object communicate with another?

(See some options in book)

We will do option #4 because it makes use of the `applyForce()` function we
developed earlier. In other words, where we earlier made up a force:

````
// Made-up force
PVector f = new PVector(0.1,0);
m.applyForce(f);
````

We will now have:

````
void draw() {
  background(255);

	//Get the attractive force to mover m from attractor a
  PVector f = a.attract(m); 
	// Apply this force to the mover
  m.applyForce(f); 

  m.update();

  a.display();
  m.display();
}
````

and now we need to write the `attract()` method in the Attractor class,
using the math that we worked out just above:

````
PVector attract(Mover m) {

  // What’s the force’s direction?
  PVector force = PVector.sub(location,m.location);
  float distance = force.mag();
  force.normalize();
  //[offset-down] What’s the force’s magnitude?
  float strength = (G * mass * m.mass) / (distance * distance);
  force.mult(strength);

  // Return the force so that it can be applied!
  return force;
}
````

Any problems with this?

What happens if the distance is really, really small, like 0.0000000001?

It might be useful to constrain the force to reasonable values (which you can
play around with) 

````
	distance = constrain(distance,5,25);
````

So finally we can combine all of this:

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
class Mover {

  PVector location;
  PVector velocity;
  PVector acceleration;
  float mass;

  Mover() {
    location = new PVector(0, 0);
    velocity = new PVector(0, 0);
    acceleration = new PVector(0, 0);
    mass = .1;
  }

  void update() {
    velocity.add(acceleration);
    acceleration.mult(0); // this makes sure the acceleration is zer0 for the next fram
    location.add(velocity);
  }

  void display() {
    stroke(0);
    fill(175);
    ellipse(location.x, location.y, 16, 16);
  }

  void applyForce(PVector force) {
    PVector f = force.get(); // Make a copy of the PVector before using it!
    f.div(mass);
    acceleration.add(f);
  }
}
````

### February 3
#### Administration
- **Important**: The point of your presentation is to study, think about, and
	critique your subject and the underlying assumptions and ethics. It is 
	**not** meant to be a simple presentation of history,technology, or facts.

- `PVector.get()` deprecated; use `PVector.copy()` instead.
- How to make an object rotate in the direction of travel

````
// A Mover and an Attractor
Mover m;
Attractor a;

void setup() {
  size(640, 360);
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
    location = new PVector(0, 0);
    mass = 20;
    G = 0.4;
  }

  PVector attract(Mover m) {
    // Move the attractor to wherever the mouse is
    location.set(mouseX, mouseY);

    PVector force = PVector.sub(location, m.location);
    float distance = force.mag();
    // Remember, we need to constrain the distance
    // so that our circle doesn’t spin out of control.
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
class Mover {

  PVector location;
  PVector velocity;
  PVector acceleration;
  float mass;

  Mover() {
    location = new PVector(0, 0);
    velocity = new PVector(0, 0);
    acceleration = new PVector(0, 0);
    mass = .1;
  }

  void update() {
    velocity.add(acceleration);
    acceleration.mult(0); // this makes sure the acceleration is zer0 for the next fram
    location.add(velocity);
  }

  void display() {
    stroke(0);
    fill(175);

    // Rotate the mover to point in the direction of travel
    rectMode(CENTER);
    pushMatrix();
    translate(location.x, location.y);
    rotate(velocity.heading());
    rect(0, 0, 30, 10);
    popMatrix();
  }

  void applyForce(PVector force) {
    PVector f = force.get(); // Make a copy of the PVector before using it!
    f.div(mass);
    acceleration.add(f);
  }
}
````
- Look at homework for next week

- Review 
	- Trigonometry
	- Polar coordinates

### Week 4
### February 8

##### Look at Homework

##### Autonomouse Agents: Action Selection and Steering

Today we will be looking at simulating desires. Try to keep in mind that these
are just suggestions, and you should start thinking of synthetic desires you
might implement

The term autonomous agent generally refers to an entity that makes its own
choices about how to act in its environment without any influence from a
leader or global plan.

* An autonomous agent has a limited ability to perceive environment. 
* An autonomous agent processes the information from its environment and
calculates an action. This action is a force.

##### Vehicles and Steering
Action Selection: A vehicle has a goal (or goals) and can select an action (or
a combination of actions) based on that goal. This is essentially where we
left off with autonomous agents. The vehicle takes a look at its environment
and calculates an action based on a desire

Steering: Once an action has been selected, the vehicle has to calculate its
next move. For us, the next move will be a force; more specifically, a
steering force: 

**steering force = desired velocity - current velocity**

##### Cybernetics

The core concept of Cybernetics is circular causality or feedback—that is,
where the outcomes of actions are taken as inputs for further action.

(*cyber* comes from the Greek word for *steer*. We also get the word *govern*
from *cyber*)

We will think literally in this chapter (follow that pixel!), but you can
build on this by thinking more abstractly (like Braitenberg). What would it
mean for your vehicle to have “love” or “fear” as its goal, its driving force?

For you to be creative (to make these steering behaviors your own) you might
combine multiple actions within the same vehicle. So view these examples not
as singular behaviors to be emulated, but as pieces of a larger puzzle that
you will eventually assemble.

#####  The Steering Force

Again, from above:

**steering force = desired velocity - current velocity**

This is super important. How do we implement it?

````
PVector steer = PVector.sub(desiredVelocity,currentVelocity);
````

What is the `desiredVelocity`? It is the vector that will get us to the
target, in other words, the target's location:

````
PVector desiredVelocity = PVector.sub(targetLocation, ourCurrentLocation);
````

But if we just used this directly it would just instantly move there. To make
it more lifelike, we might start by saying:

**The vehicle desires to move towards the target at maximum speed**

How do we apply this? We set the `desiredVelocity` magnitude to 
our maximum speed (which we arbitrarily set at some number):

````
// Get the vector to the target
// this gives us the direction
PVector desiredVelocity = PVector.sub(targetLocation, ourCurrentLocation);

// Make the magnitude of the vector our maximum speed:
desiredVelocity.normalize();
desiredVelocity.mult(maxspeed);
````

We can implement this in a new method for our Vehicle class:

````
void seek(PVector target) {
	PVector desired = PVector.sub(target,location);
	desired.normalize();
	// Heading to the target at maximum speed
	desired.mult(maxspeed);

	// Reynolds’s formula for steering force
	// Also the foundation of cybernetics
	PVector steer = PVector.sub(desired,velocity);
	// Using our physics model and applying the force
	// to the object’s acceleration
	applyForce(steer);
}
````

The nice thing about vectors is you can inspect them visually as well:
See Figure 6.4

As another example of making it more lifelike, we might limit the force:

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

Assuming our Vehicle class has these variables:

````
class Vehicle {
  PVector location;
  PVector velocity;
  PVector acceleration;
  // Maximum speed
  float maxspeed;
  // And maximum force
  float maxforce;
````

Putting this all together we have:

````
class Vehicle {

  PVector location;
  PVector velocity;
  PVector acceleration;
  // Additional variable for size
  float r;
  float maxforce;
  float maxspeed;

  Vehicle(float x, float y) {
    acceleration = new PVector(0,0);
    velocity = new PVector(0,0);
    location = new PVector(x,y);
    r = 3.0;
    //[full] Arbitrary values for maxspeed and
    // force; try varying these!
    maxspeed = 4;
    maxforce = 0.1;
    //[end]
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
  void seek(PVector target) {
    PVector desired = PVector.sub(target,location);
    desired.normalize();
    desired.mult(maxspeed);
    PVector steer = PVector.sub(desired,velocity);
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
    translate(location.x,location.y);
    rotate(theta);
    beginShape();
    vertex(0, -r*2);
    vertex(-r, r*2);
    vertex(r, r*2);
    endShape(CLOSE);
    popMatrix();
  }
}

````

What do we need to test this? Let's put the target wherever we click the
mouse 
(Try to figure this out on your own before scrolling down)

````



















````

````
PVector target = new PVector(0,0);
void mouseClicked() {
  target.x = mouseX;
  target.y = mouseY;
}
````

setup() just needs to create a new Vehicle object.
What does draw() do?
(Try to figure this out on your own before scrolling down)

````



















````

````
void draw() {
  background(255);

  // Draw the target
  circle(target.x, target.y, 20);

  // Now tell the vehicle to go there
  v.seek(target);
  v.update();
  v.display();
}
````

Exercises 6.1, 6.2, 6.3

#####  Arriving Behaviour

- What are we doing that causes it to overshoot?
- How can we avoid this?
(Try to figure this out on your own before scrolling down)

````



















````
````
void arrive(PVector target) {
	PVector desired = PVector.sub(target,location);

	// The distance is the magnitude of
	// the vector pointing from
	// location to target.
	float d = desired.mag();
	desired.normalize();
	// If we are closer than 100 pixels...
	if (d < 100) {
		//[full] ...set the magnitude
		// according to how close we are.
		float m = map(d,0,100,0,maxspeed);
		desired.mult(m);
		//[end]
	} else {
		// Otherwise, proceed at maximum speed.
		desired.mult(maxspeed);
	}

	// The usual steering = desired - velocity
	PVector steer = PVector.sub(desired,velocity);
	steer.limit(maxforce);
	applyForce(steer);
}
````

*Gravity always points straight down, no matter how close you are, and no
matter how weak or strong the force is. The steering function, however, says:
“I have the ability to perceive the environment.” The force isn’t based on
just the desired velocity, but on the desired velocity relative to the current
velocity. Only things that are alive can know their current velocity. A box
falling off a table doesn’t know it’s falling. A cheetah chasing its prey,
however, knows it is chasing.*

*The steering force, therefore, is essentially a manifestation of the current
velocity’s error: "I’m supposed to be going this fast in this direction, but
I’m actually going this fast in another direction. My error is the difference
between where I want to go and where I am currently going." Taking that error
and applying it as a steering force results in more dynamic, lifelike
simulations. With gravitational attraction, you would never have a force
pointing away from the target, no matter how close. But with arriving via
steering, if you are moving too fast towards the target, the error would
actually tell you to slow down!*

##### Your Own Desires: Desired Velocity

What behaviours do we have so far?
- Seek 
- Arrive

We will now look at some more behaviours.
These are meant to be inspiration for other behaviours you can come up with.
**As long as you can come up with a vector that describes a vehicle’s desired
velocity, then you have created your own steering behaviour.**

##### Wandering

*Figure 6.12 illustrates how the vehicle predicts its future location as a
fixed distance in front of it (in the direction of its velocity), draws a
circle with radius r at that location, and picks a random point along the
circumference of the circle. That random point moves randomly around the
circle in each frame of animation. And that random point is the vehicle’s
target, its desired vector pointing in that direction.*

*But the seemingly random and arbitrary nature of this solution should drive
home the point I’m trying to make—these are made-up behaviors inspired by
real-life motion. You can just as easily concoct some elaborate scenario to
compute a desired velocity yourself. And you should.*

Exercise 6.4

Review polar coordinates:

Converting from  Cartesian (x,y) to polar (r,theta):

````
PVector v = new PVector(10, 20);

float theta = v.heading();
float r = v.mag();
````

Converting from polar (r,theta) to Cartesian (x,y):

````
float r = 75;
float theta = PI / 4;

float x = r * cos(theta);
float y = r * sin(theta);
````

##### Stay within walls

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
	resolution = 10;
	// Total columns equals width divided by resolution.
	cols = width/resolution;
	// Total rows equals height divided by resolution.
	rows = height/resolution;
	field = new PVector[cols][rows];
}

// to make a random force field
for (int i = 0; i < cols; i++) {
  for (int j = 0; j < rows; j++) {
    field[i][j] = PVector.2D();
  }
}

// To make a random field using Perlin noise, mapped to an angle
float xoff = 0;
for (int i = 0; i < cols; i++) {
  float yoff = 0;
  for (int j = 0; j < rows; j++) {
    //[offset-up] Noise
    float theta = map(noise(xoff,yoff),0,1,0,TWO_PI);
    field[i][j] = new PVector(cos(theta),sin(theta));
    yoff += 0.1;
  }
  xoff += 0.1;
}
````

Now the vehicle needs to know where it is in this force field:

````
int column = int(location.x/resolution);
int row = int(location.y/resolution);
````

and our force field class should provide a method which return
the force vector at that that location:

````
 PVector lookup(PVector lookup) {

	// Using constrain()
	int column = int(constrain(lookup.x/resolution,0,cols-1));
	int row = int(constrain(lookup.y/resolution,0,rows-1));

	// Note the use of get() to ensure
	// we return a copy of the PVector.
	return field[column][row].get();
}
````

### February 10
- Ideas for behaviour:
	- Amina:
		- Asteroids, rocket to avoid asteroids
		- Planet, rocket to seek the planet
	- Katie
		- Cat follows light
		- parking car/landing airplane
		- trying to find your phone when it makes a sound
	- Kyrie
		- looking for a hole
		- smart mouse (avoid cheese in trap)
		- navigating maze of traps
	- Maria
		- moving a ball using the mouse
		- cirlce items inside moves freely, outside of circle move slower
	- Nathan
		- gliding on wind currents
		- ship sinking in water vortex
		- bugs scatter from dots
	- Fatima
		- Creeping (slowly inching towards an object)
		- Observing (coming close and then slowing down and then leaving)
		- Contemplation (vehicles converge and then chose a direction)
	- Alpha
		- Game: catching something thrown (wandering, following)
		- Force field to create water effect (also curls)
	- Shaurya
		- approach something by being sneaky 
		- "reverse tetris" 
	- Tori
		- dog pulling on leash
		- fish in school
		- predator stalking prey (like Shaurya's sneaky)
	- Pangna
		- object in space influenced by black holes, planets
		- maze: find exit 
		- ship on sea affected by water and wind currents while trying explore islands
	- Leo
		- leaf being blown by wind
		- boat on a calm lake pushed by wind
		- boat on a stormy sea in huge waves
	- Zheki
		- 2 types of beings (hunters, gatherers) and food
	- Rashid
		- solar system planets wander around sun 
		- particles wander and then come together to form a picture

#### Perlin Noise

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

Let's review what we started on Monday. Let's put that into a program. 
How do we know it's working? Let's show the flow field like Shiffman does in
the book.

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
  int resolution; // Size of each square in the grid, in pixels

  // Constructor takes the desired resolution
  FlowField(int _res) {
    resolution = _res;
    cols = width/resolution;
    rows = height/resolution;

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
    int column = int(constrain(lookup.x/resolution, 0, cols-1));
    int row = int(constrain(lookup.y/resolution, 0, rows-1));

    return field[column][row].copy();
  }

  // Display the flow field so we can see if it looks like what we think it should
  //
  void display() {
    for (int i = 0; i < cols; i++) {
      for (int j = 0; j < rows; j++) {
        print("col " + i + " row " + j + "  ");
        println(i*resolution, j*resolution, field[i][j].x, field[i][j].y);
        pushMatrix();

        // This translates to the top left corner of the grid, but really
        // it should center the vector in the middle of the grid
        translate(i*resolution, j*resolution);
        PVector f = field[i][j].copy();
        f.mult(resolution);
        line(0, 0, f.x, f.y);
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
  // Additional variable for size
  float r;
  float maxforce;
  float maxspeed;

  Vehicle(float x, float y) {
    acceleration = new PVector(0, 0);
    velocity = new PVector(0, 0);
    location = new PVector(x, y);
    r = 3.0;
    //Arbitrary values for maxspeed and
    // force; try varying these!
    maxspeed = 4;
    maxforce = 10;
  }

  // Update the velocity and location, based on the acceleration generated by the steering force
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
  v.update(); // Update the velocity and location, based on the acceleration generated by the steering force
  v.display(); // display the vehicle
}
````

Here is a short example showing how to work with ArrayLists:

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
      PVector v = myVectors.get(i);
      println("index = " + i + " x = " + v.x + " y = " + v.y);
    }
  }
}

// Need to have a draw() function so that callbacks occur
void draw() {
}
````

### Week 5
### February 15
- Record!

Look at homework

Respond to questions that came up


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
			PVector weAt = new PVector(i*resolution, j*resolution);
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

##### Multiple vehicles

````



















````

````
ArrayList<Vehicle> vehicles = new ArrayList<Vehicle>();

vehicles.add(new Vehicle(width, height/2));

for (Vehicle v : vehicles) { 

vehicles.add(new Vehicle(mouseX, mouseY));
````

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

- Feedback. Complex systems often include a feedback loop where the the
		output of the system is fed back into the system to influence its behavior
		in a positive or negative direction. 

Complexity will serve as a theme for this class.

We’ll begin by adding one more feature to our Vehicle class: an
ability to look at neighboring vehicles.

Let's start with a bunch of vehicles. What do we need to do in `setup()` and
`draw()`?

````
// Declare an ArrayList of Vehicle objects.
ArrayList<Vehicle> vehicles;

void setup() {
  // Initialize and fill the ArrayList
  // with a bunch of Vehicles.
  vehicles = new ArrayList<Vehicle>;
  for (int i = 0; i < 100; i++) {
    vehicles.add(new Vehicle(random(width),random(height)));
  }
}

void draw(){
  for (Vehicle v : vehicles) {
    v.update();
    v.display();
  }
}
````

Now we want to add some behaviours. We could make them e.g. follow the mouse, 
but the whole purpose now is to have them relate to each other in various
ways, so how about keeping away from neighbors:

````
    v.separate(vehicles);
````

Note the huge difference here! Each vehicle needs to look at what all the
other vehicles is doing! This is how real living beings operate.

what does the `separate()` function do?
- If a given vehicle is too close to you, steer away from that vehicle. How do
	we do this?
	- We already know how to steer towards a target. Reverse that force and we
		have the flee behavior.  
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

	// Apply the force to the Vehicle’s
	// acceleration.
	applyForce(steer);
}
````

[Here](https://github.com/michaelshiloh/resourcesForClasses/blob/master/src/processingSketches/robotaPsyche/separate/separate.pde)
is the program we developed in class putting this all together


### February 17

Error in reading assignment for next week: Read Vehicles 5 and 6

Discussion:
- Introductory readings
- Casey Reas' Eyeo talk
- Vehicles 4
- Toys, Totems, and Tools (The Living Brain)

Questions
- How do we distinguish between humans and other beings (what does Descarte
	say?)
- How do you think we make this distinction now?
- How might we decide whether another being is conscious?
- Does this change if the other being is a machine?
- How do we distinguish between something that appears to be conscious and
	something that really is coscious? Does this change depending on whether the
	other being is an animal, machine, or alien?
- How do we know that other humans are conscious? How do we know that we are
	conscious?
- Can, and should, machines be held responsible for actions that affect
	humans?
- Can machines understand the difference between what is right and what is
	wrong? 
- Is "right" and "wrong" universal? What if aliens have different notions of
	this? How should we treat them? How should they treat us?
- What responsibilities do we have towards conscious machines?
- Do conscious machines have rights?
- Is emergent behaviour real?
- How are Ross Ashby's Homeostat and Grey Walter's tortoises different from 
animals?

##### Reorganize the code we have:

Vehicle
- Behaviours
	- Seek (target) 
	- Follow (flowfield)
	- Separate (ArrayList of all objects)
- all behaviours do this:
	- Calculate the force
	- apply the force 
		- which adds it to the cumulative acceleration
- update()
	- apply acceleration to velocity
	- apply velocity to location
	- clear the acceleration for the next frame


If we're going to apply multiple forces, it turns out to be useful
to modify this slightly:

1. Each behaviour does not apply but returns the force
1. Main program adjust the relative forces if we wish
1. Main program applies the forces

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





### Week 6

### February 22
Further work on evolution

### February 24

Student Presentation: Youssef

Debugging
- inspector

### Week 7

### March 1
##### todays-lecture
##### Administration
- Record 
- Notifications off
- One presentation each day rather than two on Wednesday

##### Look at evolution projects

##### Student Presentation: Rashid

##### Debugging
- Pause, Single step, Run, slow down, speed up

##### Memory
- Reset
- New class
- When did something happen?

### March 3

### Week 8
### March 17
### March 22

### Week 9
### March 24
### March 29

### Week 10
### March 31
### April 5

### Week 11
### April 7
### April 12

### Week 12
### April 14
### April 19

### Week 13
### April 21
### April 26

### Week 14
### April 28
### May 3
