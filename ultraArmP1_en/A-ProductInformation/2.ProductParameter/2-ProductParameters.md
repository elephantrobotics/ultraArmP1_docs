# Robot Parameters

> In the first chapter, we discussed the product's selling points and design concept, providing you with a panoramic perspective for a high-level understanding of the product. Now, let's move on to the second chapter — Robot Parameters. This chapter will be key to understanding the product's technical details. A thorough understanding of these technical parameters will help you fully appreciate the advancement and practicality of our product, and ensure you can use these technologies more effectively to meet your specific needs.

## 1. Robot Specifications

<img src = ../../resources/A-ProductInformation/2-ProductFeature/P1Production.png
width = "1200" align = "center">

| Index | Parameters |
| :-----------: | :---------: |
| Name | High-Precision 4-Axis Intelligent Palletizing Stepper Motor Robotic Arm |
| Model | ultraArm P1 |
| Degrees of Freedom | 4 |
| Payload | 650 g |
| Working Radius | 360 mm |
| Repeatability | +/- 0.1 mm (ISO 9283) |
| Weight | < 4.5 Kg |
| Power Input | DC 12V, 8A |
| Service Life | > 5000 h |
| Operating Temperature | 0°~50℃ |
| Operating Humidity | 5%~80% |
| Communication | WiFi-2.4G / Bluetooth 2.4G/5G / USB 3.0 / UART / RS485 |
| Communication Protocol | TCP/IP-Socket / MODBUS |

---

## 2. Structural Dimension Parameters
> This chapter uses millimeters as distance units and degrees as angle units.

### 2.1 Product Dimensions

<img src = ../../resources/A-ProductInformation/2-ProductFeature/product_dimensions.png
width = "550" align = "center">

### 2.2 Joint Range of Motion

**Hardware Limits**

| Joint | Range |
| :--------: | :----------:|
| J1 | -168° ~ +168° |
| J2 | -25° ~ +90° |
| J3 | +85° ~ +205° |
| J4 | -180° ~ +180° |

**Software Limits**

| Joint | Range |
| :--------: | :----------:|
| J1 | -158° ~ +158° |
| J2 | -18° ~ +85° |
| J3 | +89° ~ +190° |
| J4 | -179° ~ +179° |

### 2.3 Maximum Joint Speed

| Joint | Maximum Speed |
| :--------: | :----------:|
| J1 | 180°/s |
| J2 | 120°/s |
| J3 | 130°/s |
| J4 | 180°/s |

## 3. Base Interface Description

### Base Front View:

  <img src="../../resources/A-ProductInformation/2-ProductFeature/base_front.png" width="300" height="auto" />

  - ① MiniRobot Control Screen
  - ② MiniRobot Button
  - ③ 12V Power: Connect the power adapter.
  - ④ Motor Interface: For connecting and controlling motors.
  - ⑤ Digital Limit Input: Dedicated input for PNP three-wire photoelectric/proximity sensors.
  - ⑥ 3.3V-I/O: Provides 3.3V power and digital signal interface.
  - ⑦ I²C Grove: For connecting I²C communication sensors or modules.
  - ⑧ PWM Grove: For connecting devices requiring PWM signals, such as servos.
  - ⑨ Power Switch: Press to lock (power on), press again to release (power off).

### Base Side View:

  <img src="../../resources/A-ProductInformation/2-ProductFeature/base_side.png" width="300" height="auto" />

  - ① RS485 Grove: Industrial serial communication, strong anti-interference, long distance.
  - ② Type-C Interface: Program flashing and communication.
  - ③ UART Grove: Serial communication, connect serial devices.

### End-Effector Interface Description

  <img src="../../resources/A-ProductInformation/2-ProductFeature/arm_production.png" width="300" height="auto" />

  - ① Servo Interface: Provides power and control signals for end servo-type actuators (e.g., pneumatic grippers).
  - ② Grove: Supports 5V digital logic level input.
  - ③ Grove: Provides 5V digital logic level output.
  - Tool Button: Used for functional interaction in robotic applications, supports customizable button functions.

## 4. Cartesian Coordinate

<img src = ../../resources/A-ProductInformation/2-ProductFeature/coordinate_system.png
width = "550" align = "center">

* The definitions of each coordinate system are shown in the table below:

| Coordinate System | Definition |
| :--- | :--- |
| **End Coordinate System** | - Origin: Center point of the end quick-connect lock head horizontal plane.<br>- X-axis: When the robotic arm is at joint zero position, facing the base switch, horizontally forward.<br>- Y-axis: When the robotic arm is at joint zero position, facing the base switch, horizontally to the left.<br>- Z-axis: When the robotic arm is at joint zero position, facing the base switch, vertically upward. |
| **Base Coordinate System** | - Origin: Center point of the base bottom surface.<br>- X-axis: Facing the base switch, horizontally forward.<br>- Y-axis: Facing the base switch, horizontally to the left.<br>- Z-axis: Vertically upward. |

## 5. Joint Coordinate System

<img src = ../../resources/A-ProductInformation/2-ProductFeature/关节坐标系.png
width = "550" align = "center">

| Color | Description |
| ----- | ----------- |
| Green | Normal program execution |
| Blue  | Kinematics abnormality |
| Red   | System error |

## 6. Working Range

<img src = ../../resources/A-ProductInformation/2-ProductFeature/工作空间示意图.png
width = "550" align = "center">

<img src = ../../resources/A-ProductInformation/2-ProductFeature/水平平面工作范围.png
width = "550" align = "center">

<img src = ../../resources/A-ProductInformation/2-ProductFeature/垂直平面工作范围.png
width = "550" align = "center">

---

[← Previous Chapter](../1.ProductIntroduction/1-ProductIntroduction.md) | [Next Chapter →](../../B-BasicSettings/3.UserNotice/README.md)
