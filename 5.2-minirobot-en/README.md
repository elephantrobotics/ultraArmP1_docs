# MiniRobot User Guide

MiniRobot is the control panel for the ultraArm P1 robotic arm, providing rich functional interfaces for intuitive operation and monitoring of the robotic arm. This document summarizes the main functional modules of MiniRobot and their usage.

## Main Function Modules

### [1. First Use](./5.2.1-home.md)
After powering on the robotic arm, MiniRobot will display the logo and enter the main interface. The main interface displays the joint information and coordinate information of the robotic arm by default. You can switch to display different information using the buttons at the bottom of the screen.
On the main interface:
- Button A: Enter the menu interface (automatically returns to the main interface after 30 seconds of inactivity)
- Button B: Display angle information and coordinate information
- Button C: Display the input/output status of the four bottom IOs
- Button D: Display the enabled or connection status of WiFi, USB, and BlueTooth

### [2. DragTeach](./5.2.2-dragteach.md)
Record and play robotic arm motion trajectories through intuitive drag operations:
- Record: Record robotic arm motion trajectories (up to 120 seconds)
- Play: Play saved trajectory files
- Supports both RAM and Flash storage methods
- Trajectories saved in Flash can be uploaded to the blocklyrunner folder

### [3. BlocklyRunner](./5.2.3-blocklyrunner.md)
Manage and execute trajectory files published via myStudio Pro or MiniRobot:
- Automatically checks for published trajectories in myStudio Pro's production folder
- Supports play, pause, and stop operations
- Can set single loop or infinite loop playback mode, default is infinite loop
- Supports deleting published trajectory files

### [4. QuickMove](./5.2.4-quickmove.md)
Provides precise movement control of the robotic arm:
- FreeMove: Long press the end button to freely drag the robotic arm
- JogMove: Control single joint movement via jogging
- Supports angle mode and coordinate mode
- Single step 0.1°, long press for continuous movement

### [5. Connection](./5.2.5-connection.md)
View and configure robotic arm connection information:
- USB: Can modify baud rate (both serial port 0 and serial port 1 modified simultaneously)
- WLAN: WiFi connection information, press C key to enter menu to select WiFi and other operations
- BlueTooth: Bluetooth connection information, press C key to enable or disable Bluetooth

### [6. Firmware](./5.2.6-firmware.md)
Displays all version information of the robotic arm:
- RobotID: Unique identifier of the robotic arm
- Display: MiniRobot version
- Version: Main controller version

### [7. Calibration](./5.2.7-calibration.md)
Used to calibrate the zero position of each joint of the robotic arm:
- Select the joint to calibrate
- Use A and B keys to adjust joint position or hold the end button to adjust position (align each joint with physical scale marks)
- After moving to the calibration position, press C key to save, and the corresponding joint will perform calibration action and stop at 0°

### [8. Settings](./5.2.8-settings.md)
Robotic arm settings options:
- Clear Error: Can clear joint limit (homing), clear singularity error
- View Log (python script logs): Can view detailed logs of BlocklyRunner executing python files

### [9. Q&A](./5.2.9-Q&A.md)
Lists common questions and answers when using MiniRobot to control the robotic arm.

## Operation Precautions
- Avoid enabling WiFi, BlueTooth, modbus communication, and BlocklyRunner executing python scripts simultaneously, which may cause insufficient memory
- In FreeMove mode, the motor brakes will release, please be careful with safety
- The menu interface will automatically return to the main interface after 30 seconds of inactivity
- There is a maximum time limit for trajectory recording (120 seconds)
- Trajectories saved in RAM will be lost after machine restart

## File Saving Instructions
- Saving to Flash in MiniRobot will overwrite previous files, only retaining the latest version
- Only trajectory files saved in Flash in MiniRobot can be uploaded to the blocklyrunner folder

## Error Codes

The error code comparison table for robotic arm movement is as follows:

| Error Code | Error Name | Error Description | Solution |
|---------|----------|----------|----------|
| 1-01-1 ~ 1-01-4 | J1-J4 Joint Limit | Robotic arm exceeded software limit | <strong>Python</strong>:<br>Solution A: Use `forced_reset_zero` interface to home<br>Solution B: Use `set_joint_release` interface to switch to FreeMove mode, relax the abnormal joint, manually move within limit then use `set_joint_enable` to lock the joint<br><br><strong>MiniRobot</strong>:<br>Solution A: Enter FreeMove mode page, hold the end button to relax all joints, manually move within limit |
| 1-32-0 | No Coordinate Solution | Cannot reach this coordinate | <strong>Python</strong>:<br>Solution A: Use `clear_error_status` interface to clear error |
| 1-50-0 | J2-J3 Coupling | J2 and J3 joints in coupling position | <strong>Python</strong>:<br><strong>Solution A</strong>: Use `clear_error_status` interface to clear error<br><br><strong>MiniRobot</strong>:<br><strong>Solution A</strong>: Can clear in Settings-Clear Error function page |
| 1-81-1 ~ 1-81-4 | J1-J4 Joint Collision | Collision detection triggered | <strong>Python</strong>:<br><strong>Solution A</strong>: Use `collision_unlock` interface to clear error<br><br><strong>MiniRobot</strong>:<br><strong>Solution A</strong>: Can clear in Settings-Clear Error function page |
| 3-08-1 ~ 3-08-4 | J1-J4 Encoder Error | Motor encoder error | Solution A: Restart<br>Solution B: Disassemble to check if encoder cable is loose or disconnected, re-plug<br>Solution C: After-sales service |
| 4-01-0 | Backend Communication Error | Backend communication error. Symptoms: Robotic arm actual movement does not match operation, or robotic arm remains at previous position | <strong>Solution A</strong>:<br>1. First close the closed loop failure popup<br>2. Re-perform operation to check if closed loop failure still appears or return to main interface to check if data turns gray<br>3. If yes, please contact staff for troubleshooting |

[← Previous Chapter](../5.1-SystemInstructions.md) | [Next Chapter →](./5.2.1-home.md)
