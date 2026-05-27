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

## 关节控制

对于串联式多关节机器人，关节空间的控制是针对机器人各个关节的变量进行的控制，目标是让机器人各个关节按照一定速度达到目标位置。

> **注意：** 在设置角度时，不同系列的机械臂限位有所不同，具体可查看对应型号的参数介绍。


**案例使用**

```python
import time
from pymycobot import Pro450Client

pro450 = Pro450Client('192.168.0.232', 4500)  # 客户端连接通信

if pro450.is_power_on() !=1:
    pro450.power_on()  # 上电

print(pro450.get_angles())  # 读取全关节角度信息

pro450.send_angles([0, 0, 0, 0, 0, 0], 50)  # 发送全关节角度，速度为50，使机械臂所有关节运动到零位
time.sleep(3)

pro450.send_angle(1, 90, 50)  # 发送单关节角度，速度为50，使J1关节运动至90度
time.sleep(2)

pro450.send_angles([0, -10, -123, 45, 0, 90], 50)  # 发送全关节角度，速度为50
time.sleep(3)
```

---

[← 上一页](./2_API.md) | [下一页 →](./4_coord.md)
