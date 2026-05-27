# MiniRobot User Guide

MiniRobot is the control panel for the myCobot Pro 450 robotic arm, providing a rich set of functional interfaces for easy and intuitive operation and monitoring. This document summarizes the main functional modules of MiniRobot and their usage methods.

## Main Functional Modules

### [1.First Use](./5.4.1-home.md) 
After the robotic arm is powered on, MiniRobot will display its logo and enter the main interface. The main interface displays the robotic arm's joint information and static IP information by default. Different information can be switched using the buttons at the bottom of the screen:

On the main interface:

- A button: Enter the menu interface (automatically returns to the main interface after 30 seconds of inactivity)
- B button: Display angle information and static IP
- C button: Display coordinate information and static IP
- D button: Display the input/output status of the M8 interface at the end

### [2.DragTeach](./5.4.2-dragteach.md) 
Record and play back the robotic arm's motion trajectory using intuitive drag-and-drop operations:

- Record: Record the robotic arm's motion trajectory (maximum 120 seconds)
- Play: Play back a saved trajectory file
- Supports both RAM and Flash storage methods
- Trajectories saved in Flash can be uploaded to the myStudio Pro production folder

### [3.BlocklyRunner](./5.4.3-blocklyrunner.md) 
Manage and execute trajectory files published by myStudio Pro or MiniRobot:

- Automatically checks published trajectories in the myStudio Pro production folder
- Supports play, pause, and stop operations
- Sets single-loop or infinite loop playback mode
- Supports deleting published track files

### [4.QuickMove](./5.4.4-quickmove.md) 
Provides precise movement control for the robotic arm:

- FreeMove: Press and hold the end effector button to drag the robotic arm freely
- JogMove: Controls the movement of a single joint by jogging
- Supports angle and coordinate modes
- 0.1° single step, long press for continuous movement

### [5.Calibration](./5.4.5-calibration.md) 
Used to calibrate the zero position of each joint of the robotic arm:

- Select the joint to be calibrated
- Use the A and B keys to adjust the joint position
- After moving to the calibration position, press the C key to save; the joint angle will be reset to 0°

### [6.Firmware](./5.4.6-firmware.md) 
Displays all version information of the robotic arm:

- RobotID: Unique identifier for the robotic arm
- Screen: MiniRobot version
- System: System version
- Soft: myStudio Pro version + Motion Control version
- Tool: End-effector version

### [7.Connection](./5.4.7-connection.md) 
View the robotic arm's network connection information:

- Select different network interfaces
- Display current network configuration information

## Operation Precautions

- In free movement mode, the motor brake will release; please be careful.
- The menu interface will automatically return to the main interface after 30 seconds of inactivity.
- There is a maximum recording time limit (120 seconds).
- Tracks saved in RAM will be lost after a machine restart.

## File Saving Instructions

- myStudio Pro production folder: Published track files
- myStudio Pro test folder: Unpublished track files
- Saving to Flash in MiniRobot will overwrite previous files, keeping only the latest version
- Only track files saved in Flash in MiniRobot can be uploaded to the myStudio Pro production folder


## Error Codes

Error codes that occur during machine operation are divided into software error codes and motor error codes. The error code table is as follows:

### Software

| Bits | Description |
|---|------|
| 0 | CAN initialization error. Check the main control board, repair the error, and then power off and restart. Symptoms: The robot cannot be enabled or controlled. |
| 1 | Motor initialization error. Check the motor communication lines, repair the error, and then power off and restart. Symptoms: The robot cannot properly provide joint information or control. |
| 2 | Motor malfunction. Check the motor communication lines, etc. Symptoms: Abnormal machine position feedback. Can be cleared using error recovery. |
| 3 | Motor malfunction. Check the motor communication lines, etc. Symptoms: Abnormal machine position feedback. Can be cleared using error recovery. |
| 4 | Position out of tolerance. Check the motor encoder, etc. Symptoms: Abnormal machine connection, unable to move and control. Can be cleared using error recovery. |
| 5 | Emergency stop not triggered. Check the communication lines, etc. Symptoms: Abnormal feedback of not triggering emergency stop. Can be cleared using error recovery. | 
| 6 | Emergency stop not triggered. Check communication lines, etc. Symptom: Abnormal feedback indicating no emergency stop triggered; can be cleared using error recovery. |
| 7 | Motor encoder damaged. Motor cannot move when encoder is damaged; encoder needs replacement. |
| 8 | Emergency stop triggered by button. Enable state must be restored before movement. |


### Motor

| Bit | Description |
|---|------|
| 0 | CAN bus error. Error recovery can be used. If recovery fails, check communication lines, repair, and then power on to enable. |
| 1 | Short circuit. Error recovery can be used. |
| 2 | Invalid setting data. |
| 3 | Control error. Error recovery can be used. |
| 4 | CAN communication error. Error recovery can be used. If recovery fails, check communication lines, repair, and then power on to enable. |
| 5 | Feedback error. Error recovery can be used. |
| 6 | Positive limit switch activated. |
| 7 | Negative limit switch activated. |
| 8 | Overcurrent, recovery available. |
| 9 | I2T protection, recovery available. |
| 10 | Overtemperature, recovery available. |
| 11 | Driver board overtemperature, recovery available. |
| 12 | Overvoltage, recovery available. |
| 13 | Undervoltage, recovery available. |
| 14 | Command error. |
| 15 | Enabled but inactive. |

---

[← Previous Chapter](../5.1-SystemInstructions.md) | [Next Chapter →](./5.2.1-home.md)
