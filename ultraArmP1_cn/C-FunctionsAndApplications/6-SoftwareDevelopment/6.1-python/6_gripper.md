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

## 夹爪控制

使用Python控制夹爪之前，需要先在机械臂上安装连接好夹爪。不同夹爪适配不同的机械臂，这里使用mycobot自适应夹爪。

**案例使用**

```python
import time
from pymycobot import UltraArmP1

ua = UltraArmP1('COM3', 1000000)  # 串口连接通信

print(ua.get_gripper_run_status())  # 读取夹爪运行状态
time.sleep(1)

print(ua.get_gripper_angle())  # 读取夹爪角度信息
time.sleep(1)

ua.set_gripper_angle(50, 90)  # 设置夹爪角度为50，速度90
time.sleep(2)

ua.set_gripper_angle(100, 70)  # 设置夹爪角度为100，速度70
time.sleep(2)

ua.set_gripper_angle(0, 70)  # 设置夹爪角度为0，速度70
time.sleep(2)
```

---

[← 上一页](./5_pump.md) | [下一页 →](./7_exception_description.md)