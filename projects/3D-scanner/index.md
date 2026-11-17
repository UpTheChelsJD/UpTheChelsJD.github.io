

# Building a DIY Arduino-Based 3D Scanner

I am so glad I could do this project over a span of many days. I have learnt a lot everyday, and I have many great resources around me to make this task very fruitful and joyful.

---

## How We Started

When our team first started this project, we wanted to build something that combined mechanical engineering, programming, and 3D design. We knew our task was about using the stepper motors, so we decided to create a DIY 3D scanner using Arduino and many motors and the materials available in our robotics lab.

Wachi had some inspiration after watching a youtube video on the same concept. His idea was to build a machine that could measure a physical object from different angles and heights, eventually allowing us to reconstruct its shape as a digital 3D model. He wanted the **first motor** to rotate a circular platform holding the object. The platform would turn through **360 degrees so the sensor could collect measurements from different angles**. He then wanted the **second motor** to control the vertical movement of a Sharp infrared distance sensor. By moving the sensor up and down, we could measure different sections of the object.

In theory, combining these movements would allow us to collect enough measurements to begin reconstructing a three-dimensional model.

However, we quickly realized that designing the system was much easier than actually building it. It was a matter of time before we met a lot of difficulties along the way.

### Our Initial Design

```text
       Sharp IR Sensor
              |
       Vertical Movement
              |
        Stepper Motor 2

              +

      Rotating Platform
              |
       Stepper Motor 1
              |
       360° Rotation
```

## Our Original Plan: Using a Shaft

We initially took inspiration from existing DIY 3D scanners that used a threaded shaft to move a sensor vertically. That mechanism seemed relatively straightforward. A stepper motor would rotate a threaded lead screw, and a nut connected to the sensor carriage would travel along the screw. Two smooth guide rods would prevent the carriage from rotating or becoming unstable.

**Stepper Motor → Lead Screw → Moving Carriage → IR Sensor**

![](../../images/S1.png)

The above picture was what we tried to aim for, yet **when we checked the materials available in our robotics lab**, we encountered our **FIRST MAJOR PROBLEM**.

**We did not have a T8 lead screw or two smooth 8 mm guide rods.**

We did, however, have a long hexagonal metal rod that was approximately 6–7 mm thick.

At first, we wondered whether we could simply substitute this rod for the threaded shaft. After exploring how lead screws work, we realized that this would not be possible without modifying the mechanism. The hexagonal rod had no threads, meaning it could not move a nut up and down through rotation.

This was frustrating because it meant our original mechanical design would not work with the components we had. And so instead of buying an entirely new set of materials, we decided to investigate another approach - belts.

## Changing Our Design: From Shaft to Belt

After realizing that our original mechanism would not be practical, we began exploring how belts and pulleys could produce vertical movement. We wanted to achieve the same result as the lead screw system: moving the Sharp IR sensor up and down while keeping it stable.

Our revised concept used a timing belt driven by two stepper motors - one on top and the other down the bottom.

The motor would rotate a pulley at the bottom of the structure. The belt would run around this pulley and another pulley at the top, forming a continuous loop.

When the motor rotated, the belt would move. A carriage attached to one side of the belt would then travel vertically (**this we would need to CAD in future classes**) The Sharp IR sensor would be secured to this carriage rather than directly attached to the belt.


### Design Documentation

**Belt and Pulley Mechanism**

![](../../images/S2.png)

![](../../images/S3.png)

### Design Reflection

This was actually very rewarding. We miscalculated the first time, and we only have one motor running right now. Next class, we would need to add another motor to make the top part spin, so we could have a spinning belt. The design is pretty good and cohesive according to plan. We now need to upgrade some parts of our design and make it more stable. We need to CAD out the sharp structure connection part onto the stepper motor's belt so it can move up and down.

Yet, here is just a video to show how we were able to make one connection part work! This would mean that we could replicate this into two or even 3!

<video style="width:100%; max-width:700px;" controls>
  <source src="../../videos/S5.mp4" type="video/mp4">
</video>
---

## Designing the Rotating Platform in Onshape

While working on the vertical mechanism, we also spent a significant amount of time developing the rotating platform. This part was especially important because the object needs to remain stable while the motor rotates it through 360 degrees.

We originally thought that creating a circular platform would be one of the easiest parts of the project.

However, the connection between the platform and the stepper motor turned out to be much more complicated than we expected. It would often times bend and not stick properly, causing us to use many prototypes and making the radius a little larger every time.

![](../../images/S4.png)

The above is our final design with the stepper motor on!

### Some Problems Regarding our first prototype.

