# Obstacle_Detection_Robot
An obstacle detection robot is an autonomous system designed to navigate its environment while avoiding collisions with objects or walls. It typically uses sensors—such as ultrasonic, infrared, or LiDAR—to scan the path ahead, processing the incoming data via a microcontroller (like Arduino or ESP32) to steer around hazards in real time.

Conversation with Gemini
obstacle detection robot. give a small description for github.

An obstacle detection robot is an autonomous system designed to navigate its environment while avoiding collisions with objects or walls. It typically uses sensors—such as ultrasonic, infrared, or LiDAR—to scan the path ahead, processing the incoming data via a microcontroller (like Arduino or ESP32) to steer around hazards in real time.

GitHub Repository Description Options
Choose one of these depending on the style of your repository:

Short & Punchy (Bio):

An Arduino-based autonomous obstacle-avoidance robot utilizing ultrasonic sensors to navigate environments safely and efficiently.

Feature-Focused:

Arduino/C++ powered obstacle detection robot featuring real-time distance sensing, automated path correction, and modular motor control for seamless navigation.

Bullet-Point README Intro:

An autonomous robotics project built for accurate obstacle detection and collision avoidance using ultrasonic sensors, an L298N motor driver, and a microcontroller.

ok send code for this also. i used ultrasonic sensor, and other necessary thngs with moving automatically by external battery.

Here is a complete, well-commented Arduino sketch designed for an obstacle-avoiding robot. It uses an Ultrasonic Sensor (HC-SR04) to measure distances and an L298N Motor Driver to control two DC motors powered by your external battery pack.

Arduino Code
C++
// --- Pin Configurations ---

// HC-SR04 Ultrasonic Sensor Pins
const int TRIG_PIN = 9;
const int ECHO_PIN = 10;

// L298N Motor Driver Pins
// Left Motor
const int IN1 = 2;
const int IN2 = 3;
// Right Motor
const int IN3 = 4;
const int IN4 = 5;

// Enable pins (Connect to PWM pins to control speed, or leave HIGH)
const int ENA = 6; 
const int ENB = 11;

// --- Thresholds ---
const int OBSTACLE_DISTANCE_CM = 25; // Distance to trigger turn (in cm)
const int MOTOR_SPEED = 180;          // Motor speed range (0 - 255)

void setup() {
  // Motor pins as outputs
  pinMode(IN1, OUTPUT);
  pinMode(IN2, OUTPUT);
  pinMode(IN3, OUTPUT);
  pinMode(IN4, OUTPUT);
  pinMode(ENA, OUTPUT);
  pinMode(ENB, OUTPUT);

  // Ultrasonic pins
  pinMode(TRIG_PIN, OUTPUT);
  pinMode(ECHO_PIN, INPUT);

  // Start serial monitor for debugging
  Serial.begin(9600);
}

void loop() {
  int distance = getDistance();
  Serial.print("Distance: ");
  Serial.print(distance);
  Serial.println(" cm");

  if (distance > 0 && distance < OBSTACLE_DISTANCE_CM) {
    // Obstacle detected: Stop, back up briefly, and turn
    stopMotors();
    delay(200);
    
    moveBackward();
    delay(400);
    
    stopMotors();
    delay(200);
    
    turnRight();
    delay(500); // Adjust duration to control turn angle
  } else {
    // Path is clear: Move forward
    moveForward();
  }

  delay(50); // Short delay for measurement stability
}

// --- Helper Functions ---

// Function to measure distance using HC-SR04
int getDistance() {
  digitalWrite(TRIG_PIN, LOW);
  delayMicroseconds(2);
  digitalWrite(TRIG_PIN, HIGH);
  delayMicroseconds(10);
  digitalWrite(TRIG_PIN, LOW);

  long duration = pulseIn(ECHO_PIN, HIGH, 30000); // 30ms timeout
  if (duration == 0) return 999; // Return large distance if timeout occurs

  int distanceCm = duration * 0.034 / 2;
  return distanceCm;
}

// Motor Movement Commands
void moveForward() {
  analogWrite(ENA, MOTOR_SPEED);
  analogWrite(ENB, MOTOR_SPEED);
  digitalWrite(IN1, HIGH);
  digitalWrite(IN2, LOW);
  digitalWrite(IN3, HIGH);
  digitalWrite(IN4, LOW);
}

void moveBackward() {
  analogWrite(ENA, MOTOR_SPEED);
  analogWrite(ENB, MOTOR_SPEED);
  digitalWrite(IN1, LOW);
  digitalWrite(IN2, HIGH);
  digitalWrite(IN3, LOW);
  digitalWrite(IN4, HIGH);
}

void turnRight() {
  analogWrite(ENA, MOTOR_SPEED);
  analogWrite(ENB, MOTOR_SPEED);
  digitalWrite(IN1, HIGH);
  digitalWrite(IN2, LOW);
  digitalWrite(IN3, LOW);
  digitalWrite(IN4, HIGH); // Reverse right motor to spin in place
}

