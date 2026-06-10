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

## 坐标控制

主要用于实现智能规划路线让机械臂从一个位置到另一个指定位置。分为`[x,y,z,rx]`，其中`[x,y,z]`表示的是机械臂头部在空间中的位置（该坐标系为[直角坐标系](https://zhidao.baidu.com/question/2125035227927850747.html)），`[rx]`表示的是机械臂头部在该点的姿态（该坐标系为欧拉坐标）。算法的实现以及欧拉坐标的表示需要一定的学术知识，这里不对其过多的讲解，我们只要懂得直角坐标系就可以很好的使用这个函数了。

> **注意：** 在设置坐标时，不同系列的机械臂关节构造有所不同，同一组坐标，不同系列的机械臂会展示不同的姿态。
>
> <img src="../../../resources/C-FunctionsAndApplications/6-SoftwareDevelopment/6.1-python/coord.jpg" style="zoom: 67%;" />

**案例使用**

```python
import time
from pymycobot import UltraArmP1

ua = UltraArmP1('COM3', 1000000)  # 串口连接通信

print(ua.get_coords_info())  # 读取坐标姿态信息

ua.set_angles([-22.98, 38.49, 142.29, 1.23], 50)  # 发送角运动到某一姿态以进行坐标控制，速度为 50
time.sleep(3)

ua.set_coord('X', 325, 50)  # 发送单坐标控制，速度为50，使X轴运动到200mm的位置
time.sleep(2)

ua.set_coords([325.86, 100, -50.16, -21.83], 50) # 发送多坐标控制，速度为50
time.sleep(3)
```

---

[← 上一页](./3_angle.md) | [下一页 →](./5_pump.md)