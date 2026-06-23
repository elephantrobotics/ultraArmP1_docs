## Preparation before use

Before using the Python API, please make sure that the following hardware and environment are ready:

- **Hardware Equipment** 
  - ultraArm P1 robotic arm 
  - USB-Type-C serial cable (used to connect the robot arm and computer) 
  - Power adapter

- **Software and Environment** 
  - Python 3.6 and above installed 
  - `ultraArm_P1_lib` library installed (installed via `pip install ultraArm_P1_lib` terminal command) 
  - Make sure ultraArm P1 is properly powered on and in standby mode

---

## Joint control

For a series multi-joint robot, the control of joint space is based on the control of variables of each joint of the robot, with the goal of making each joint of the robot reach the target position at a certain speed.

> **Note:** When setting the angle, different series of robot arm limits have different limits. For details, please check the parameter introduction of the corresponding model.

**Case use**

```python
import time
from ultraArm_P1_lib import UltraArmP1

ua = UltraArmP1('COM3', 1000000) # Serial port connection communication

print(ua.get_angles_info()) # Read all joint angle information

ua.set_angles([0, 0, 90, 0], 50) # Send the full joint angle at a speed of 50 to make all joints of the robot arm move to zero position
time.sleep(3)

ua.set_angle(1, 90, 50) # Send the single joint angle at a speed of 50 to make the J1 joint move to 90 degrees
time.sleep(2)

ua.set_angles([120, 0, 90, 0, 90], 50) # Send the full joint angle with a speed of 50
time.sleep(3)
```

---

[← Previous Chapter](./2_API.md) | [Next Chapter→](./4_coord.md)