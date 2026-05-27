## Pre-use Preparation

Before using the sample functions, please ensure that the following hardware and environment are complete:

- **Hardware**
  - MyCobot Pro 450 robot arm
  - Network cable (for connecting the robot arm to the computer)
  - Power adapter
  - Emergency stop switch (for safe operation)

- **Software and Environment**
  - Python 3.6 or later installed
  - The `pymycobot` library installed (using the `pip install pymycobot` terminal command)
  - Ensure that the MyCobot Pro 450 is properly powered on and in standby mode.
  - **Note**: The Pro 450 server automatically starts upon powering on; no manual operation is required.

- **Network Configuration**
  - MyCobot Pro 450 default IP address: `192.168.0.232`
  - Default port number: `4500`
  - **Note**: PC The local network card IP address must be set to the same network segment as the robot (e.g., 192.168.0.xxx, where xxx is a number between 2 and 254 and must not conflict with the robot).
  - For detailed configuration instructions, please refer to the section on [Static IP Configuration](../../5-BasicApplication/5.3-myStudioPro/5.3.1-myStudioFirstUse.md).
  - Example:
    - Robot IP: `192.168.0.232`
    - PC IP: `192.168.0.100`
    - Subnet mask: `255.255.255.0`
    - DNS server: `114.114.114.114`

  - **Verification**: After completing the network configuration, execute the following command on the PC terminal. If data packets are successfully returned, the network connection is normal:

    ```bash
    ping 192.168.0.232
    ```

---

## Gripper Control

Before using Python to control the gripper, you must first install and connect the gripper to the robotic arm. Different grippers are compatible with different robotic arms. Here, we use the myGripper F100 Pro force-controlled gripper.

>>Note: Before use, ensure that the communication mode on the gripper's small screen is set to Modbus mode; otherwise, the gripper will not function properly. Refer to [Gripper Screen Control](https://docs.elephantrobotics.com/docs/myGripper-F100-en/5-BasicApplication/5.1.html)

**Example Use**

```python
import time
from pymycobot import Pro450Client

# The default IP address is "192.168.0.232" and the default port number is 4500
pro450 = Pro450Client('192.168.0.232', 4500) # Client connection communication

if pro450.is_power_on() !=1:
    pro450.power_on()  # Power on

print(pro450.get_pro_gripper_firmware_version()) # Read the major and minor version numbers of the gripper

time.sleep(1)

print(pro450.get_pro_gripper_angle()) # Read the gripper angle information

time.sleep(1)

pro450.set_pro_gripper_angle(50) # Set the gripper angle to 50
time.sleep(2)

pro450.set_pro_gripper_speed(70) # Set the gripper speed to 70
time.sleep(1)

pro450.set_pro_gripper_open() # Set the gripper to fully open
time.sleep(2)

pro450.set_pro_gripper_close() # Set the gripper to fully close
time.sleep(2)
```

---

[← Previous Chapter](./5_IO.md) | [Next Chapter→](./7_exception_description.md)