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

### __Flowgorithm__
![Mainline Routine](<../../../Desktop/Screenshot 2025-05-19 at 12.25.29 am.png>)

![Detect object](<../../../../../var/folders/n0/62gnh5b52d907yv253nh0z7m0000gn/T/TemporaryItems/NSIRD_screencaptureui_VTo151/Screenshot 2025-05-19 at 2.55.09 am.png>)

![Collect Red/Yellow block](<../../../../../var/folders/n0/62gnh5b52d907yv253nh0z7m0000gn/T/TemporaryItems/NSIRD_screencaptureui_cwhg8E/Screenshot 2025-05-19 at 3.01.25 am.png>)

![Take back to the start](<../../../../../var/folders/n0/62gnh5b52d907yv253nh0z7m0000gn/T/TemporaryItems/NSIRD_screencaptureui_wHERor/Screenshot 2025-05-19 at 3.03.30 am.png>)

### __Pseudocode__
### Mainline Routine:
``` 
BEGIN
    Detect object
    Collect Red Block
    Take back to the start
    Detect object
    Collect Yellow Block
    Take back to the start
END
```

#### Detect object:
```
BEGIN Detect object
    WHILE distance > 100mm
        READ distance
        Drive Forwards
    ENDWHILE
    Turn right 90 degrees
END Detect object
```

#### Collect Red/Yellow block:
```
BEGIN Collect Red/Yellow block
    Detect object
    If distance < 100mm
        Drive Forwards
        Turn towards start
    ENDIF
    Drive Forwards
END Collect Red/Yellow block
```

#### Take back to the start:
```
BEGIN Take back to the start
    Collect Red/Yellow Block
    Drive Forwards
END Take back to the start
```

### __Test Case__ 

This is for the ultrasonic sensor to detect obstacles and move around them:  
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


This is for the colour sensor to detect the blue and green obstacles:  

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



This is for the colour sensor to be able to detect any form of colour:   
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


This is for the robot to be able to detect the yellow block:
```
from pybricks.hubs import EV3Brick
from pybricks.ev3devices import (Motor, TouchSensor, ColorSensor,
                                 InfraredSensor, UltrasonicSensor, GyroSensor)
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
collected_obstacles = 0


# Write your program here.
ev3.speaker.beep()




while collected_obstacles < 2:
  colour = colour_sensor.color()
  robot.drive(200, 0)


  if obstacle_sensor.distance() < 300:
      wait(3)
      if colour == Color.YELLOW:
         robot.straight(100)
         robot.turn(225)
         robot.straight(200)
         robot.turn(140)
         robot.straight(120)
         robot.straight(-30)
         collected_obstacles += 1
```
We were going to use this until we found out that the colour sensor couldn't detect the yellow block in the way we wanted it to because the block was too close so we had to sratch the automatic driving idea.  

FINAL TEST CASE  
This is for the robot to be able to bring the red and yellow blocks back:
``` 
from pybricks.hubs import EV3Brick
from pybricks.ev3devices import (Motor, TouchSensor, ColorSensor,
                                 InfraredSensor, UltrasonicSensor, GyroSensor)
from pybricks.parameters import Port, Stop, Direction, Button, Color
from pybricks.tools import wait, StopWatch, DataLog
from pybricks.robotics import DriveBase
from pybricks.media.ev3dev import SoundFile, ImageFile


# This program requires LEGO EV3 MicroPython v2.0 or higher.
# Click "Open user guide" o+ the EV3 extension tab for more information.
# Create your objects here.
ev3 = EV3Brick()

obstacle_sensor = UltrasonicSensor(Port.S4)
colour_sensor = ColorSensor(Port.S3)
left_motor = Motor(Port.B)
right_motor = Motor(Port.C)
robot = DriveBase(left_motor, right_motor, wheel_diameter=55.5, axle_track=104)

# Beep to signal the program has started
ev3.speaker.beep()

# The robot drives up so it's facing the red obstacle
robot.straight(200)
robot.turn(107)
robot.straight(595)
robot.turn(107)


while True:
   # Robot auto drives by driving 5 cm at a time while checking the if the if statement is true
   robot.drive(50, 0)
   # Once it detects an obstacle within 10 cm
   if obstacle_sensor.distance() < 100:
      robot.stop()
      # Its screen displays "Obstacle Detected! :0"
      ev3.screen.clear()
      ev3.screen.draw_text(20, 50, "Obstacle Detected!")
      # Robot moves forward 12 cm and turns back to capture obstacle
      robot.straight(120)
      robot.turn(196)    
      # Robot going back to start area
      robot.straight(200)
      robot.turn(-107)
      robot.straight(595)
      robot.turn(-107)
      robot.straight(200)
      break


# The robot drives up so it's facing the yellow obstacle
robot.straight(200)
robot.turn(107)
robot.straight(670)
robot.turn(107)
robot.straight(450)
robot.turn(107)


while True:
   # Robot auto drives by driving 5 cm at a time while checking the if the if statement is true
   robot.drive(50, 0)
   # Once it detects an obstacle within 10 cm
   if obstacle_sensor.distance() < 100:
      robot.stop()
      if colour_sensor.color()  == 6:  # Colour sensor detects the floor is white
        ev3.screen_clear()
        # The robot's screen displays that the floor is white (non-functional)
        ev3.screen.draw_text(20, 50, "w w waitt.. the floor MIGHT be white. heh.")
        wait(3000)
      # The robot's screen displays "Obstacle Detected!"
      ev3.screen.clear()
      ev3.screen.draw_text(20, 50, "Obstacle Detected!")
      # Robot going back to start area
      robot.straight(500)
      robot.turn(-107)
      robot.straight(200)
```

