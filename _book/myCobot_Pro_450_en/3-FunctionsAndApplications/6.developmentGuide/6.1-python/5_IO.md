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

## IO Control

IO stands for data input and output. Our robot arm's Basic and Atom pins have multiple pins. This section mainly explains how to use the end-point IO to control the gripper.

**Example Use**

```python
import time
from pymycobot import Pro450Client

# The default IP address is "192.168.0.232" and the default port number is 4500
pro450 = Pro450Client('192.168.0.232', 4500) # Client connection and communication

if pro450.is_power_on() !=1:
    pro450.power_on()  # Power on

# Open the gripper
def open_gripper():
    pro450.set_digital_output(1, 0) # Set pin 1 to a low level
    pro450.set_digital_output(2, 1) # Set pin 2 to a high level
    time.sleep(0.05)

# Close the gripper
def close_gripper():
    pro450.set_digital_output(1, 1) # Set pin 1 to a high level
    pro450.set_digital_output(2, 0) # Set pin 2 to a low level
    time.sleep(0.05)

# Repeat the gripper opening and closing twice
for i in range(2):
    open_gripper()
    time.sleep(3)
    close_gripper()
```
---

[← Previous Chapter](./4_coord.md) | [Next Chapter→](./6_gripper.md)