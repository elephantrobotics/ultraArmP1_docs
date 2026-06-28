## 使用前准备

在使用 Python API 之前，请先确认以下硬件和环境准备齐全：

- **硬件设备**  
  - ultraArm P1 机械臂  
  - USB-Type-C 串口线（用于连接机械臂与电脑）  
  - 电源适配器  

- **软件与环境**  
  - 已安装 Python 3.6 及以上版本  
  - 已安装 `pymycobot` 库（通过 `pip install pymycobot` 终端命令安装）  
  - 确保 ultraArm P1 已正确接通电源，并处于待机状态

---

## 关节控制

对于串联式多关节机器人，关节空间的控制是针对机器人各个关节的变量进行的控制，目标是让机器人各个关节按照一定速度达到目标位置。

> **注意：** 在设置角度时，不同系列的机械臂限位有所不同，具体可查看对应型号的参数介绍。


**案例使用**

```python
import time
from pymycobot import UltraArmP1

ua = UltraArmP1('COM3', 1000000)  # 串口连接通信

print(ua.get_angles_info())  # 读取全关节角度信息

ua.set_angles([0, 0, 90, 0], 50)  # 发送全关节角度，速度为50，使机械臂所有关节运动到零位
time.sleep(3)

ua.set_angle(1, 90, 50)  # 发送单关节角度，速度为50，使J1关节运动至90度
time.sleep(2)

ua.set_angles([120, 0, 90, 0, 90], 50)  # 发送全关节角度，速度为50
time.sleep(3)
```

---

[← 上一页](./2_API.md) | [下一页 →](./4_coord.md)
