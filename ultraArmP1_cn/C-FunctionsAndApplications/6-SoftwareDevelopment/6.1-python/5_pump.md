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

## 吸泵控制

这里主要说明控制吸泵的使用

**案例使用**

```python
import time
from pymycobot import UltraArmP1

ua = UltraArmP1('COM3', 1000000)  # 串口连接通信

# 吸泵-吸气
def open_pump():
    ua.set_pump_state(0)
    time.sleep(0.05)

# 吸泵-吹气
def pump_blow_air():
    ua.set_pump_state(1)
    time.sleep(0.05)

# 吸泵-关闭
def close_pump():
    ua.set_pump_state(2)
    time.sleep(0.05)

# 吸泵重复工作两次
for i in range(2):
    open_pump()
    time.sleep(3)
    pump_blow_air()
    time.sleep(1)
    close_pump()
```

---

[← 上一页](./4_coord.md) | [下一页 →](./6_gripper.md)