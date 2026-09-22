# Joystick-Controlled Arduino Maze

## Project Inspiration

For this project, I wanted to build on my existing knowledge of Arduino, breadboards, digital and analog inputs, and basic coding. I had seen many cool videos online where people used a joystick to control a tilting maze, and I wanted to see if I could replicate the same idea using the skills I had already learned.

My goal was to create a maze controlled by a joystick. Moving the joystick left and right would control one servo motor, while moving it up and down would control another. By combining the movement of the two servos, I could tilt the maze in different directions and guide a ball through it.

---

## Setting Up the Joystick and Servos

The first step was getting the joystick to communicate with the Arduino and using its values to control two servo motors.

My joystick had five connections:

- **GND → Arduino GND**
- **+5V → Arduino 5V**
- **VRx → A0**
- **VRy → A1**
- **SW → Digital Pin 2**

`VRx` measures the horizontal movement of the joystick, while `VRy` measures its vertical movement. Since these are analog inputs, the Arduino reads values from approximately **0 to 1023**.

I also connected the Arduino's **5V and GND pins to the positive and negative rails of the breadboard**. This allowed the breadboard to distribute power to the components connected to it.

For the two servos, their control wires were connected to:

- **X-axis servo signal → Digital Pin 9**
- **Y-axis servo signal → Digital Pin 10**

The servo power and ground connections were connected through the breadboard power rails.

<!-- INSERT CIRCUIT / BLUEPRINT IMAGE HERE -->

![Circuit Diagram](images/76961c43-f83f-423f-a8e1-35f5d990b0ac.png)

---

## Programming the Joystick

I first included Arduino's Servo library so that the Arduino could control the angle of each servo.

```cpp
#include <Servo.h>

Servo xServo;
Servo yServo;
