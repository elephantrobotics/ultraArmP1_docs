# 5.3 myStudio Pro Introduction

**myStudio Pro** is a robot programming and control software integrating multiple functions, providing users with one-stop solutions such as visual programming interaction, quick movement control, drag teaching, robot status query and configuration. The software mainly integrates five functional modules: `Block Programming`, `Debug Panel`, `Resource Center`, `Scene apps`, and `Configuration`, covering the entire process requirements from programming to debugging, from learning to deployment.

**Block Programming** The module draws inspiration from the Scratch programming language developed by the Massachusetts Institute of Technology. It uses a graphical method of assembling building blocks to facilitate programming. Users can gradually construct complete code logic by intuitively dragging and combining the modules. The entire process is simple to operate and easy to understand, making it particularly suitable for beginners in programming and educational settings.

From the perspective of user experience, **Block Programming** is a low-barrier, visual code generation tool that makes programming as easy and intuitive as building with blocks. From the developer's viewpoint, this module is essentially a text editor that can dynamically generate structured code. The code generated through interactive dragging by users will eventually be transformed into an instruction sequence that can be executed on the robot. This design and interaction method not only reduces the difficulty of use but also ensures the professionalism and executability of the program.

**Debug Panel** The module can control the angles and coordinates of each joint through point motion. It allows setting the change in joint angles and coordinate movement distance for each point motion, and can display the posture of the robotic arm in real time. It also enables manual control of the signal switch status of the corresponding IO ports.

**Resource Center** The module provides users with a convenient resource navigation function, presenting centralized access points to commonly used external links, such as technical documents and official contact information. Users do not need to search manually and can quickly access relevant support materials, thereby improving the efficiency of use and maintenance.

**Scene apps** The module provides users with the two core functions of the robot, namely writing and drawing, as well as laser engraving. Users can perform graphic editing and preview operations, and convert them into actual control instructions to realize the writing, drawing and laser engraving functions of the robot.

**Configuration** The module covers the basic configuration options for software and robot systems. Users can perform operations such as language switching, setting joint motion limits, system update detection, and software driver updates here.

## Interface Introduction

<img width = "1200" align = "center" src="../../../resources/C-FunctionsAndApplications/5-BasicApplication/5.3-myStudioPro/home-1.png" />

| Serial Number | Function Description |
| ------ | ------------------------------------------------------------ |
| 1 | Name and logo：Displays the software name and logo only |
| 2 | Robot Status：Real-time detection of the robotic arm status |
| 3 | Log：Records robotic arm logs; you can view the current log or download historical logs |
| 4 | Return to zero：Returns the robotic arm to the initial zero position |
| 5 | Soft emergency stop：Emergency interruption of all motion commands |
| 6 | Block programming：Enter the block programming workspace and create a new workspace |
| 7 | Create file：Enter the block programming workspace and create a new workspace |
| 8 | Open file：Enter the block programming workspace and open the file management panel |
| 9 | Debug Panel：Enter the debug panel tool page |
| 10 | Sample files：Block programming sample files; click to go to the block programming workspace |
| 11 | Resource Center：Enter the Resource Center page |
| 12 | Scene apps：Enter the Scene apps main page |
| 13 | Configuration：Enter the Configuration page |
| 14 | Recent files：Displays the 20 most recent files from Scene apps and Block programming |
| 15 | Open file management：Enter the block programming workspace and open the file management panel |

---

## Return to Zero

> Used to automatically return the robotic arm from any posture to the initial zero position

This button's function is: to control all the joints of the robot to return to the zero position.

<img width = "1200" align = "center" src="../../../resources/C-FunctionsAndApplications/5-BasicApplication/5.3-myStudioPro/home-2.png" />

**Note:** The function of this button is activated only when the communication with the robot has been successfully established. After long-pressing and clicking this button with the left mouse button, the robot will start to execute the zero return command. The robotic arm will slowly move to the zero position. Once the long press is released, the zero return command will stop being executed.

After the zero return is completed, a pop-up window will appear to indicate that the zero return is completed.