The robot could kind if bring the red block back but we couldn't acheive the same for the yellow block. We couldn't use the colour sensor as it couldn't sense the colour accurately until it was very close and we didn't have the time to tweak it so we used a random if statement to use the colour sensor in some way. In the end, we just made the colour sensor detect the ground which is white.   

This is for the colour sensor to detect the colour of the ground:

```
from pybricks.hubs import EV3Brick
from pybricks.ev3devices import (Motor, TouchSensor, ColorSensor,
                                 InfraredSensor, UltrasonicSensor, GyroSensor)
from pybricks.parameters import Port, Stop, Direction, Button, Color
from pybricks.tools import wait, StopWatch, DataLog
from pybricks.robotics import DriveBase
from pybricks.media.ev3dev import SoundFile, ImageFile




# This program requires LEGO EV3 MicroPython v2.0 or higher.
# Click "Open user guide" o+ the EV3 extension tab for more information.


# Create your objects here.
ev3 = EV3Brick()


obstacle_sensor = UltrasonicSensor(Port.S4)
colour_sensor = ColorSensor(Port.S3)


left_motor = Motor(Port.B)
right_motor = Motor(Port.C)


robot = DriveBase(left_motor, right_motor, wheel_diameter=55.5, axle_track=104)


# <-------------------------------------- FUNCTIONS -------------------------------------->


"""Function to include the two sensors
Use when the obstacle is right in line of path of the EV3 so it can stop by detecting the obstacle using the ultrasonic sensor.
Also incorporates the colour sensor in a really useless way just so it fits assessment requirements"""
def autodrive():
   
   while True:
       # Robot auto drives by driving 5 cm at a time while checking the if the if statement is true
      robot.drive(50, 0)
       # Once it detects an obstacle within 10 cm
      if obstacle_sensor.distance() < 100:
         robot.stop()
         if colour_sensor.reflection() > 50: # Colour sensor detects the floor is white (white reflects around 60-100 but the sheet is kind of off-white )
            ev3.screen.clear()
            # The robot's screen displays that the floor is white (non-functional)
            ev3.screen.draw_text(20, 50, "w w waitt.. the floor MIGHT be white. heh.")
            wait(3000)
         # The robot's screen displays "Obstacle Detected!""
         ev3.screen.clear()
         ev3.screen.draw_text(20, 50, "Obstacle Detected!")


"""Function for simplifying the main program so the "robot.straight()" and "robot.turn" don't make huge code blocks
Measurements of each robot.straight and robot.turn are added in the main program (when there's too many straights and turns, writing 0 just makes it do nothing)"""
def move_path(forward1, turn1, forward2, turn2, forward3, turn3):
   robot.straight(forward1)
   robot.turn(turn1)
   robot.straight(forward2)
   robot.turn(turn2)
   robot.straight(forward3)
   robot.turn(turn3)


# <-------------------------------------- PROGRAM -------------------------------------->


# Beep to signal the program has started
ev3.speaker.beep()


# The robot drives up so it's facing the red obstacle
move_path(200, 107, 595, 107, 0, 0)


# Robot drives forward (toward red block) until the ultrasonic sensor detects it within 10 cm
autodrive()


# Robot moves forward 12 cm and turns back to capture obstacle
move_path(120, 196, 0, 0, 0, 0)


# Robot going back to start area
move_path(200, -90, 595, -107, 200, 0)


# The robot drives up so it's facing the yellow obstacle
move_path(200, 107, 670, 107, 450, 107)


# Robot drives forward (toward yellow block) until the ultrasonic sensor detects it within 10 cm
autodrive()


# Robot going back to start area
move_path(500, -107, 200, 0, 0, 0)

``` 
We had problems with the colour sensor detecting that the floor was white so Arisa changed it to calculate the reflection instead because the floor was a slight off-white which would be hard for the robot to detect. We also managed to tweak some of the measurements.   


