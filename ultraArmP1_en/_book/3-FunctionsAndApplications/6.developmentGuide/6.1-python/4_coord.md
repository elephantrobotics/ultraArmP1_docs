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

## Coordinate Control

It is mainly used to realize intelligent route planning to let the robot arm move from one position to another specified position. It is divided into `[x, y, z, rx, ry, rz]`, where `[x, y, z]` represents the position of the robot head in space (the coordinate system is the [rectangular coordinate system](https://zhidao.baidu.com/question/2125035227927850747.html)), and `[rx, ry, rz]` represents the posture of the robot head at this point (the coordinate system is the Euler coordinate system). The implementation of the algorithm and the representation of Euler coordinates require certain academic knowledge. We will not explain it too much here. As long as we understand the rectangular coordinate system, we can use this function well.

> **Note:** When setting coordinates, different series of robot arm joint structures are different. For the same set of coordinates, different series of robot arms will show different postures.
>
> <img src="../../../resources\3-FunctionsAndApplications\6.developmentGuide\python\axis/coordinate.jpg" style="zoom: 67%;" />

**Example Use**

```python
import time
from pymycobot import Pro450Client

# The default IP address is "192.168.0.232" and the default port number is 4500
pro450 = Pro450Client('192.168.0.232', 4500) # Client connection communication

if pro450.is_power_on() !=1:
    pro450.power_on()  # Power on

print(pro450.get_coords()) # Read coordinate attitude information

pro450.send_angles([0, -10, -123, 45, 0, 0], 50) # Send angular motion to a certain attitude for coordinate control, speed is 50

time.sleep(3)

pro450.send_coord(1, 200, 50) # Send single coordinate control, speed is 50, so that the X axis moves to the position of 200mm

time.sleep(2)

pro450.send_coords([300, 86.8, 256.9, -178.0, 0.0, -90.0], 50) # Send multi-coordinate control, speed is 50
time.sleep(3)
```

---

[← Previous Chapter](./3_angle.md) | [Next Chapter→](./5_IO.md)