void stopMotors() {
  digitalWrite(IN1, LOW);
  digitalWrite(IN2, LOW);
  digitalWrite(IN3, LOW);
  digitalWrite(IN4, LOW);
}
Wiring Summary
Component	Component Pin	Arduino Pin	Notes
Ultrasonic	VCC	5V	Sensor power
GND	GND	Common ground
Trig	Pin 9	Output trigger
Echo	Pin 10	Input echo
L298N Driver	IN1, IN2	Pin 2, Pin 3	Left motor direction
IN3, IN4	Pin 4, Pin 5	Right motor direction
ENA, ENB	Pin 6, Pin 11	Speed control (remove jumpers)
Power	External Battery (+)	L298N 12V Pin	Powers motor driver
External Battery (-)	L298N GND & Arduino GND	Must share a common ground
Important Power Note: Ensure the ground (GND) of your external battery pack is connected to both the motor driver GND and the Arduino GND, or signal processing will be erratic.

Where should we go from here?

Add servo motor sweeping to look left/right before turning

Format this for a clean GitHub README file
readme file

⚙️ Logic Flow
Triggering Sensor: The HC-SR04 emits a 10µs ultrasonic pulse.

Echo Calculation: Measures return duration to compute distance in centimeters (Distance= 
2
Time×0.034
​
 ).

Threshold Checking:

If Distance > 25 cm: Drive forward.

If Distance ≤ 25 cm: Halt → Reverse 400ms → Stop → Pivot right 500ms → Resume scanning.

📂 Project Structure
Plaintext
.
├── README.md
└── src/
    └── obstacle_detection_robot.ino
🚀 Future Enhancements
Integrate an SG90 Servo Motor to sweep the sensor left and right to pick the clearer path before turning.

Add an IR Line Tracking Module to create a hybrid line-following and obstacle-avoiding vehicle.

Implement ESP32 Wi-Fi / Bluetooth control for switching between autonomous mode and manual teleoperation.

