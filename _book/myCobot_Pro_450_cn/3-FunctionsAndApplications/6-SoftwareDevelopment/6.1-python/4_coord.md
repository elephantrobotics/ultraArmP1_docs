## 使用前准备

在使用 Python API 之前，请先确认以下硬件和环境准备齐全：

- **硬件设备**  
  - MyCobot Pro 450 机械臂  
  - 网线（用于连接机械臂与电脑）  
  - 电源适配器  
  - 急停开关（确保安全操作）

- **软件与环境**  
  - 已安装 Python 3.6 及以上版本  
  - 已安装 `pymycobot` 库（通过 `pip install pymycobot` 终端命令安装）  
  - 确保 MyCobot Pro 450 已正确接通电源，并处于待机状态  
  - **注意**：Pro 450 服务端会在设备上电后自动启动，无需手动操作  

- **网络配置**  
  - MyCobot Pro 450 默认 IP 地址：`192.168.0.232`  
  - 默认端口号：`4500`  
  - **注意**：PC 端需要将本机网卡 IP 设置为 **同一网段**（例如 `192.168.0.xxx`，`xxx` 为 2~254 之间的任意数，且不能与机械臂冲突）。 
  - 具体配置方式请查看 [静态IP配置](https://docs.elephantrobotics.com/docs/mycobot-pro450-cn/3-FunctionsAndApplications/5-BasicApplication/5.3-myStudioPro/5.3.1-myStudioFirstUse.html#%E9%9D%99%E6%80%81ip%E9%85%8D%E7%BD%AE) 章节内容。
  - 示例：  
    - 机械臂 IP：`192.168.0.232`  
    - PC IP：`192.168.0.100`  
    - 子网掩码：`255.255.255.0`
    - DNS服务器：`114.114.114.114`
  
  - **验证**：完成网络配置后，可在 PC 终端执行以下命令，若能成功返回数据包，则说明网络连接正常：  
  
    ```bash
    ping 192.168.0.232
    ```

---

## 坐标控制

主要用于实现智能规划路线让机械臂从一个位置到另一个指定位置。分为`[x,y,z,rx,ry,rz]`，其中`[x,y,z]`表示的是机械臂头部在空间中的位置（该坐标系为[直角坐标系](https://zhidao.baidu.com/question/2125035227927850747.html)），`[rx,ry,rz]`表示的是机械臂头部在该点的姿态（该坐标系为欧拉坐标）。算法的实现以及欧拉坐标的表示需要一定的学术知识，这里不对其过多的讲解，我们只要懂得直角坐标系就可以很好的使用这个函数了。

> **注意：** 在设置坐标时，不同系列的机械臂关节构造有所不同，同一组坐标，不同系列的机械臂会展示不同的姿态。
>
> <img src="../../../resources\3-FunctionsAndApplications\6.developmentGuide\python\axis/坐标.jpg" style="zoom: 67%;" />

**案例使用**

```python
import time
from pymycobot import Pro450Client

pro450 = Pro450Client('192.168.0.232', 4500)  # 客户端连接通信

if pro450.is_power_on() !=1:
    pro450.power_on()  # 上电

print(pro450.get_coords())  # 读取坐标姿态信息

pro450.send_angles([0, -10, -123, 45, 0, 0], 50)  # 发送角运动到某一姿态以进行坐标控制，速度为 50
time.sleep(3)

pro450.send_coord(1, 200, 50)  # 发送单坐标控制，速度为50，使X轴运动到200mm的位置
time.sleep(2)

pro450.send_coords([300, 86.8, 256.9, -178.0, 0.0, -90.0], 50) # 发送多坐标控制，速度为50
time.sleep(3)
```

---

[← 上一页](./3_angle.md) | [下一页 →](./5_IO.md)