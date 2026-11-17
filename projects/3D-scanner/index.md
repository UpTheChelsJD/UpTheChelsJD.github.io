
---
layout: default
title: "Innovator's Journal - DIY 3D Scanner"
---

# Innovator's Journal
## Building an Arduino-Based 3D Scanner

**Project:** DIY 3D Scanner  
**Team Members:** [Team Members]  
**Location:** Robotics Lab  
**Project Status:** In Development

---

## 1. How We Started

When our team first started this project, we wanted to build something that combined mechanical engineering, programming, and 3D design. We decided to create a DIY 3D scanner using Arduino and materials available in our robotics lab.

The idea was to build a machine that could measure a physical object from different angles and heights, eventually allowing us to reconstruct its shape as a digital 3D model.

Our initial concept involved two stepper motors with different responsibilities.

The first motor would rotate a circular platform holding the object. The platform would turn through 360 degrees so the sensor could collect measurements from different angles.

The second motor would control the vertical movement of a Sharp infrared distance sensor. By moving the sensor up and down, we could measure different sections of the object.

In theory, combining these movements would allow us to collect enough measurements to begin reconstructing a three-dimensional model.

However, we quickly realized that designing the system was much easier than actually building it.

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

**Image: Initial Design Concept**

[Insert photograph or sketch of our original scanner design here]

---

## 2. Our Original Plan: Using a Shaft

We initially took inspiration from existing DIY 3D scanners that used a threaded shaft to move a sensor vertically.

The mechanism seemed relatively straightforward. A stepper motor would rotate a threaded lead screw, and a nut connected to the sensor carriage would travel along the screw. Two smooth guide rods would prevent the carriage from rotating or becoming unstable.

This approach appealed to us because it provided a clear way to convert rotational movement into vertical movement.

Our original idea was:

**Stepper Motor → Lead Screw → Moving Carriage → IR Sensor**

But when we checked the materials available in our robotics lab, we encountered our first major problem.

We did not have a T8 lead screw or two smooth 8 mm guide rods.

We did, however, have a long hexagonal metal rod that was approximately 6–7 mm thick.

At first, we wondered whether we could simply substitute this rod for the threaded shaft. After exploring how lead screws work, we realized that this would not be possible without modifying the mechanism. The hexagonal rod had no threads, meaning it could not move a nut up and down through rotation.

This was frustrating because it meant our original mechanical design would not work with the components we had.

Instead of buying an entirely new set of materials, we decided to investigate another approach.

### Design Documentation

**Image 1: Original Shaft-Based Design**

[Insert image here]

**Image 2: The Hexagonal Rod Available in Our Lab**

[Insert image here]

---

## 3. Changing Our Design: From Shaft to Belt

After realizing that our original mechanism would not be practical, we began exploring how belts and pulleys could produce vertical movement.

We wanted to achieve the same result as the lead screw system: moving the Sharp IR sensor up and down while keeping it stable.

Our revised concept used a timing belt driven by a stepper motor.

The motor would rotate a pulley at the bottom of the structure. The belt would run around this pulley and another pulley at the top, forming a continuous loop.

When the motor rotated, the belt would move. A carriage attached to one side of the belt would then travel vertically.

The Sharp IR sensor would be secured to this carriage rather than directly attached to the belt.

Our new mechanism was:

**Stepper Motor → Pulley → Timing Belt → Moving Carriage → Sharp IR Sensor**

One of the more difficult concepts for us was understanding how a motor spinning in circles could create upward and downward movement.

We eventually understood that the pulley does not need to change the direction of the motor's rotation. If the motor shaft is positioned horizontally, the pulley rotates in a vertical plane, moving the belt upward on one side and downward on the other.

This allowed us to think about the vertical structure in a completely different way.

We also explored the possibility of using our hexagonal rod as a guide for the moving carriage. The rod would help keep the sensor aligned while the belt provided the actual movement.

We investigated an HTD 350-5M timing belt, which introduced additional considerations such as pulley compatibility, belt length, and tension.

