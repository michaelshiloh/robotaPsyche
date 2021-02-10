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
- Notifications off
- Record 
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
##### todays-lecture
- Record!
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

### Week 5
### February 15
### February 17

### Week 6
### February 22
### February 24

### Week 7
### March 1
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