<img width = "1200" align = "center" src="../../../resources/C-FunctionsAndApplications/5-BasicApplication/5.3-myStudioPro/home-3.png" />

---

## Emergency Stop

> Used to urgently cut off all motion commands while the robotic arm is moving

This button's function is: to stop the current movement of the robot and abort all running programs.

<img width = "1200" align = "center" src="../../../resources/C-FunctionsAndApplications/5-BasicApplication/5.3-myStudioPro/home-4.png" />

After clicking, a motion stop command will be issued. Once the command is issued successfully, the robotic arm will stop moving and a success message will be displayed.

---

## Function Implementation

> Used to quickly navigate to five core functional modules: Block programming, Debug panel, Resource Center, Scene apps, and Configuration

Here you can select the functions you wish to use. The functions include the following:

> 1. [Block Programming](./5.3.3-blockly.md)
> 2. [Debug panel](./5.3.4-debugPlane.md)
> 3. [Resource Center](./5.3.5-resourceCenter.md)
> 4. [Scene apps](./5.3.6-scene.md)
> 5. [Configuration](./5.3.7-setting.md)

---

## Block Programming

> Used to create or open a block programming file and enter the block programming workspace

`Block Programming` is a fully visual, modular programming interface that belongs to a graphical programming language. It is suitable for beginners to familiarize themselves with programming. Users develop applications by dragging and dropping puzzle pieces, which enables them to create simple and complex functions. It supports functions such as saving, loading, single-step debugging execution, and executing a specified single block.

#### Create File

This is a clickable button. When you click it with the left mouse button, it will lead you to the building [block programming](./5.3.3-blockly.md) interface.

<img width = "1200" align = "center" src="../../../resources/C-FunctionsAndApplications/5-BasicApplication/5.3-myStudioPro/home-5.png" />

#### Open File

This is the same function as the clickable button at the "Recent Files" section, which leads to `the file management` button.

<img width = "1200" align = "center" src="../../../resources/C-FunctionsAndApplications/5-BasicApplication/5.3-myStudioPro/home-6.png" />

After clicking, it will automatically redirect to the block programming and open the file management list. You can perform related operations on JSON files based on the file list.

**Quickly load the previously saved blockly/gcode files**

When you have used the block programming and have saved a blockly file, as shown in the figure below, the name of the saved file and the save time will be displayed. The maximum number of displayed files is 20. If there are more than 20 files, only the 20 most recently saved files will be shown. Clicking the left mouse button will open the block programming and automatically load the selected blockly file.

<img width = "1200" align = "center" src="../../../resources/C-FunctionsAndApplications/5-BasicApplication/5.3-myStudioPro/home-7.png" />

---

## Common Tools

> Used to quickly jump to four frequently used tools: Debug panel, Resource Center, Scene apps, and Configuration

#### [Debug Panel](./5.3.4-debugPlane.md)

Function: Provides quick control of robot I/O as well as quick control of joint angles and coordinates.

<img width = "1200" align = "center" src="../../../resources/C-FunctionsAndApplications/5-BasicApplication/5.3-myStudioPro/home-8.png" />

#### [Resource Center](./5.3.5-resourceCenter.md)

Function: Provide robot product user manual, official videos, official GitHub, official online store, and feedback function.

<img width = "1200" align = "center" src="../../../resources/C-FunctionsAndApplications/5-BasicApplication/5.3-myStudioPro/home-9.png" />

#### [Scene Apps](./5.3.6-scene.md)

Function: Integrates the following core functions: writing, drawing, and laser engraving.

<img width = "1200" align = "center" src="../../../resources/C-FunctionsAndApplications/5-BasicApplication/5.3-myStudioPro/home-10.png" />

#### [Configuration Center](./5.3.7-setting.md)

Function: Integrates the following core functions: real-time monitoring of robot status and information, one-click check for updated application versions, personalized settings (language/motion parameters), pin configuration, etc., helping you efficiently manage the robot system.

