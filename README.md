# IoT Flood Monitoring and Early-Warning System

A low-cost, real-time edge IoT node that monitors water level and raises local visual and audible alarms. It uses an Arduino UNO Q, an HC-SR04 ultrasonic sensor and a liquid float switch for redundant detection, with live telemetry on an SSD1306 OLED and a buzzer for three alert stages: NORMAL, WARNING and CRITICAL.

This project builds on the design in the IEEE paper cited at the bottom of this file.

Table of Contents
Problem Statement
Objectives
Features
System Architecture
Components Required
Circuit and Wiring
Alert Logic
Getting Started
Results
Comparison with the IEEE Reference Design
Future Scope
Project Structure
Reference
License
Problem Statement

Flooding is one of the most frequent and devastating natural disasters worldwide, accounting for over 41% of all recorded natural calamities. Developing nations and vulnerable communities often suffer severe damage to human life, agriculture and urban infrastructure because they lack affordable, localized early-warning mechanisms near major water bodies, rivers and drainage networks.

Traditional flood monitoring setups can be expensive to deploy, complex to maintain, or overly reliant on distant weather forecasting models that fail to capture localized flash floods. There is an urgent need for a low-cost, real-time, highly reliable edge IoT monitoring system that continuously tracks local water levels, provides redundant hardware verification during critical overflows, and instantly alerts on-site personnel and local residents through immediate visual and audible alarms.

Objectives
Real-time edge monitoring: Design and deploy an IoT sensor node on the Arduino UNO Q platform to continuously measure water level without physical contact.
Dual-sensor redundancy: Combine high-precision distance measurement (HC-SR04 ultrasonic sensor) with a physical secondary limit mechanism (liquid float switch) to prevent false positives and guarantee alert execution during critical overflows.
Local visual and acoustic alerting: Show real-time telemetry on a 0.96" SSD1306 OLED display and trigger multi-stage alarms on a 5V active buzzer based on defined safety thresholds (NORMAL, WARNING, CRITICAL).
Autonomous high-speed edge processing: Use the dual-brain architecture of the Arduino UNO Q (STM32 MCU for real-time sensor polling, Qualcomm MPU for Linux-based data processing) for low-latency operation.
Features
Contactless water-level measurement (HC-SR04, 2 cm to 400 cm range)
Independent float-switch check that confirms critical overflow
Three-stage alert logic with on-site OLED status and buzzer alarm
Low-cost, off-the-shelf parts that are easy to prototype on a breadboard
Autonomous operation with no operator needed to raise a local alert

System Architecture
<img width="601" height="225" alt="image" src="https://github.com/user-attachments/assets/80b34de6-ce78-46df-a5c2-b13d3ebab62f" />

Components Required

Category	Component	Model / Specification
Microcontroller	Arduino UNO Q	4 GB RAM / 32 GB eMMC
Primary sensor	Ultrasonic sensor	HC-SR04 (2 cm – 400 cm)
Secondary sensor	Liquid float switch	Vertical / horizontal switch
Visual display	OLED display module	0.96" SSD1306 (128×64, I²C)
Acoustic actuator	Active buzzer	5V active sounder module
Power supply	USB-C adapter / cable	5V DC / 2A
Prototyping	Jumpers and breadboard	Male-to-female / male-to-male
Circuit and Wiring

Module	Signal	UNO Q pin
HC-SR04	VCC / GND	5V / GND
HC-SR04	TRIG	[pin]
HC-SR04	ECHO	[pin]
Float switch	Signal / GND	[pin] / GND
SSD1306 OLED	VCC / GND	[3.3V or 5V] / GND
SSD1306 OLED	SDA / SCL	[pin] / [pin]
Active buzzer	Signal / GND	[pin] / GND

Add your circuit diagram or breadboard photo here:

![Circuit diagram](docs/images/circuit.png)
Alert Logic

The ultrasonic sensor measures the distance to the water surface, so a smaller distance means a higher water level.

State	Condition	OLED	Buzzer
NORMAL	Distance ≥ [__] cm	Level and NORMAL	Silent
WARNING	Distance ≤ [__] cm	Level and WARNING	First alarm stage
CRITICAL	Distance ≤ [__] cm or float switch trips	Level and CRITICAL	Strongest alarm

The float switch forces CRITICAL even if the ultrasonic reading is wrong, so a real overflow is not missed.

Getting Started
Prerequisites
Arduino UNO Q with USB-C cable and a 5V / 2A supply
Arduino App Lab or Arduino IDE set up for the UNO Q
Libraries: [Adafruit SSD1306], [Adafruit GFX] (adjust to what your code uses)
Setup
Clone the repository:
bash
   git clone https://github.com/<your-username>/<your-repo>.git
   cd <your-repo>
Wire the components as shown in Circuit and Wiring.
Open the sketch in src/ and set your pin numbers and thresholds.
Select the Arduino UNO Q board, then upload the sketch.
Place the ultrasonic sensor above the water surface, facing down, and mount the float switch at the critical height.
Results

Add your own photos and readings here.

Output	Preview
Assembled circuit	![circuit](docs/images/circuit.jpg)
OLED showing level and status	![oled](docs/images/oled.jpg)
Buzzer alarm at CRITICAL	![alarm](docs/images/critical.jpg)

Measured readings: [water level in cm → NORMAL / WARNING / CRITICAL]

Comparison with the IEEE Reference Design
Aspect	IEEE reference	This project
Controller	Arduino Nano (16 MHz)	Arduino UNO Q (STM32 MCU + Qualcomm MPU)
Sensing	Ultrasonic + float sensor	HC-SR04 + liquid float switch
Display	0.96" SSD1306 OLED	0.96" SSD1306 OLED
Alerting	GSM900A module sends SMS	5V active buzzer with three alarm stages
Processing	Single microcontroller	Dual-brain edge processing with Linux
Future Scope
Remote alerts by SMS (GSM) or an Android app
Rainfall and weather-forecast data fusion
AI-based predictive insights
Smart-city integration and use in other disaster types
Better sensor reliability, power backup and data security

[1] [Authors], "[Paper title]," in Proc. 2025 7th Int. Conf. Innovative Data Communication Technologies and Application (ICIDCA), Coimbatore, India, Oct. 6–8, 2025. IEEE. doi: 10.1109/ICIDCA66325.2025.11280326
