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
##### todays-lecture
### January 25
#### Administration
- Notifications off
- Record 

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

	### Week 3
	### February 1
	### February 3

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
