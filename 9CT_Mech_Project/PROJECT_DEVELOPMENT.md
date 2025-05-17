##### markdownguide.org/basic-syntax/

# 9CT Assessment Task 1
### By Yuna Shin

## __Requirments Outline__
### __Defining the Purpose__
#### Write 2-3 sentences explaining what your robot is deisgned to do in the challenge:
I need to design a program for the EV3 robot to navigate around the green and blue blocks using the colour sensor and ultrasonic sensor while picking up or moving the yellow and red blocks.

### __Identify Key Actions__
#### List at least 3-5 specific actions your robot needs to perform to complete the task. These should be clear, simple actions like moving, turning, detecting something or triggering an event (e.g. sound or lights):
1. Detecting the green, blue, red and yellow blocks
2. Turning 90° or going around when green and blue blocks is detected
3. Put it into the "cage" if red and yellow blocks is detected
4. Returning the red and yellow blocks to the starting "box"

### __Functional Requirements__
#### A functional requirement is the action or bhevious the robot needs to perform. For each key action, write a functional requirement that clearly states what the robot must do. These should be written in a clear and concise manner:
*Object Detection* : The robot must stop when the ultrasonic sensor detects an object within 10cm and detect the colour

*Green/Blue Detection* : The robot must turn 90° or go around the object if the colour sensor detects that it is green or blue

*Yellow/Red Detections* : The robot must move towards the objects if the colour sensor detects that it is yellow or red and put it into the "cage"

### __Use Cases__
#### A use case describes a specific situation or scanerio where the robot will perform that action. Write a use case for each functional requirement describing a scenario where the robot will perform the required action.
*Scenario 1* : The robot is navigating a path and encounters an obstacle  
*Inputs* : The ultrasonic sensor detects an object within 10 cm  
*Inputs* : The colour sensor detects the colour as red or yellow  
*Action* : The robot puts it into the "cage" 
*Expected Outcome* : The robot picks it up and returns the block to the starting "box"

*Scenario 2* : The robot is navigating a path and encounters an obstacle  
*Inputs* : The ultrasonic sensor detects an object within 10cm  
*Inputs* : The colour sensor detects the colour as green or blue  
*Action* : The robot turn 90° to avoid the obstacle  
*Expected Outcome* : The robot avoids the obstacle and continues its path

### __Test Cases__
#### For each use case, develop a test case for it:
* Identify the inputs (e.g. sensor, data, button presses)  
* Identify the expected outcomes (e.g. motor movement, sound, display messages)  

| Test Case | Input     | Expected Output  |
|---------- |---------- |----------------  |
|Avoids Obstacle|Ultrasonic Sensor detects < 10cm|Robot detects the colour|
|Detects Colour|Colour Sensor detects red or yellow|Robots puts it into the "cage"|
|           |Colour Sensor detects green or blue|Robots stops and turns 90°|

### __Non-Functional Requirements__
#### These are requirements that focus on how well the robot will perform in completing the task, rather than the completing of the task itself. Consider the following:
* Efficieny - It should be able to complete it within minutes
* Response Time - The robot should detect the obstacle and its colour within 1 second
* Accuracy  - When putting it into the "cage" it should not miss or move backwards to lose the block

### __Test Case__ 
#### Number 1
This is for the ultrasonic sensor to detect obstacles and move around them.  
First try:
```
obstacle_sensor = UltrasonicSensor(Port.S4)  
robot = DriveBase(left_motor, right_motor, wheel_diameter=55.5, axle_track=104)  
ev3.speaker.beep()

while True:   
robot.drive(200, 0)  
    if obstacle_sensor.distance() < 150:  
        wait(10)  
        robot.straight(-100)  
        robot.turn(90)  
        robot.straight(150)  
        robot.turn(-90)  
```
Improvements:  
The robot kept getting too close to the obstacles before detecting them ...

Improved Version:
```
obstacle_sensor = UltrasonicSensor(Port.S4)  
robot = DriveBase(left_motor, right_motor, wheel_diameter=55.5, axle_track=104)  
ev3.speaker.beep()

while True:   
robot.drive(200, 0)  
    if obstacle_sensor.distance() < 300:  
        wait(10)  
        robot.straight(-200)  
        robot.turn(90)  
        robot.straight(300)  
        robot.turn(-90)  
```

#### Number 2
This is for the colour sensor to detect the blue and green obstacles.  
First try:
```
obstacle_sensor = UltrasonicSensor(Port.S4)
colour_sensor = ColorSensor(Port.S3)
left_motor = Motor(Port.B)
right_motor = Motor(Port.C)
robot = DriveBase(left_motor, right_motor, wheel_diameter=55.5, axle_track=104)
ev3.speaker.beep()

while True:
    robot.drive(200, 0)

    if obstacle_sensor.distance() < 300:
        wait(10)
        if colour_sensor.color() == Color.BLUE:
            ev3.speaker.beep()
            ev3.speaker.beep()
            break
        elif colour_sensor.color() == Color.GREEN:
            ev3.speaker.beep()
            ev3.speaker.beep()
            ev3.speaker.beep()
            break
        else:
            ev3. speaker.beep()
```

Improvements:  
This didnt work and kept on just beeping once saying that it was only able to detect and obstacle but no the correct colour. The Color.Green or Color.BLUE will probably be the problem as it is the result the robot returns to us and we acidentally used that as our actual code. We can try to shorten the length in whioch the ultrasonic sensor detects the obstalce to try and get the colour sensor to detect it better at a shorter distance.

1. use the youtube video and change it to detect colour and create a new variable but it doesnt work
2. change the detect colour to just colour thinking it was a code made by python but we had to define it so it prob dont work



Improved Version:  
```
from pybricks.hubs import EV3Brick
from pybricks.ev3devices import (Motor, TouchSensor, ColorSensor,InfraredSensor, UltrasonicSensor, GyroSensor)
from pybricks.parameters import Port, Stop, Direction, Button, Color
from pybricks.tools import wait, StopWatch, DataLog
from pybricks.robotics import DriveBase
from pybricks.media.ev3dev import SoundFile, ImageFile


# This program requires LEGO EV3 MicroPython v2.0 or higher.
# Click "Open user guide" on the EV3 extension tab for more information.
# Create your objects here.
ev3 = EV3Brick()
obstacle_sensor = UltrasonicSensor(Port.S4)
colour_sensor = ColorSensor(Port.S3)
left_motor = Motor(Port.B)
right_motor = Motor(Port.C)
robot = DriveBase(left_motor, right_motor, wheel_diameter=55.5, axle_track=104)


# Write your program here.
ev3.speaker.b
while True:
   colour = colour_sensor.color()
   robot.drive(200, 0)


    #if obstacle_sensor.distance() < 300:
      #wait(3)
   if colour == Color.BLUE:
      ev3.speaker.beep()
      ev3.speaker.beep()
      break
   if colour == Color.GREEN:
      ev3.speaker.beep()
      ev3.speaker.beep()
      ev3.speaker.beep()
      break
   #else:
      #ev3.speaker.beep()
      #break
```

Imrovements:  
We got help from Mr Scott and he helped us get the colour sensor to detect the colours of the blocks by getting rid of all the stuff around it and only focusing on judt detecting the colour. The problem in our code was not our colour sensor detection but:  
1. the robot was in light detecting settings.
2. our else statement was messing with the code as if it detected a different colour, it just ended because of the break.  

Now we are going to use this code and put it into our program.