Wachi took a long time first testing the multimeter, and after many classes, the wiring of the stepper motors were still not fixed. I ended up helping him, and a crucial part he forgot a little bit about was how the stepper motors had to match up together in order to function properly. With four nodes of **red, green, blue, and black**, each node is correspondent with another, and matching is very important.

Hence, here is just a video to show our current progress with the motors and how they are spinning already!

<video style="width:100%; max-width:700px;" controls>
  <source src="../../videos/S6.mp4" type="video/mp4">
</video>

## What We Have Understood, What We Have Learnt, and What We Have Used.

### Parts and Components We Worked With

| Component | Purpose |
|---|---|
| Arduino Uno R3 | Controls the stepper motors |
| Stepper Motor 1 | Rotates the object holder |
| Stepper Motor 2 | Drives the vertical movement system |
| Circular Platform | Holds the object during scanning |
| Sharp IR Sensor | Measures distance to the object |
| Timing Belt | Transfers motor rotation into vertical movement |
| Pulleys | Drive and guide the timing belt |
| Hexagonal Rod | Potential guide for the sensor carriage |
| Custom CAD Parts | Support and connect mechanical components |
| Base Structure | Supports the overall scanner |

### Robotics Lab

A huge thanks to Dr. Dzula for opening the robotics lab! We really got into working with the laser cutter, and many components listed above was from the robotics lab. The many M4 screws, M3 screws, dangling parts, connection metal pieces, small screwdrivers, rulers, laser cutter, 3D SCANNER, arduino, the belts, the hot glue gun, and the long hexagonal poles were all from the Robotics lab.

**Our Physical Prototype**

![](../../images/S7.png)

---

### Arduino Programming and Motor Testing

Another major part of our project involved connecting the stepper motors to the Arduino and writing code to control them. We used an Arduino Uno R3 and experimented with different motor setups. Our goal was to independently control the motor responsible for the rotating platform and the motor responsible for the vertical movement mechanism. However, at this stage, our Arduino program is still a very early prototype.

### What Our Current Code Does

```cpp
const int stepX = 2;
const int dirX = 5;

const int stepY = 3;
const int dirY = 6;

const int enablePin = 8;

// Smaller interval = faster rotation
const unsigned long fastSpeed = 5000;
const unsigned long slowSpeed = 20000;

unsigned long lastStepX = 0;
unsigned long lastStepY = 0;

void setup() {
  pinMode(stepX, OUTPUT);
  pinMode(dirX, OUTPUT);

  pinMode(stepY, OUTPUT);
  pinMode(dirY, OUTPUT);

  pinMode(enablePin, OUTPUT);

  digitalWrite(dirX, HIGH);
  digitalWrite(dirY, HIGH);

  digitalWrite(enablePin, LOW);
}

void loop() {
  unsigned long currentTime = micros();

  // Motor 1: Fast rotation
  if (currentTime - lastStepX >= fastSpeed) {
    lastStepX = currentTime;

    digitalWrite(stepX, HIGH);
    delayMicroseconds(5);
    digitalWrite(stepX, LOW);
  }

  // Motor 2: Slow rotation
  if (currentTime - lastStepY >= slowSpeed) {
    lastStepY = currentTime;

    digitalWrite(stepY, HIGH);
    delayMicroseconds(5);
    digitalWrite(stepY, LOW);
  }
}
```

Our current program is designed mainly to test whether the motors can rotate continuously. We have experimented with running two stepper motors at different speeds. One motor is intended for the rotating object platform, while the other is intended for the vertical belt mechanism.
**At the moment, our code simply keeps the motors spinning - one slow, the other fast. It does not yet perform an automated 3D scanning sequence.**


## AI Usage and Our Own Contributions

To be honest, we used ChatGPT throughout parts of our project as a resource for learning about mechanical design and exploring possible solutions. **We want to be transparent about its role.**

ChatGPT helped us understand how different vertical movement mechanisms work, including lead screws, linear guide rods, timing belts, pulleys, and moving carriages. It also provided conceptual diagrams and explanations that inspired some of our structural ideas.

However, there is a significant difference between receiving an idea and implementing it in a physical machine. The practical development of the project was carried out by our team, and the whole mechanical side was done by my friends and I. We were responsible for designing and repeatedly modifying our CAD models, experimenting with motor configurations, connecting the Arduino and motors, developing and testing the Arduino code, and debugging the system. It was tedious but rewarding alas.

**Overall, AI supported our understanding and brainstorming, while the hands-on design, programming, fabrication, troubleshooting, and engineering decisions were completed by our team.**

---

## Final Thoughts at this Juncture of Time

Looking back at our work so far, this project has been more complicated than we expected. We had thought of the task being simple, but it clearly isn't that easy! We will work harder for the final week and push for a good result!

[← Back to All Projects](../../)

