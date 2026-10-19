# 3D Scanner Project With Ryan

## Starting the Project

For this project, we were encouraged to do as much as we possibly could in terms of creating a 3D scanner. Before starting with stepper motors, we first visited the museum of our school to have a look at their 3D prototype. And it was surely fascinating to see that a real-life object could be described into 0s and 1s.  

Our goal was to work towards making a simple 3D scanner with Arduino, with learning about motion sensors, distance sensors, mini-motors, and stepper motors being the firststep. Our task for the few classes was like:

`Test Servo in terms of moving the sensor → making it stop at an angle and measure a distance  →change to stepper-motor  → make it move again`

Our final idea is to have an ultrasonic distance sensor move in more than one direction so it can collect distance measurements from different positions. Instead of trying to build the entire scanner at once, we decided to first get one part working.

## Step 1:
Ryan and I used ChatGPT with GPT-6 Astra to help develop the servo-and-ultrasonic scanner. We used it to generate starting code, explain unfamiliar functions, and understand how to make the movement and distance measurements happen in order. This would be the first step of everything. Firstly, however, the tools and materials we had to use were:

- Arduino Uno R3
- Ultrasonic distance sensor (HC-SR04)
- Servo motor
- Breadboard
- Jumper wires
- USB cable
- Computer with Arduino IDE

With a little prior experience for the maze, I was pretty familiar with the small mini-servos. I knew much of the code with a joystick handling the angle, but to make it automatic was one that required inquiry with AI. And so the new task was: **for every 15 degrees**, the sensor could move and take a reading at each position. 

## What We Asked AI

Our first request was about combining the servo movement with distance detection. A shortened version of the prompt was:

`How do I make a servo stop every 15 degrees and use a distance sensor to detect an object and scan it?`

AI suggested a **positional servo and an HC-SR04 ultrasonic sensor**. It generated a program that moved the servo from **15 to 165 degrees** in 15-degree intervals, took a measurement at each position, and then scanned back. The generated program included the movement loops, a function for reading distance, pauses between actions, and Serial output. This gave us a starting point for testing the sensor and servo together. After receiving the code, we asked follow-up questions about what the functions did, why the servo needed a delay, and how the sensor converted echo time into centimetres. Our questions became more focused as we worked through the program.

## Understanding How the Servo Works

The first part was about constructing the Arduino Board. The main template looked like this:

Servo Motor

| Servo Connection | Arduino Connection |
|---|---|
| Signal | Digital Pin 9 |
| Power | 5V |
| Ground | GND |

Ultrasonic Sensor

| Sensor Connection | Arduino Connection |
|---|---|
| VCC | 5V |
| GND | GND |
| Trig | Digital Pin 10 |
| Echo | Digital Pin 11 |

And for the code part, AI gave us this:

```cpp
#include <Servo.h>

Servo scannerServo;

const int servoPin = 9;
const int trigPin = 10;
const int echoPin = 11;

const int stepAngle = 15;
const int settleTime = 500;  // Wait before measuring, in milliseconds

float readDistance() {
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);

  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);

  unsigned long duration = pulseIn(echoPin, HIGH, 30000UL);

  if (duration == 0) return -1;  // No echo received

  float distance = duration / 58.0;

  if (distance < 2 || distance > 400) return -1;

  return distance;
}

void scanAtAngle(int angle) {
  scannerServo.write(angle);
  delay(settleTime);  // Let the servo reach and hold the angle

  float distance = readDistance();

  Serial.print(angle);
  Serial.print(",");

  if (distance < 0) {
    Serial.println("no_reading");
  } else {
    Serial.println(distance, 1);
  }

  delay(500);  // Keep holding before moving again
}

void setup() {
  Serial.begin(9600);

  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);
  digitalWrite(trigPin, LOW);

  scannerServo.attach(servoPin);
  scannerServo.write(90);
  delay(1000);

  Serial.println("angle_deg,distance_cm");
}

void loop() {
  // Scan forward: 15, 30, 45 ... 165 degrees
  for (int angle = 15; angle <= 165; angle += stepAngle) {
    scanAtAngle(angle);
  }

  // Scan backward, skipping endpoints already measured
  for (int angle = 165 - stepAngle; angle > 15; angle -= stepAngle) {
    scanAtAngle(angle);
  }
}
```
### 1. Setting Up the Servo and Sensor

```cpp
#include <Servo.h>

Servo scannerServo;

const int servoPin = 9;
const int trigPin = 10;
const int echoPin = 11;

const int stepAngle = 15;
const int settleTime = 500;
```

First, I imported the Servo library to control the servo motor. I then assigned Pin 9 to the servo motor and Pins 10 and 11 to the ultrasonic sensor. I also set `stepAngle` to 15, meaning the servo moves 15 degrees at a time. The movement of the servo would be:
```text
15°
30°
45°
60°
75°
...
165°
```
Moreover, the `settleTime` of 500 milliseconds gives the servo time to reach its position before measuring the distance.

### 2. Measuring Distance

```cpp
digitalWrite(trigPin, HIGH);
delayMicroseconds(10);
digitalWrite(trigPin, LOW);

unsigned long duration = pulseIn(echoPin, HIGH, 30000UL);

float distance = duration / 58.0;
```

