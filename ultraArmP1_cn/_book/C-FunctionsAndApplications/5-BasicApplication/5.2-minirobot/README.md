# MiniRobot使用指南

MiniRobot是ultraArm P1机械臂的控制面板，提供了丰富的功能接口，方便用户直观地操作和监控机械臂。本文档汇总了MiniRobot的主要功能模块及其使用方法。

## 主要功能模块

### [1.首次使用](./5.2.1-home.md)
机械臂上电后，MiniRobot会显示Logo并进入主界面。主界面默认显示机械臂的关节信息和坐标信息。通过屏幕底部按键可切换显示不同信息。
在主界面下:
- A键：进入菜单界面（30秒无操作自动返回主界面）
- B键：显示角度信息和坐标信息
- C键：显示底部四个IO的输入输出状态
- D键：显示WiFi、USB和BlueTooth的开启或连接状态

### [2.DragTeach](./5.2.2-dragteach.md)
通过直观的拖动操作来录制和播放机械臂运动轨迹：
- Record：录制机械臂运动轨迹（最长120秒）
- Play：播放已保存的轨迹文件
- 支持RAM和Flash两种存储方式
- Flash中保存的轨迹可上传至blocklyrunner的文件夹

### [3.BlocklyRunner](./5.2.3-blocklyrunner.md)
管理和执行通过myStudio Pro或MiniRobot发布的轨迹文件：
- 自动检查myStudio Pro的生产文件夹中的已发布轨迹
- 支持播放、暂停、停止操作
- 可设置单次循环或无限循环播放模式，默认无限循环播放
- 支持删除已发布的轨迹文件

### [4.QuickMove](./5.2.4-quickmove.md)
提供机械臂的精确移动控制：
- FreeMove（自由移动）：长按末端按钮可自由拖动机械臂
- JogMove（关节点动）：通过点动控制单个关节运动
- 支持角度模式和坐标模式
- 单步0.1°，长按连续运动

### [5.Connection](./5.2.5-connection.md)
查看和配置机械臂的连接信息：
- USB：可修改波特率（串口0和串口1同步修改）
- WLAN：WiFi连接信息，可点击C键进入菜单选择连接对应wifi等等操作
- BlueTooth：蓝牙连接信息，可点击C键进入开启或关闭蓝牙

### [6.Firmware](./5.2.6-firmware.md)
显示机械臂的所有版本信息：
- RobotID：机械臂唯一标识符
- Display：MiniRobot版本
- Version：主控版本

### [7.Calibration](./5.2.7-calibration.md)
用于校准机械臂各关节的零位位置：
- 选择需要校准的关节
- 使用A、B键调整关节位置或者按住末端按钮调整位置（各关节对准物理刻度线）
- 运动至校准位置后按下C键保存，对应关节将执行校准动作，完成之后停止到0°

### [8.Settings](./5.2.8-settings.md)
机械臂的设置选项：
- Clear Error（清除错误）：可清除关节限位（回零）、清除奇异点报错
- View Log（python脚本日志）：可查看BlocklyRunner执行python文件的详细日志

### [9.Q&A](./5.2.9-Q&A.md)
列出了使用 MiniRobot 控制机械臂时的常见问题及解答。

## 操作注意事项
- 避免同时开启WiFi、BlueTooth、modbus通信和BlocklyRunner执行python脚本，导致运行内存不足
- 在自由移动模式下，电机制动器会松开，请务必注意安全
- 菜单界面30秒无操作会自动返回主界面
- 录制轨迹有最长时间限制（120秒）
- RAM中保存的轨迹在机器重启后会丢失

## 文件保存说明
- MiniRobot中进行Flash保存会覆盖之前的文件，仅保留最新版本
- MiniRobot中只有保存在Flash中的轨迹文件才可以被上传至blocklyrunner的文件夹

## 错误码

机器运动中出现的错误码对照表如下：

| 错误编码 | 报错名称 | 错误描述 | 解决方案 |
|---------|----------|----------|----------|
| 1-01-1 ~ 1-01-4 | J1-J4关节超限 | 机械臂超出软件限位 | <strong>Python</strong>:<br>方案A：使用`forced_reset_zero`接口回零<br>方案B：使用`set_joint_release`接口切换至自由移动模式，放松异常关节，手动移动到限位内之后再使用`set_joint_enable`接口抱紧关节<br><br><strong>MiniRobot</strong>:<br>方案A：进入FreeMove模式页面，按住末端按钮即可放松所有关节，手动移动到限位内即可 |
| 1-32-0 | 坐标无解 | 该坐标无法抵达 | <strong>Python</strong>:<br>方案A：使用`clear_error_status`接口清除报错 |
| 1-50-0 | J2、J3耦合 | J2和J3关节处于耦合位置 | <strong>Python</strong>:<br><strong>方案A</strong>：手动移动到脱离耦合点位<br><br><strong>MiniRobot</strong>:<br><strong>方案A</strong>：手动移动到脱离耦合点位
| 1-81-1 ~ 1-81-4 | J1-J4关节碰撞 | 触发碰撞检测 | <strong>Python</strong>:<br><strong>方案A</strong>：使用`collision_unlock`接口清除报错<br><br><strong>MiniRobot</strong>:<br><strong>方案A</strong>：在Settings-Clear Error功能页面下可清除 |
| 3-08-1 ~ 3-08-4 | J1-J4编码器报错 | 电机编码器报错 | 方案A：重启<br>方案B：拆机检查编码器线是否松动或脱落，进行重新插拔<br>方案C：售后维修 |
| 4-01-0 | 后端通信异常 | 后端通信异常。表现：机械臂实际运动与操作不符，或者出现机械臂停留在上一步等。 | <strong>方案A</strong>：<br>1. 先关掉闭环失败弹窗<br>2. 重新进行一次操作检查是否还提示闭环失败或者退回主页面检查数据是否变灰色<br>3. 如是请联系工作人员排查

[← 上一章](../5.1-SystemInstructions.md) | [下一章 →](./5.2.1-home.md)