Although this design still required further development, it gave us a more realistic direction for the project.

### Design Documentation

**Image 3: Belt and Pulley Mechanism**

[Insert image here]

**Image 4: Vertical Structure CAD or Prototype**

[Insert image here]

### Design Reflection

What did we learn from changing the mechanism?

[Write reflection here]

---

## 4. Designing the Rotating Platform in Onshape

While working on the vertical mechanism, we also spent a significant amount of time developing the rotating platform.

This part was especially important because the object needs to remain stable while the motor rotates it through 360 degrees.

We originally thought that creating a circular platform would be one of the easiest parts of the project.

However, the connection between the platform and the stepper motor turned out to be much more complicated than we expected.

We used Onshape to create multiple versions of the platform. We experimented with different shapes, dimensions, and mounting structures.

### Our First Prototype

Our first CAD model was a basic circular platform that could be attached to the motor shaft.

Although the design looked reasonable in Onshape, we found that the platform was not sufficiently stable when connected to the motor.

The platform could wobble, which was a serious problem because an unstable object would make the scanner's measurements less consistent.

This made us realize that simply attaching a plate to a motor was not enough.

We needed to think about how the plate was supported, how the motor shaft connected to it, and whether the design could maintain its alignment during rotation.

**Image 5: First Rotating Platform CAD**

[Insert image here]

### Redesigning the Motor Connection

For our next version, we returned to Onshape and modified the mounting structure.

We experimented with different connections between the circular platform and the motor shaft.

Our main goal was to make the platform more secure and reduce unnecessary movement during rotation.

However, some of these redesigned versions still had stability problems.

This led to more adjustments.

We changed dimensions, reconsidered the geometry of the motor connection, and continued creating new CAD models.

**Image 6: Second Rotating Platform CAD**

[Insert image here]

### Further Prototypes and Improvements

After several iterations, we began paying more attention to the relationship between the physical motor and the CAD model.

We learned that even if a design appeared correct on a computer, there could still be problems when we attempted to assemble it.

For example, a connection might be too loose, slightly misaligned, or unable to support the rotating platform as expected.

We continued redesigning the platform to address these issues.

This became one of the most time-consuming parts of our project, but it also taught us a lot about the difference between digital modeling and physical engineering.

Rather than settling for a design that was unstable, we repeatedly went back to Onshape to improve it.

**Image 7: Third Rotating Platform CAD**

[Insert image here]

**Image 8: Later CAD Versions or Physical Prototypes**

[Insert image here]

### What We Learned

One of the biggest lessons from this stage was that a functioning mechanism needs more than just correct dimensions.

It also needs structural stability, proper alignment, and a reliable connection between components.

Our repeated CAD redesigns helped us develop a better understanding of mechanical engineering and the importance of testing prototypes in real life.

---

## 5. Building and Fabricating the Parts

Our team spent a considerable amount of time working in the robotics lab, experimenting with different components and trying to turn our designs into physical structures.

We used the resources available to us, including the laser cutter, various motors, mounting components, and other mechanical parts.

Many of our decisions depended on what materials and equipment we could find in the lab.

Instead of following a fixed list of purchased parts, we often had to adapt our original ideas to the resources we already had.

Some of our work involved designing or modifying motor mounts, experimenting with ways to support the rotating platform, and planning how the vertical carriage could be connected to the belt.

We also had to consider how different components would fit together.

A change to one part of the structure often required adjustments to several other parts.

For example, changing the motor's position could affect the pulley alignment and the location of the belt. Similarly, modifying the rotating platform sometimes required us to redesign its connection to the motor.

This made the fabrication process much more iterative than we initially expected.

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

### Fabrication Photos

**Image 9: Working in the Robotics Lab**

[Insert image here]

**Image 10: Laser-Cut or Fabricated Components**

[Insert image here]

**Image 11: Our Physical Prototype**

[Insert image here]

---

## 6. Arduino Programming and Motor Testing

Another major part of our project involved connecting the stepper motors to the Arduino and writing code to control them.