The ultrasonic sensor sends out sound waves that bounce off nearby objects and return to the sensor. The Arduino measures how long the sound waves take to return using `pulseIn()`. It then divides the time by 58 to calculate the approximate distance in centimeters. For example, if the sound waves take 1160 microseconds to return, the object is about 20 cm away.

### 3. Moving the Servo Motor

```cpp
void scanAtAngle(int angle) {
  scannerServo.write(angle);
  delay(settleTime);

  float distance = readDistance();
```

I created a function called `scanAtAngle()` to move the servo and measure the distance. The `scannerServo.write(angle)` command moves the servo to a specific angle. After waiting 500 milliseconds, the Arduino calls `readDistance()` to measure how far away an object is.

### 4. Displaying the Measurements

```cpp
Serial.print(angle);
Serial.print(",");

if (distance < 0) {
  Serial.println("no_reading");
} else {
  Serial.println(distance, 1);
}
```

This section displays the angle and measured distance in the Serial Monitor. If the ultrasonic sensor cannot get a valid reading, it displays `no_reading` instead. For example, the output could look like this:

```text
angle_deg,distance_cm
15,34.2
30,28.6
45,21.3
60,15.7
```

This allows me to see the distance of an object at different angles.

### 5. Creating the Scanning Motion

```cpp
void loop() {
  for (int angle = 15; angle <= 165; angle += stepAngle) {
    scanAtAngle(angle);
  }

  for (int angle = 165 - stepAngle; angle > 15; angle -= stepAngle) {
    scanAtAngle(angle);
  }
}
```

Finally, I used two `for` loops to make the servo scan back and forth automatically. The first loop moves the servo from 15 to 165 degrees, increasing by 15 degrees each time. The second loop moves it backward from 150 (not 165 because it was 165 just a few seconds ago) to 30 degrees. After that, the process repeats continuously. This allows the ultrasonic sensor to scan different directions without manually controlling the servo.

Here, this is our final setup for the basic servo + ultrasonic distance sensor:

<img width="410" height="540" alt="93fe708cf5f01cc86b67540fb686ab30" src="https://github.com/user-attachments/assets/5e53eb87-e2c1-492c-afcd-d93bbabb872d" />
<img width="410" height="540" alt="ce49f9da8a1fa438a202cf5a1ccbbaed" src="https://github.com/user-attachments/assets/b2635698-0a25-4f9e-b16e-2c45e636c6f8" />

And after trying it out, Ryan and I got to this video with this progress:

<video width="700" controls style="max-width: 100%;">
<source src="videos/servo-scanner-test.mp4" type="video/mp4">
</video>

In the video, you could see a few numbers popping onto the Monitor screen. This is because the Arduino measures how long the echo takes to return and converts that time into centimeters.
The Serial Monitor gives us results like:
```text
15,35.2
30,30.8
45,23.6
```
For example:
```text
45,23.6
```
means the servo was around 45 degrees and the detected object was about 23.6 cm away.
This gives us two pieces of information for every measurement:
angle + distance, perfect for future 3D imprinting!

## Problems and Limits With Using AI

Surely enough the AI did its job, but it did not mean that the answers automatically worked with our physical project. A lot of times it wasn't the code that didn't work. Sometimes, the wires would be loose or sometimes the Serial Output would be different. Thus, we still had to compare the AI code's information with the tutorial and the actual components in front of us.

After all, AI did most of the initial coding for the servo scanner and helped us develop the combined scanner code, but Ryan and I still had to physically build the system, choose how we wanted it to move, test each component, compare the AI suggestions with the tutorial, and decide what changes actually worked.

The biggest thing we learned from using AI was that getting code from AI is different from having a working project. We still needed to understand enough of the code to test it, recognize when something did not match our hardware, and explain what each major part was doing.

## Adding the Stepper Motor

After getting the servo and distance sensor working, in the next class, Ryan and I moved on to the next part of our plan: **adding a stepper motor instead of a servo**.

The goal of the stepper motor is to eventually move the scanner in another direction while the servo changes the angle of the distance sensor. This should let us collect measurements from more positions instead of only sweeping side to side. It could turn 360 degrees all around! With this design, we wouldn't have to manually rotate the device again.

We had not worked with this type of stepper motor and driver before, so we followed the tutorial:

`Control a NEMA 17 Stepper Motor with A4988 Driver and Arduino - Full Guide`

The tutorial showed us how to connect the NEMA 17 stepper motor, A4988 motor driver, Arduino, and external power supply. It also told us how to be caution with handling heat and overburning of the motors. It told us to use a **multimeter** and change the amps of the electrical current and many more important minute details!

For this stage, the tutorial was our main source rather than AI. We followed the wiring shown in the video and stopped at different points to make sure we understood what each connection was doing before continuing.

## Stepper Motor Setup

