## Preparation before use

Before using the Python API, please make sure that the following hardware and environment are ready:

- **Hardware Equipment** 
  - ultraArm P1 robotic arm 
  - USB-Type-C serial cable (used to connect the robot arm and computer) 
  - Power adapter

- **Software and Environment** 
  - Python 3.6 and above installed 
  - `pymycobot` library installed (installed via `pip install pymycobot` terminal command) 
  - Make sure ultraArm P1 is properly powered on and in standby mode

---

## Gripper Control

Before using Python to control the gripper, the gripper needs to be installed and connected to the robotic arm. Different grippers are adapted to different robotic arms; here, the mycobot adaptive gripper is used.

**Example Usage**

```python
import time
from pymycobot import UltraArmP1

ua = UltraArmP1('COM3', 1000000) # Serial port connection communication

print(ua.get_gripper_run_status()) # Read the gripper's running status

time.sleep(1)

print(ua.get_gripper_angle()) # Read the gripper's angle information

time.sleep(1)

ua.set_gripper_angle(50, 90) # Set the gripper angle to 50 and the speed to 90

time.sleep(2)

ua.set_gripper_angle(100, 70) # Set the gripper angle to 100 and the speed to 70

time.sleep(2)

ua.set_gripper_angle(0, 70) # Set the gripper angle to 0 and the speed to 70

time.sleep(2)

```

[← Previous Chapter](./5_pump.md) | [Next Chapter→](./7_exception_description.md)