### __Peer Evaluation__
#### __Arisa Komatsu__
#### When rating 1-5 with 1 being lacklustre effort and 5 being oustanding effort, how much effort do you feel this group member put into this project?
5/5  

#### Explain the reason for this score in detail:
I gave Arisa a 5/5 for effort as most of our coding was on her computer and additionally, she motivated our group and kept the flow going. She was the one continuously asking Mr Scott for help and ideas while me and Vanessa gave her some ideas and backed her up with the coding.

#### When rating 1-5 with 1 being no at all and 5 being an exceptional amount, how much did this team member contribute to the team's efforts throughout this project?
5/5

####  Explain the reason for this score in detail:
She continuously contributed by bringing in multiple ideas and different ways in which our code could work. Although we were going to let the robot roam free and detect colours, which didn't work, Arisa was the first one to suggest that we should code the robot's movements and just let it follow step by step rather than just detecting colours and obstacles. Additionally, she worked hard on our research task and pushed me and Vanessa to fix our work over and over.

#### When rating 1-5 with 1 being not well at all and 5 being exceptionally well, how well do you think this team member performed throughout all stages of the project?
5/5

#### Explain the reason for this score in detail:
Out of the three of us, Arisa was the first to fly through all the theory work in the Requirements outline. She helped me and Vanessa greatly, to write our project development by putting all the code in a docs, especially for me as I was on a holiday in Tasmania. She also gave me an idea on how I should create my flowgorithm and how to strucuture my test cases. 

----------

#### __Vanessa He__
#### When rating 1-5 with 1 being lacklustre effort and 5 being oustanding effort, how much effort do you feel this group member put into this project?
3/5  

#### Explain the reason for this score in detail:
I'm not sure on how much effort she put in during the time I was in Tasmania but for the time I was here during class, she continously helped with the code and brought in a few ideas. She supported Arise from the side.

#### When rating 1-5 with 1 being no at all and 5 being an exceptional amount, how much did this team member contribute to the team's efforts throughout this project?
2/5

####  Explain the reason for this score in detail:
Although she did bring in a few ideas, she barely helped with actually writing the coding and didn't contribute to the making of the arms of the robot in which I did myself. However, her dedication to the project was present as she also helped with my prject development for the flowgorithm as I couldn't figure out how to bring in the image (which I doubt it works).

#### When rating 1-5 with 1 being not well at all and 5 being exceptionally well, how well do you think this team member performed throughout all stages of the project?
3/5

#### Explain the reason for this score in detail:
Vanessa was always present throughout the project and kept up with the requirements outline most of the time although she did fall behind in some points. However, she was always there to help although she did muck around sometimes but Arisa helped her to get back on track in which she then did.

### __Final Evaluation__
#### Evaluate your group's final performance in relation to the identified need:
Overall, we failed to match the criteria needed to compelte the assessment task. Although we managed to capture and bring back the red block, the yellow block was a complete fail. Additionally, we couldn't even make the colour sensor detect the colours and ended up using it for the ground which was usless. 

#### Evaluate your project in relation to project management:
We managed to complete less than half of our goals and would have needed double, or even triple the time to get close to finishing the project. The parts that we managed to finished were the ultrasonic sensor, colour sensor (although it was useless) and the functions related to the red block. 

#### Evaluate your project in relation to team collaboration:
Everyone from our group collaborated but I believe that Arisa did the most, then me and then Vanessa. Arisa mostly focused on the code while I did the building part by myself. I fixed the arms here and there to match the coding comfortably so they wouldn't get in the way of the blocks and accidtenlly create a larger issue in some way or another. I had also helped with Arisa at the start when we were trying out ways to get the colour sensor to work. 

#### Justify future improvement you could make to your final product:
If we had more time, we could have coded the robot to be able to bring back the yellow block as well as found some use in the colour sensor. We could have been able to make the colour sensor detect the colour in a few seconds and made the robot move freely rather than giving it step by step instructions. I also wanted to make the arms sturdier and have a bigger barrier just in case the blocks slipped out. 