We used an Arduino Uno R3 and experimented with different motor setups.

Our goal was to independently control the motor responsible for the rotating platform and the motor responsible for the vertical movement mechanism.

However, at this stage, our Arduino program is still a very early prototype.

### What Our Current Code Does

Our current program is designed mainly to test whether the motors can rotate continuously.

We have experimented with running two stepper motors at different speeds.

One motor is intended for the rotating object platform, while the other is intended for the vertical belt mechanism.

**At the moment, our code simply keeps the motors spinning. It does not yet perform an automated 3D scanning sequence.**

The program is useful for checking:

- Whether the Arduino can communicate with the motor control hardware.
- Whether both motors respond to the code.
- Whether the motors can rotate continuously.
- How changing the programmed motor speed affects their movement.
- Whether our electronic connections work as expected.

This testing stage is important because we need to understand how the motors behave before connecting them to a complete scanning mechanism.

### Our Current Motor Setup

```text
Arduino Uno R3
      |
      |---- Motor Control 1
      |           |
      |      Stepper Motor 1
      |           |
      |      Continuous Rotation
      |
      |---- Motor Control 2
                  |
             Stepper Motor 2
                  |
             Continuous Rotation
```

The motors are being tested as independent components. We have not yet completed the programming needed to synchronize their movements with the IR sensor.

### What Our Code Cannot Do Yet

The current version does not:

1. Automatically stop the rotating platform after a precise 360-degree scanning cycle.
2. Move the IR sensor upward by a specific measured distance after each rotation.
3. Read and record Sharp IR sensor measurements as part of a coordinated scan.
4. Synchronize the sensor measurements with the platform's angular position.
5. Convert collected sensor data into a complete 3D model.

These are functions we hope to add as the project develops.

### Our Next Programming Goal

Eventually, we want the Arduino to follow a sequence similar to this:

```text
START
  |
  v
Set Sensor to Initial Height
  |
  v
Rotate Platform
  |
  v
Collect IR Distance Measurements
  |
  v
Complete One 360-Degree Rotation
  |
  v
Move Sensor Up by a Small Increment
  |
  v
Repeat Until Scanning Is Complete
  |
  v
Save Measurement Data
  |
  v
Process Data into a 3D Model
```

For now, this remains a proposed workflow rather than a completed program.

We believe it is important to document this distinction because our project is still under development.

### Arduino Documentation

**Image 12: Arduino Wiring and Motor Connections**

[Insert image here]

**Image 13: Screenshot of Our Arduino Code**

[Insert image here]

**Image 14: Motor Testing**

[Insert image or video link here]

### Programming Reflection

What problems did we encounter while connecting and testing the motors?

[Write reflection here]

---

## 7. Debugging and Problem-Solving

One of the most important parts of this project has been the amount of troubleshooting required.

Not everything worked the first time, and several parts of our design had to be reconsidered.

Some of our main challenges included finding suitable mechanical components, stabilizing the rotating platform, determining how to produce vertical movement, and establishing reliable connections between the Arduino and motors.

We often had to return to earlier stages of the project after discovering a problem.

For example, when the rotating platform was unstable, we could not simply solve the issue through programming. We needed to modify the physical design.

Similarly, when considering the vertical mechanism, we learned that the motor's rotation alone would not lift the sensor. We needed a suitable pulley and belt arrangement, along with a stable guide structure.

Our process involved testing, identifying problems, discussing possible solutions, and modifying our designs.

### Main Challenges

| Problem | What We Did | What We Learned |
|---|---|---|
| Missing lead screw | Explored belt-driven movement | Designs can be adapted |
| Missing guide rods | Investigated hexagonal rod | Material limitations affect design |
| Unstable rotating platform | Redesigned CAD models repeatedly | Stability requires careful engineering |
| Vertical movement mechanism | Studied pulleys and belts | Rotational motion can create linear motion |
| Motor configurations | Tested different setups | Motor placement affects performance |
| Arduino connections | Debugged electronics and code | Reliable wiring is essential |
| Incomplete scanning code | Focused on continuous motor testing | Complex programs should be developed in stages |