<img width = "1200" align = "center" src="../../../resources/C-FunctionsAndApplications/5-BasicApplication/5.3-myStudioPro/home-11.png" />

---

## Information Display

> Used to display the robotic arm running status, current coordinates, and end-effector motion data in real time on the bottom status bar

The underlying part of the application, including the alert notifications and the current operating status of the robot.

<img width = "1200" align = "center" src="../../../resources/C-FunctionsAndApplications/5-BasicApplication/5.3-myStudioPro/home-12.png" />

---

## Alarm Notification

> Used to provide alarm prompts via pop-ups and the status bar when the robotic arm encounters an error

Function: Displays robot error messages, and left-clicking opens the error log window.

Left-click to open the error log window.

<img width = "1200" align = "center" src="../../../resources/C-FunctionsAndApplications/5-BasicApplication/5.3-myStudioPro/home-13.png" />

If the robot encounters an error during operation, the application will capture the exception and display it in the error log interface. The meanings in the error log table are as follows:

- number: Error log number
- time: Time when the error occurred
- type: Type of error encountered
- description: Error description

After the application captures an error, it will first display a pop-up prompt and provide a solution. If you do not want to handle the error, you can ignore it. When you disconnect and reconnect the device or enter the error log interface, clicking the "Clear" button will re-enable the pop-up prompt and save it to the error log table.

For example, capturing a joint 1 over-limit error:

<img width = "1200" align = "center" src="../../../resources/C-FunctionsAndApplications/5-BasicApplication/5.3-myStudioPro/home-14.png" />

1. When an abnormality occurs with the robotic arm, a specific warning pop-up window will be displayed. This pop-up window consists of four main parts: 1. The detailed content of the current abnormal error; 2. The solution method for the current abnormal error. If the current abnormality can be resolved or recovered, it will be displayed; otherwise, no content will be shown; 3. The `Restore` button for the current abnormal error that can be cleared or recovered. Clicking this button will automatically perform the repair process for the abnormality; otherwise, no button will be displayed; 4. The `Confirm` button for the current abnormality. If you do not want to handle the error, you can click this button to ignore the current abnormality.

<img width = "1200" align = "center" src="../../../resources/C-FunctionsAndApplications/5-BasicApplication/5.3-myStudioPro/home-15.png" />

2. Restoreed exceptions will be displayed in the historical alarm table of the exception list, while the current existing exceptions will be shown in the current alarm table. At the same time, the duration of the exception occurrence will be automatically recorded.

<img width = "1200" align = "center" src="../../../resources/C-FunctionsAndApplications/5-BasicApplication/5.3-myStudioPro/home-16.png" />

3. (1) The robot `Download log` button automatically retrieves logs from the past 1 day up to the current time, and downloads the log file in `.log` format to the corresponding storage location. If the exception is repairable, you can click the `Restore` button (2) to perform the anomaly repair operation. Clicking the `Clear record` button (3) will clear the historical alarm records that have been resolved.

<img width = "1200" align = "center" src="../../../resources/C-FunctionsAndApplications/5-BasicApplication/5.3-myStudioPro/home-17.png" />

---

## Robot status

Function: Display the current operating status of the robot

| Color | meaning |
| ---- | ------------------------------------------------------------ |
| <img width = "60"  align="center" src="../../../resources/C-FunctionsAndApplications/5-BasicApplication/5.3-myStudioPro/notConnected.svg" />| Not connected |
| <img width = "60"  align="center" src="../../../resources/C-FunctionsAndApplications/5-BasicApplication/5.3-myStudioPro/standby.svg" />    | Connecting |
| <img width = "60"  align="center" src="../../../resources/C-FunctionsAndApplications/5-BasicApplication/5.3-myStudioPro/running.svg" />    | Running |
| <img width = "60"  align="center" src="../../../resources/C-FunctionsAndApplications/5-BasicApplication/5.3-myStudioPro/error.svg" />    | Robot error |

---

[← Previous Chapter](../5.2-minirobot/5.2.9-Q&A.md) | [Next Chapter →](./5.3.1-firstUse.md)
