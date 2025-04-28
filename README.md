# Bricklayer Bot

![Bricklayer bot](https://github.com/user-attachments/assets/b2dc0e25-17c9-471a-806e-dd145547422d)

This robot program uses a 3d printed chute to carry and lay bricks along a line.

## Components

1. 3D printed chute
Check out the 3D model [on onshape](https://cad.onshape.com/documents/ce2139a7720a6f7f91b6e3fd/w/4e778bed12d5593772b55c33/e/0c2e88e45d036e104e664543) ![image](https://github.com/user-attachments/assets/862918f4-37ff-4838-a7cd-4d48b7848d49)

2. Linefollower
![Linefollowing](https://github.com/user-attachments/assets/60f61450-b8f4-426b-9af7-fa481355d085)
The robot uses an IR sensor on the bottom to scan for a black line.
I implemented a PD controller in the microcontroller software to make it steer and follow the line.

3. Bump sensor 
![Bump switches](https://github.com/user-attachments/assets/40c394de-c5e7-4569-bc76-599cce542beb)
These bump sensors interrupt the line follower and indicate when it's time to place the brick.

4. Servo
![servo](https://github.com/user-attachments/assets/463d0d2a-44d1-43b1-97e2-c7225cf37c10)
The servo motor holds the brick up on the chute and releases it during the bricklaying sequence.

5. Motors
As seen in the other pictures, this robot has wheels. The computer program uses MSP432's TimerA module to send a PWM control to the motor and change the speed and direction, for steering and turning.
It can tank-turn in place, which is useful for turning around on the line while staying on the line.

6. LEDs
![LED](https://github.com/user-attachments/assets/6fe27869-855a-4ac0-9035-502eb64f9c63)
The bricklayer robot indicates status and error states with an LED on the board. White means it ran off the line while carrying a brick and it doesn't know where to place it. Red means it arrived at a place to place a brick but it doesn't have a brick. Yellow means it has hit a bump switch.

## State diagram
![state diagram](https://github.com/user-attachments/assets/06184c4a-4f84-4c36-a011-a03efdf56f1f)
1. The program starts by raising the chute-blocking servo and requesting a brick. Once a brick is placed into the chute,
2. the robot moves on to linefollowing. It carries the brick until it hits the brick it has placed previously.
3. The bump switch interrupts the linefollowing subroutine and changes the state to placing the brick. This involves moving the servo to open the chute and driving backwards 10 inches (the brick is 6 inches square and the robot needs room to turn around after placing).
4. The next action is to turn around 180 degrees in place with a tank turn.
5. The robot linefollows again in the opposite direction along the black line. At the end of the line all IR sensors report white and
6. the robot poses for the brick. This involves turning around 180 degrees to face the start of the line, then returning to the beginning of the program.

# Source
Because the software was written for a course assignment, the source code is not available to prevent cheating. If you are not a student of CPET 253, you can email me smm9509@rit.edu and I will likely share my source code. The source code contains modified copies of BSD code from the textbook for the course, this is the attribution written by the author: 
```c
/* This example accompanies the book
   "Embedded Systems: Introduction to Robotics,
   Jonathan W. Valvano, ISBN: 9781074544300, copyright (c) 2019
 For more information about my classes, my research, and my books, see
 http://users.ece.utexas.edu/~valvano/

Simplified BSD License (FreeBSD License)
Copyright (c) 2019, Jonathan Valvano, All rights reserved. */```
The source code from this project also contains unlicensed code from the templates provided by the professors for my course, which complicates licensing and redistribution.
