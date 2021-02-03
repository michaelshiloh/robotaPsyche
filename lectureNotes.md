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
##### todays-lecture
#### Administration
- Notifications off
- Record 
- **Important**: The point of your presentation is to study, think about, and
	critique your subject and the underlying assumptions and ethics. It is 
	**not** meant to be a simple presentation of history,technology, or facts.

- `PVector.get()` deprecated; use `PVector.copy()` instead.
- How to make an object rotate in the direction of travel

````
  void display() {

    stroke(0);
    fill(175);

    rectMode(CENTER);
    pushMatrix();
    translate(location.x,location.y);
    rotate(velocity.heading());
    rect(0,0,30,10);
    popMatrix();
  }
````

- Look at homework for next week

- Time permitting: 
	- Review 
		- Trigonometry
		- Polar coordinates

### Week 4
### February 8
### February 10

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