We gradually realized that the most valuable part of the project was not simply creating a final machine, but understanding why our designs worked or failed.

---

## 8. AI Usage and Our Own Contributions

We used ChatGPT throughout parts of our project as a resource for learning about mechanical design and exploring possible solutions.

We want to be transparent about its role.

ChatGPT helped us understand how different vertical movement mechanisms work, including lead screws, linear guide rods, timing belts, pulleys, and moving carriages.

It also provided conceptual diagrams and explanations that inspired some of our structural ideas.

One particularly useful contribution was helping us explore the possibility of replacing the original shaft-driven mechanism with a belt-driven system when we realized that we did not have the required materials.

AI helped us consider alternatives and understand the engineering principles behind them.

However, there is a significant difference between receiving an idea and implementing it in a physical machine.

The practical development of the project was carried out by our team.

We were responsible for designing and repeatedly modifying our CAD models, experimenting with motor configurations, connecting the Arduino and motors, developing and testing the Arduino code, and debugging the system.

We also independently used the robotics lab and its equipment, including the laser cutter, to create and modify components for our prototypes.

The decisions about which parts to use, how to assemble them, and how to respond when something did not work were made by our team.

Although ChatGPT provided useful inspiration, it could not determine whether a particular physical assembly would actually work without real testing.

We had to make those judgments ourselves through experimentation.

**Overall, AI supported our understanding and brainstorming, while the hands-on design, programming, fabrication, troubleshooting, and engineering decisions were completed by our team.**

We see AI as a useful tool for learning engineering concepts, but not as a replacement for the work required to build and debug a real system.

---

## 9. Where Our Project Stands Now

At this stage, our project is still in development.

We have established the main concept of a two-motor 3D scanner and explored the mechanical structures required for the rotating platform and vertical sensor movement.

We have also created several CAD prototypes, particularly for the rotating object holder, and learned from the stability problems we encountered.

On the electronics side, we have worked on connecting our stepper motors to Arduino and developing basic code to test continuous motor rotation.

However, we have not yet completed a fully functioning 3D scanning system.

Our next steps involve refining the mechanical structure, improving the motor-to-platform connection, testing the belt-driven carriage, and developing a more complete Arduino program that coordinates the motors with the sensor.

Eventually, we hope to collect meaningful distance measurements and reconstruct a digital 3D model of a physical object.

We still have several engineering problems to solve, but our progress so far has given us a much stronger understanding of the mechanical and electronic systems involved.

---

## 10. Personal Reflection

**Name:** [Your Name]

**Date:** [Date]

### What Was My Biggest Contribution?

[Write your personal reflection here]

### What Was the Most Difficult Problem I Faced?

[Write your personal reflection here]

### What Did I Learn From Repeatedly Redesigning Our Prototypes?

[Write your personal reflection here]

### How Did My Understanding of Engineering Change?

[Write your personal reflection here]

### What Would I Do Differently Next Time?

[Write your personal reflection here]

---

## 11. Final Thoughts

Looking back at our work so far, this project has been more complicated than we expected.

We began with a relatively straightforward idea: use two motors and a distance sensor to create a 3D scanner.

However, once we started working with actual components, we encountered problems involving mechanical stability, material availability, motor configurations, and electronic connections.

Our original shaft-driven design had to be reconsidered because we lacked the necessary parts. Our rotating platform required repeated CAD redesigns because several versions were unstable when mounted to the motor.

Even our Arduino code remains a basic testing program rather than a complete scanning system.

Despite these challenges, we have made progress in understanding how the components need to work together.

We have learned that engineering is not just about knowing how something should work in theory. It is also about making adjustments when the theory meets physical limitations.

For us, one of the most valuable lessons has been learning to approach problems through experimentation rather than expecting the first design to work perfectly.

**Our scanner is not finished yet, but the process of designing, testing, debugging, and improving it has already taught us a great deal about engineering.**

---

*End of Innovator's Journal*

