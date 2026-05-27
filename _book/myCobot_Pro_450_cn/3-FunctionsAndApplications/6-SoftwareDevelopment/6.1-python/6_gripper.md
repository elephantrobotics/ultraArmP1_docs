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

## 夹爪控制

使用Python控制夹爪之前，需要先在机械臂上安装连接好夹爪。不同夹爪适配不同的机械臂，这里使用myGripper F100 Pro力控夹爪。

>>注意：使用之前，确保夹爪小屏幕上的通信方式为modbus模式，否则无法正常控制夹爪。参考[ 夹爪屏幕控制](https://docs.elephantrobotics.com/docs/myGripper-F100-cn/5-BasicApplication/5.1.html)

**案例使用**

```python
import time
from pymycobot import Pro450Client

pro450 = Pro450Client('192.168.0.232', 4500)  # 客户端连接通信

if pro450.is_power_on() !=1:
    pro450.power_on()  # 上电

print(pro450.get_pro_gripper_firmware_version())  # 读取夹爪主次版本号
time.sleep(1)

print(pro450.get_pro_gripper_angle())  # 读取夹爪角度信息
time.sleep(1)

pro450.set_pro_gripper_angle(50)  # 设置夹爪角度为50
time.sleep(2)

pro450.set_pro_gripper_speed(70)  # 设置夹爪的运行速度为70
time.sleep(1)

pro450.set_pro_gripper_open()  # 设置夹爪全打开
time.sleep(2)

pro450.set_pro_gripper_close()  # 设置夹爪全闭合
time.sleep(2)
```

---

[← 上一页](./5_IO.md) | [下一页 →](./7_exception_description.md)