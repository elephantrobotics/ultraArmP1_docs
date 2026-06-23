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

## Pump Control

This section mainly explains the use of controlling the suction pump.

**Example Usage**

```python
import time
from ultraArm_P1_lib import UltraArmP1

ua = UltraArmP1('COM3', 1000000) # Serial port connection and communication

# Suction pump - suction
def open_pump():
    ua.set_pump_state(0)
    time.sleep(0.05)

# Suction pump - blowing
def pump_blow_air():
    ua.set_pump_state(1)
    time.sleep(0.05)

# Suction pump - closing
def close_pump():
    ua.set_pump_state(2)
    time.sleep(0.05)

# Suction pump repeats work twice
for i in range(2):
    open_pump()
    time.sleep(3)
    pump_blow_air()
    time.sleep(1)
    close_pump()

```

---

[← Previous Chapter](./4_coord.md) | [Next Chapter→](./6_gripper.md)