📄 License
This project is open-source and available under the MIT License.
"""

with open("README.md", "w") as f:
f.write(readme_content)

print("README.md file generated successfully.")


```text?code_stdout&code_event_index=1
README.md file generated successfully.

Your Markdown file is ready

MD icon
README
 MD 
Open
What's included in README.md:
Project Title & Overview: Clear summary formatted for GitHub.

Features: Highlighted key capabilities (Ultrasonic sensing, automated turning, PWM speed control).

Hardware & Components Table: Detailed bill of materials (Arduino, L298N, HC-SR04, motors, chassis, battery).

Wiring & Circuit Table: Complete pinout diagram and power layout (with explicit shared ground instructions).

Embedded Arduino Code: Embedded C++ code snippet formatted with proper syntax highlighting.

Logic Flow & Calculations: Formula explanation for distance calculation.

Future Upgrades: Bulleted list of future roadmap features (servo scanning, ESP32 Bluetooth control, line follower hybrid).

create-readme
Generates a professional GitHub README file as a downloadable Markdown file.

Instructions
Instructions
Create a detailed, beautifully structured README.md file for the project.




Gemini is AI and can make mistakes.

Analyzing
# Autonomous Obstacle Detection & Avoidance Robot

An autonomous mobile robot designed to navigate environments independently while detecting and avoiding obstacles in real time. Built using an Arduino microcontroller, HC-SR04 ultrasonic sensor, L298N motor driver module, and dual DC motors powered by an external battery supply.

---

## 📌 Features

* **Real-Time Distance Measurement:** Uses high-frequency sound waves via the HC-SR04 ultrasonic sensor to calculate clearance in front of the vehicle.
* **Automated Collision Avoidance:** Automatically halts, reverses, and alters path direction upon detecting an obstacle within 25 cm.
* **Pulse Width Modulation (PWM) Speed Control:** Dynamic motor speed tuning via L298N enable pins for smooth navigation and turns.
* **Isolated High-Current Powering:** Separates microcontroller logic power from high-drain motor power for electrical stability and safety.

---

## 🛠️ Components & Hardware Requirements

| Component | Quantity | Details |
| :--- | :---: | :--- |
| **Arduino Uno / Nano** | 1 | Microcontroller Board |
| **HC-SR04 Ultrasonic Sensor** | 1 | Distance measuring module |
| **L298N Dual H-Bridge Motor Driver** | 1 | Motor controller board |
| **Geared DC Motors + Wheels** | 2 | 3V–12V Robot Chassis Motors |
| **External Battery Pack** | 1 | 7.4V Li-ion (2x 18650) or 9V/12V Battery Holder |
| **Robot Chassis** | 1 | 2WD Acrylic Base Frame |
| **Jumper Wires & Breadboard** | — | Male-to-Male and Male-to-Female leads |

---

## 🔌 Wiring & Circuit Diagram

### Pin Mapping Table

| Component | Sensor / Driver Pin | Arduino Pin | Power Supply Connections |
| :--- | :--- | :--- | :--- |
| **HC-SR04** | `VCC` | `5V` | — |
| | `GND` | `GND` | — |
| | `Trig` | Pin `9` | — |
| | `Echo` | Pin `10` | — |
| **L298N Driver** | `IN1` | Pin `2` | — |
| | `IN2` | Pin `3` | — |
| | `IN3` | Pin `4` | — |
| | `IN4` | Pin `5` | — |
| | `ENA` | Pin `6` (PWM) | — |
| | `ENB` | Pin `11` (PWM) | — |
| | `12V` Terminal | — | Battery Positive (`+`) |
| | `GND` Terminal | `GND` (Shared) | Battery Negative (`-`) |
| | `5V` Terminal | `VIN` or `5V` | (Optional logic power supply) |

> ⚠️ **CRITICAL NOTE:** Always connect the **GND** terminal of the L298N motor driver to both the battery negative (`-`) and an Arduino `GND` pin. A shared ground reference is required for signal transmission.

---

## 💻 Arduino Code Setup

1. Install the [Arduino IDE](https://www.arduino.cc/en/software).
2. Connect your Arduino board via USB.
3. Open the repository code `src/obstacle_detection_robot.ino`.
4. Select the appropriate Board and Port under the **Tools** menu.
5. Click **Upload** (`Ctrl + U` or `Cmd + U`).

```cpp
// --- Pin Configurations ---
const int TRIG_PIN = 9;
const int ECHO_PIN = 10;

const int IN1 = 2;
const int IN2 = 3;
const int IN3 = 4;
const int IN4 = 5;
const int ENA = 6; 
const int ENB = 11;

// --- Configurable Parameters ---
const int OBSTACLE_DISTANCE_CM = 25; // Distance threshold in cm
const int MOTOR_SPEED = 180;          // Speed (0 to 255)

void setup() {
  pinMode(IN1, OUTPUT);
  pinMode(IN2, OUTPUT);
  pinMode(IN3, OUTPUT);
  pinMode(IN4, OUTPUT);
  pinMode(ENA, OUTPUT);
  pinMode(ENB, OUTPUT);

  pinMode(TRIG_PIN, OUTPUT);
  pinMode(ECHO_PIN, INPUT);

  Serial.begin(9600);
}

void loop() {
  int distance = getDistance();
  Serial.print("Distance: ");
  Serial.print(distance);
  Serial.println(" cm");

  if (distance > 0 && distance < OBSTACLE_DISTANCE_CM) {
    stopMotors();
    delay(200);
    moveBackward();
    delay(400);
    stopMotors();
    delay(200);
    turnRight();
    delay(500);
  } else {
    moveForward();
  }

  delay(50);
}

int getDistance() {
  digitalWrite(TRIG_PIN, LOW);
  delayMicroseconds(2);
  digitalWrite(TRIG_PIN, HIGH);
  delayMicroseconds(10);
  digitalWrite(TRIG_PIN, LOW);

  long duration = pulseIn(ECHO_PIN, HIGH, 30000);
  if (duration == 0) return 999;

  return duration * 0.034 / 2;
}

void moveForward() {
  analogWrite(ENA, MOTOR_SPEED);
  analogWrite(ENB, MOTOR_SPEED);
  digitalWrite(IN1, HIGH);
  digitalWrite(IN2, LOW);
  digitalWrite(IN3, HIGH);
  digitalWrite(IN4, LOW);
}

void moveBackward() {
  analogWrite(ENA, MOTOR_SPEED);
  analogWrite(ENB, MOTOR_SPEED);
  digitalWrite(IN1, LOW);
  digitalWrite(IN2, HIGH);
  digitalWrite(IN3, LOW);
  digitalWrite(IN4, HIGH);
}

void turnRight() {
  analogWrite(ENA, MOTOR_SPEED);
  analogWrite(ENB, MOTOR_SPEED);
  digitalWrite(IN1, HIGH);
  digitalWrite(IN2, LOW);
  digitalWrite(IN3, LOW);
  digitalWrite(IN4, HIGH);
}

void stopMotors() {
  digitalWrite(IN1, LOW);
  digitalWrite(IN2, LOW);
  digitalWrite(IN3, LOW);
  digitalWrite(IN4, LOW);
}
```

---

## ⚙️ Logic Flow

1. **Triggering Sensor:** The HC-SR04 emits a 10µs ultrasonic pulse.
2. **Echo Calculation:** Measures return duration to compute distance in centimeters ($Distance = rac{Time 	imes 0.034}{2}$).
3. **Threshold Checking:**
   * If `Distance > 25 cm`: Drive forward.
   * If `Distance ≤ 25 cm`: Halt → Reverse 400ms → Stop → Pivot right 500ms → Resume scanning.

---

## 📂 Project Structure

```text
.
├── README.md
└── src/
    └── obstacle_detection_robot.ino
```

---

## 🚀 Future Enhancements

* Integrate an **SG90 Servo Motor** to sweep the sensor left and right to pick the clearer path before turning.
* Add an **IR Line Tracking Module** to create a hybrid line-following and obstacle-avoiding vehicle.
* Implement **ESP32 Wi-Fi / Bluetooth control** for switching between autonomous mode and manual teleoperation.

---

## 📄 License

This project is open-source and available under the [MIT License](LICENSE).
README.md
Displaying README.md.