One thing we learned was that the stepper motor is not controlled directly from the Arduino. The A4988 driver is between the Arduino and the motor.
The Arduino sends the driver instructions such as which direction to move and when to take a step. The driver handles the higher current needed by the stepper motor with a small metal piece and then gives the information, through **pairs** of wires, to the stepper-motor. Here, a very interesting thing is that for the **red, green, blue, and black** wires, they come in pairs. From the video, I learnt that to make the motor work, pairs should be stuck together properly. The video taught us a way to test which 2 come together, and that was a very new thing I learnt!

<img width="410" height="540" alt="1e07e59390015b27927fce45a7b183a8" src="https://github.com/user-attachments/assets/fea6032e-a2e3-46b5-8117-0685baaddb4f" />
<img width="410" height="540" alt="61abceb8b2d4e09d34a9bf9b8b7f6a9c" src="https://github.com/user-attachments/assets/d9cfdedf-a3e4-46d2-84a8-5f3d8db144c6" />

## Setting the Current Limit

As mentioned before in the previous part, before running the stepper motor, we needed to set the current limit on the A4988 driver. It was important to control the variable and to not make the motor go **explode**.

The small potentiometer on the driver controls how much current the motor is allowed to receive, so we used a multimeter while adjusting it instead of guessing.

This part took longer than we expected because we ran into a few problems. At first, the multimeter stopped working because its battery ran out. After replacing the battery, it still did not seem to give us the reading we expected, so we had to stop and check the setup again.
We checked the multimeter settings, where the probes were connected, and how we were measuring the driver. Once we got the multimeter working again, we continued adjusting the small potentiometer on the A4988 while checking the reading.

<video width="700" controls style="max-width: 100%;">
<source src="videos/stepper-current-limit.mp4" type="video/mp4">
</video>


## Setting up the final spin for the motor

At this point, we have started setting up the stepper motor and driver and have adjusted the current limit. Our next step is to get the Arduino controlling the stepper motor.

And after setting the current limit, Ryan and I connected the stepper motor to the A4988 driver.

The Arduino connects to the driver using two important control pins:
```text
DIR → Arduino Pin 2
STEP → Arduino Pin 3
```
`DIR` controls which direction the motor turns. `STEP` controls when the motor moves one step.
At this point, our goal was not to connect the stepper motor to the whole scanner yet. We first wanted to see if we could make the motor turn by itself.

For our first test, we copied the basic stepper motor code from the YouTube tutorial we were following.
```cpp
const int dirPin = 2;
const int stepPin = 3;
void setup() {
pinMode(dirPin, OUTPUT);
pinMode(stepPin, OUTPUT);
delay(2000);
digitalWrite(dirPin, HIGH);
}
void loop() {
digitalWrite(stepPin, HIGH);
delayMicroseconds(5000);
digitalWrite(stepPin, LOW);
delayMicroseconds(5000);
}
```
Even though we copied this starting code from the tutorial, we went through it so we understood what it was doing.
These lines:
```cpp
const int dirPin = 2;
const int stepPin = 3;
```
tell the Arduino which pins are connected to the `DIR` and `STEP` inputs on the motor driver.
In `setup()`, these lines:
```cpp
pinMode(dirPin, OUTPUT);
pinMode(stepPin, OUTPUT);
```
make both pins outputs because the Arduino is sending signals to the driver.
The line:
```cpp
digitalWrite(dirPin, HIGH);
```
sets the direction of the motor. Changing it from `HIGH` to `LOW` would make the motor turn in the opposite direction.
The most important part is:
```cpp
digitalWrite(stepPin, HIGH);
delayMicroseconds(5000);
digitalWrite(stepPin, LOW);
delayMicroseconds(5000);
```
The STEP pin repeatedly switches between HIGH and LOW. Each pulse tells the driver to move the stepper motor another step.
The delays also affect how quickly the pulses are sent. With a `5000` microsecond delay between changes, the motor turns relatively slowly. A shorter delay would send the pulses faster and make the motor turn faster.
This first test helped us separate the project into smaller pieces. Before trying to combine the stepper motor with the servo and ultrasonic sensor, we wanted to make sure the motor, driver, wiring, and basic code could work on their own.

## THE FINAL WORKING STEPPER MOTOR IS HERE

<video width="700" controls style="max-width: 100%;">
<source src="videos/stepper-motor-test.mp4" type="video/mp4">
</video>

## Reflection

In the meantime for creating this stepper motor, a lot of times the power setup was a little weird. The voltage and power needed for the stepper motor were much higher than what we were already using for the servo and sensor, and we were worried that connecting it incorrectly could damage the scanner or the Arduino. I actually received a small electrical shock at one point, and so we stopped working with the powered circuit instead of continuing to experiment with it after that incident.This made us realize that we needed to understand the power requirements and wiring more clearly before combining the stepper motor with the rest of the scanner. 


In conclusion, we were not able to fully combine the stepper motor with the scanner before the end of the project because we ran into problems with the power setup. Time was not really enough. Instead of rushing the final connection, we stopped when we were unsure that the setup was safe.
Even though the final version with both motors was not completed, we still moved from an idea to a working scanner prototype and then began developing a more advanced version. It was a great journey analysing and discussing with my peers about the motors and the servos. Learning with technologies like Youtube Videos or AI was pretty interesting, and I wish we could carry on this good progress some day!
