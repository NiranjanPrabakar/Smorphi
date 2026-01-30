Smorphi Robot Ball Kicker
A robot control program for the Smorphi modular robot platform that uses Pixy2 camera vision to detect, track, and kick a colored ball.
Overview
This project uses a Smorphi robot equipped with a Pixy2 camera and IR sensors to autonomously track and kick a ball. The robot:

Detects colored objects using the Pixy2 camera
Tracks the object's position to stay aligned
Uses IR sensors to detect when the ball is close
Performs a kicking motion by rotating right when in range

Hardware Requirements

Smorphi Robot - Modular robot platform
Pixy2 Camera (ICSP) - For color-based object detection
IR Sensor - Connected to Module 2, Port 4 for proximity detection
ESP32 - Microcontroller board

Software Requirements

Arduino IDE (or compatible IDE)
Smorphi Library - Robot control library
Pixy2ICSP_ESP32 Library - Pixy2 camera interface for ESP32

Installation

Install the Arduino IDE
Install required libraries:

   - Smorphi library
   - Pixy2ICSP_ESP32 library

Connect your Smorphi robot to your computer via USB
Upload the code to your ESP32

Configuration
Pixy2 Camera Setup

Configure the Pixy2 camera to recognize your target object as signature 1
Use PixyMon software to train the camera on your colored ball
Mount the camera facing forward on the robot

IR Sensor

IR sensor should be connected to Module 2, Port 4
Positioned to detect objects directly in front of the robot

How It Works
Detection Logic

Pixy2 Vision: Continuously scans for objects matching signature 1
Position Tracking: Monitors x-coordinate (0-315) to keep object centered
Proximity Detection: IR sensor triggers kicking action when object is close

Movement Algorithm
ConditionActionPurposex >= 190Move forwardObject too far right, adjust positionx <= 50Move backwardObject too far left, adjust position140 < x < 145Move leftObject centered, approach itIR sensor triggeredStop & rotate rightKick the ball
Kicking Sequence
When the IR sensor detects an object:

Robot stops
Pivots right at speed 150
Holds rotation for 3 seconds
Resumes tracking

Code Structure
cppsetup()
- Initialize serial communication (115200 baud)
- Initialize Smorphi robot
- Initialize Pixy2 camera

loop()
- Read IR sensor status
- Get detected blocks from Pixy2
- Extract object properties (signature, x, y, width, height)
- Execute movement logic based on object position
- Perform kick when object is in range
Tuning Parameters
You can adjust these values for better performance:

X-coordinate thresholds: Currently 190 and 50 - adjust based on your field size
Centering range: Currently 140-145 - tighten or widen for precision
Movement speed: Currently 10 - increase for faster response
Kick rotation speed: Currently 150 - adjust kicking power
Kick duration: Currently 3000ms - modify kicking time

Troubleshooting
Robot doesn't detect the ball:

Verify Pixy2 is trained on signature 1
Check camera mounting and field of view
Ensure adequate lighting

Robot doesn't kick:

Verify IR sensor connection to Module 2, Port 4
Check sensor detection range
Test sensor with Serial.println(front_sensor_status)

Erratic movement:

Verify power supply is adequate
Check motor connections
Adjust movement speed parameters

Serial Monitor Output
The program outputs the following data at 115200 baud:

Detected block information (via print())
X-coordinate
Y-coordinate
Object width
Object height
