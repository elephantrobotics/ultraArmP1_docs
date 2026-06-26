# Communication Protocol Package

## Gcode Command Introduction (for Serial, WiFi, Bluetooth Communication)

### Communication Methods

#### Serial Communication

| Parameter | Value |
|-----------|-------|
| Baud Rate | 115200/1000000 (configurable via command or screen settings) |
| Data Bits | 8 |
| Stop Bits | 1 |
| Parity | None |

#### WiFi Communication

- Communication Mode: TCP Client
- Connection: External device connects to the same WiFi network as the screen, then connects as TCP client to the screen's IP address and port
- Data Format: Same as serial communication, using Gcode command format

#### Bluetooth Communication

- Communication Mode: Bluetooth
- Connection: After the screen enables Bluetooth, external devices can search and connect to the screen's Bluetooth device
- Data Format: Same as serial communication, using Gcode command format

### Command Format

- `$` - Start marker, indicates data includes checksum
- `*` - Checksum separator
- `XX` - Two hexadecimal XOR checksum value (calculated by XORing all characters after `$` and before `*`)
- `\r\n` - Line terminator

#### Example
- Original command: `G0 X10`
- Command with checksum: `$G0 X10*2B\r\n` (assuming XOR result is 0x2B)

#### Response Format
- Success: `Command OK` or `Command ReturnValue`
- Failure: `Command ERROR:ErrorCode`
- Motion complete feedback: `EndEnd`

### Command Categories

#### Basic Control

1. Read Main Controller Version
Command: `G6`
Response: `G6 GetSystemVersion:12` (e.g., version V1.2)
Description: Returns major and minor version number (1 decimal place). Returns `G6 ERROR:0` when screen fails to get version.

2. Read Modify Version
Command: `G7`
Response: `G7 GetModifyVersion:5` (e.g., modify version 5)
Description: Returns modify version number. Returns `G7 ERROR:0` when screen fails to get version.

3. Read Error Status
Command: `G8`
Response: `G8 ERROR:uint32_t`
Description: Returns 32-bit error status, where each bit represents an error type. Refer to byte43-46 in "High Frequency Data Reading" for error codes.

4. Upgrade STM32 Firmware
Command: `G11`
Response: `G11 OK` / `G11 ERROR:0~4`
Description: Screen sends command to handshake with main controller. After successful handshake, main controller enters boot mode for data reception and upgrade.
Error cases:
- ERROR:0 - Bin file not found
- ERROR:1 - Failed to open upgrade firmware file
- ERROR:2 - Failed to enter STM32 upgrade mode
- ERROR:3 - SD card open failed
- ERROR:4 - File name mismatch

5. Read Motor Enable Status
Command: `M22`
Response: `M22 MotorEnable:1,1,1,1,1`
Description: 5 motor enable states (J1~J4 + conveyor belt), 1=enabled, 0=disabled

6. Release All Motors
Command: `M17 J[0-4]`
Parameters: J=0 release all joints, J=1~4 release corresponding joint
Response: `M17 OK` / `M17 ERROR:0`

7. Lock All Motors
Command: `M18 J[0-4]`
Parameters: J=0 lock all joints, J=1~4 lock corresponding joint
Response: `M18 OK` / `M18 ERROR:0`

8. Read Main Controller Running Status
Command: `M200`
Response: `M200 MainMoving:0` (0=stopped, 1=moving)

9. Get Execution Point Buffer Size
Command: `M600`
Response: `M600 Queue_size:50`

### Motion Control

- Speed range: 1-100 (percentage), each 1% corresponds to approximately 1.6 degrees/s
- Motion complete feedback: `EndEnd`
- Limit error auto-report: `LimitError:0` (1:J1 limit, 2:J2 limit, 4:J3 limit, 8:J4 limit, 16:J2/J3 coupling)
- Collision detection auto-report: `CollisionDetectionError:1` (1:J1, 2:J2, 3:J3)

#### 1. Coordinate Control (Max Speed)
Command: `G0`
Parameters: `X` X-axis coordinate(mm), `Y` Y-axis coordinate(mm), `Z` Z-axis coordinate(mm), `R` R-axis angle(deg), `F` Speed(1%-100%)
Response: `EndEnd`
Example: `G0 X100 Y200 Z300 F100`
Description: Move to target coordinate at maximum speed

#### 2. Coordinate Control (Specified Speed)
Command: `G1`
Parameters: `X` X-axis coordinate(mm), `Y` Y-axis coordinate(mm), `Z` Z-axis coordinate(mm), `R` R-axis angle(deg), `F` Speed(1%-100%)
Response: `EndEnd`
Example: `G1 X223.500 Y0 Z-250 F100`
Description: Move to target coordinate at specified speed

#### 3. Multi-Joint Control
Command: `G1`
Parameters: `A` J1 joint angle(deg), `B` J2 joint angle(deg), `C` J3 joint angle(deg), `D` J4 joint angle(deg), `F` Speed(1%-100%)
Response: `EndEnd`
Example: `G1 A10 B10 C10 F20`
Description: Each joint moves to target angle at specified speed

#### 4. Single Joint Control
Command: `G1`
Parameters: `A/B/C/D` Target joint angle(deg), `F` Speed(1%-100%)
Response: `EndEnd`
Example: `G1 A10 F20`
Description: Specified joint moves to target angle at specified speed

#### 5. Single Axis Control
Command: `G1`
Parameters: `X/Y/Z/R` Target coordinate value, `F` Speed(1%-100%)
Response: `EndEnd`
Example: `G1 X10 F20`
Description: End effector moves linearly along specified axis to target position

#### 6. Continuous Joint Motion (JogAngle)
Command: `M13`
Parameters: `J` Joint(1-4), `D` Direction(0-negative, 1-positive), `F` Speed(1%-100%)
Response: `EndEnd`
Example: `M13 J3 D0 F100`
Description: Specified joint moves continuously from current angle in specified direction until limit is reached

#### 7. Continuous Coordinate Motion (JogCoord)
Command: `M14`
Parameters: `J` Axis(1-X, 2-Y, 3-Z, 4-Rx), `D` Direction(0-negative, 1-positive), `F` Speed(1%-100%)
Response: `EndEnd`
Example: `M14 J3 D0 F100`
Description: End effector moves continuously along specified axis direction until limit is reached

#### 8. Joint Step Control
Command: `M19`
Parameters: `J` Joint(1-4), `T` Step value(deg), `F` Speed(1%-100%)
Response: `EndEnd`
Example: `M19 J3 T10 F100`
Description: Specified joint moves relative to current position by specified angle increment
- J1 step range: +/- (0.1~316)deg
- J2 step range: +/- (0.1~103)deg
- J3 step range: +/- (0.1~101)deg
- J4 step range: +/- (0.1~358)deg

#### 9. Coordinate Step Control
Command: `M20`
Parameters: `J` Axis(1-X, 2-Y, 3-Z, 4-Rx), `T` Step value(mm/deg), `F` Speed(1%-100%)
Response: `EndEnd`
Example: `M20 J3 T10 F100`
Description: End effector moves relative to current position by specified distance along specified axis
- X step range: +/- (0.1~316)mm
- Y step range: +/- (0.1~360)mm
- Z step range: 91~-157mm
- R step range: +/- 180deg

#### 10. Emergency Stop
Command: `M15`
Response: `M15 OK` / `M15 ERROR:0`
Description: Immediately stops current motion and clears all pending motion commands in buffer

#### 11. Unlock After Collision Detection
Command: `M5`
Response: `M5 OK` / `M5 ERROR:0`
Description: After collision, system enters locked state and cannot be motion controlled. This command releases the motion restriction.

#### 12. Force Homing
Command: `M37`
Response: `EndEnd` / `M37 ERROR:0`
Description: Robot first moves to A0B40C30D0 position, then to zero position (A0B0C90D0) to prevent coupling during homing

#### 13. Inverse Kinematics: Read Angles from Coordinates
Command: `M46`
Parameters: `X` X-axis coordinate(mm), `Y` Y-axis coordinate(mm), `Z` Z-axis coordinate(mm), `R` R-axis angle(deg)
Response: `M46 angle:A-3.501 B33.398 C156.709 D-167.168`
Error: `M46 ERROR:1`(limit), `M46 ERROR:2`(coupling), `M46 ERROR:3`(no solution)
Example: `M46 X215.517 Y-13.185 Z-60.710 R-170.669`

#### 14. Forward Kinematics: Read Coordinates from Angles
Command: `M47`
Parameters: `A` J1 joint angle(deg), `B` J2 joint angle(deg), `C` J3 joint angle(deg), `D` J4 joint angle(deg)
Response: `M47 coord:X215.516 Y-13.185 Z-60.710 R-170.669`
Error: `M47 ERROR:1`(limit), `M47 ERROR:2`(coupling)
Example: `M47 A-3.501 B33.398 C156.709 D-167.168`

#### 15. Preview Mode
Command: `M51`
Parameters: `X` X-axis coordinate(mm), `Y` Y-axis coordinate(mm), `Z` Z-axis coordinate(mm), `R` R-axis angle(deg)
Response: `M51 OK`(reachable) / `M51 ERROR:0`(unreachable)
Example: `M51 X223.500 Y0 Z50 R50`
Description: Input complete coordinates to check if reachable without actual motion

#### Auxiliary Functions

##### Configuration

1. Zero Calibration
Command: `M30`
Parameters: `J` Joint(0-4), 0-all joints, 1~4-corresponding joint
Response: `EndEnd`
Example: `M30 J1`
Description: Manually move robot to scale mark for joint calibration. Sets J1/J2/J4 current position to 0deg, J3 to 90deg.

2. Read Zero Calibration Status
Command: `M119`
Response: `M119 ZEROSTATE:0,0,0,0` (1=calibrated, 0=not calibrated)

3. Clear Zero Calibration Status
Command: `M32`
Parameters: `J` Joint(1-4)
Response: `M32 OK` / `M32 ERROR:0`

4. Set RGB Color
Command: `M23`
Parameters: `R` Red(0-255), `G` Green(0-255), `B` Blue(0-255)
Response: `M23 OK` / `M23 ERROR:0`
Example: `M23 R50 G50 B50`

5. End Effector Button Enable
Command: `M35`
Response: `M35 OK` / `M35 ERROR:0`
Description: After enabling, end effector button can release all joints

6. End Effector Button Disable
Command: `M36`
Response: `M36 OK` / `M36 ERROR:0`

7. Read End Effector Button Status
Command: `M64`
Response: `M64 BTN:0` (1=pressed, 0=released)

8. Get Collision Detection Threshold
Command: `M3`
Response: `M3 CollisionThreshold:0,0,0,0` (range: 0.5~5)

9. Set Collision Detection Threshold
Command: `M21`
Parameters: `J` Joint(0-4, 0=all), `P` Threshold(0.5~5deg)
Response: `M21 OK` / `M21 ERROR:0`
Example: `M21 J1 P2`

10. Clear Error Status
Command: `M40`
Response: `M40 OK` / `M40 ERROR:0`
Description: Clear all error status at once. Hardware errors need hardware repair before clearing.

##### IO Control

###### Base IO

1. Set Base IO Output
Command: `M60`
Parameters: `P` Pin number(1-10), `K` Mode(0-input, 1-output), `S` State(0-low, 1-high)
Response: `M60 OK` / `M60 ERROR:0`
Example: `M60 P5 K0 S0`
Description: Default 1-5 input, 6-10 output. In input mode, pull-up/down only provides default level, actual level determined by external signal.

2. Read Base IO Status
Command: `M61`
Response: `M61 IO:0,0,0,0,0,2,2,2,2,2`
Description: 0=input low, 1=input high, 2=output low, 3=output high

###### End Effector IO

1. Set End Effector IO Output
Command: `M62`
Parameters: `P` Pin number(3-4, controls IO3/IO4), `S` State(0-low, 1-high)
Response: `M62 OK` / `M62 ERROR:0`
Example: `M62 P3 S1`
Description: IO1, IO2 are input pins, IO3, IO4 are output pins

2. Read End Effector IO Status
Command: `M63`
Response: `M63 IO:0,0,0,0`
Description: 0=input low, 1=input high, 2=output low, 3=output high

##### Gripper Control

1. Control Gripper Angle
Command: `M25`
Parameters: `P` Angle(1-100, corresponding to 0.5~50deg), `F` Speed(1-100)
Response: `M25 OK` / `M25 ERROR:0`
Example: `M25 P50 F50`

2. Read Gripper Angle
Command: `M50`
Response: `M50 GRIPPERANGLE:50` (0-100, 100=open)

3. Set Gripper Parameters
Command: `M24`
Parameters: `J` Address(1-69), `K` Mode(1-value range 0-255, 2-value range >255), `L` Value(0-65535)
Response: `M24 OK` / `M24 ERROR:0`
Example: `M24 J5 K1 L20`

4. Read Gripper Parameters
Command: `M26`
Parameters: `J` Address(1-69), `K` Mode(1-value range 0-255, 2-value range >255)
Response: `M26 GripperParameters:0`

5. Read Gripper Motion State
Command: `M27`
Response: `M27 MotionState:0` (0=stopped, 1=moving)

6. Set Gripper Enable State
Command: `M28`
Parameters: `S` State(0=disabled, 1=enabled)
Response: `M28 OK` / `M28 ERROR:0`
Description: After enabling, manual movement will auto-reset

7. Gripper Calibration
Command: `M29`
Response: `M29 OK` / `M29 ERROR:0`
Description: Gripper opens to maximum stroke for calibration, current position becomes 100 after calibration

##### Vacuum Pump Control

1. Set Vacuum Pump State
Command: `M70`
Parameters: `S` State(0=suck, 1=blow, 2=off)
Response: `M70 OK` / `M70 ERROR:0`
Example: `M70 S1`

##### Laser/PWM Control

1. Laser On/Off
Command: `M80`
Parameters: `K` State(0=off, 1=on)
Response: `M80 OK` / `M80 ERROR:0`
Example: `M80 K1`

2. Set PWM Level (Laser)
Command: `M81`
Parameters: `S` Duty cycle(0-255)
Response: `M81 OK` / `M81 ERROR:0`
Example: `M81 S50`

3. Custom PWM On/Off
Command: `M82`
Parameters: `K` State(0=off, 1=on)
Response: `M82 OK` / `M82 ERROR:0`
Example: `M82 K1`

4. Set PWM Level (Custom)
Command: `M83`
Parameters: `S` Duty cycle(0-255)
Response: `M83 OK` / `M83 ERROR:0`
Example: `M83 S50`

5. Read PWM Settings
Command: `M84`
Response: `M84 PWM:0,0,0,0` (laser state, laser level, custom PWM state, custom PWM level)

##### Conveyor Control

1. Conveyor Control
Command: `M38`
Parameters: `J` On/Off(1=on, 0=off), `K` Direction(0=forward, 1=backward), `L` Speed(1-125mm/s), `S` Distance(0=continuous, 1-1200mm)
Response: `M38 OK` / `M38 ERROR:0`
Example: `M38 J1 K1 L20 S1000`

2. Conveyor Stop
Command: `M39`
Response: `M39 OK` / `M39 ERROR:0`
Description: Conveyor stops slowly when moving, deceleration time ~500ms

##### Status Query

1. Read Angles
Command: `M405`
Response: `M405 angles:0.00,0.00,0.00,0.00`

2. Read Coordinates
Command: `M406`
Response: `M406 coords:0.00,0.00,0.00,0.00`

3. Enable/Disable Periodic Angle Report
Command: `M407`
Parameters: `S` Mode(0=off, 1=periodic report via UART1, 2=periodic report via UART0, 3=periodic report via WiFi, 4=periodic report via Bluetooth)
Response: `M407 angles:...;coords:...;pwr:1;run:0;err:0`

4. Read Screen Major/Minor Version
Command: `M401`
Response: `M401 GetSystemVersion:12` (e.g., version V1.2)

5. Read Screen Modify Version
Command: `M402`
Response: `M402 GetModifyVersion:5` (e.g., modify version 5)

6. Get Screen Control Page Status
Command: `M601`
Response: `M601 ControlPage:1` (1=control page, 0=non-control page)

7. Notify Screen of PC Control Status
Command: `M602`
Parameters: `S` State(1=PC on control page, 0=PC not on control page)
Response: `M602 OK` / `M602 ERROR:0`

8. Get Limit Switch Status
Command: `M52`
Response: `M52 SIG:0`

##### I2C Communication

1. I2C Data Write
Command: `M300`
Parameters: `I` Session ID(0-255), `U` Packet ID(0-255), `S` Operation(1=write), `L` Slave Address(0-125), `H` Register Address(0-65535), `N` Data Length(1-255), `K` Data(hex, length determined by N)
Response: `M300 OK` / `M300 ERROR:0~7`
Example: `M300 I1 U1 S1 L13 H45 N2 K8f 1E`
Description: ERROR:1-missing required parameters, ERROR:2-data length N mismatch, ERROR:3-missing K parameter, ERROR:4-address error/device not connected, ERROR:5-slave no response, ERROR:6-invalid parameter, ERROR:7-binary frame CRC error

2. I2C Data Read
Command: `M300`
Parameters: `I` Session ID(0-255), `U` Packet ID(0-255), `S` Operation(0=read), `L` Slave Address(0-125), `H` Register Address(0-65535), `N` Data Length(1-255)
Response: `M300 data:0f 24` (read data) / `M300 ERROR:0~7`
Example: `M300 I1 U1 S0 L23 HFFFF N2`

## Modbus Command Introduction (for RS485 Communication)

### Communication Parameters

| Parameter | Value |
|-----------|-------|
| Baud Rate | 115200 |
| Data Bits | 8 |
| Stop Bits | 1 |
| Parity | None |
| Device Address | 46 (0x2E) |
| Write Function Code | 16 (0x10) |
| Read Function Code | 03 (0x03) |

> **Note**: Parity bit (UART hardware layer) and checksum (Modbus protocol layer) are different concepts:
> - **Parity Bit**: Hardware-level parity in UART serial communication. This protocol uses "no parity" (8N1 mode)
> - **Checksum**: Software-level checksum in Modbus protocol. Uses CRC-16 cyclic redundancy check, appended at the end of data frame

### Communication Frame Format

#### 1. Write Command (Master -> Device)

| Field | Length | Description |
|-------|--------|-------------|
| Address | 1 byte | Device address: 46 (0x2E) |
| Function Code | 1 byte | 0x10 (Write Multiple Registers) |
| Start Register Address | 2 bytes | Starting register address (high byte first) |
| Register Count | 2 bytes | Number of registers N to write |
| Data Length | 1 byte | Data length = 2xN |
| Data | 2xN bytes | Actual data (high byte first) |
| Checksum | 2 bytes | Modbus CRC16 (low byte first) |

**Format:** `[Address-0x2E][FunctionCode-0x10][RegisterAddress-2bytes][RegisterCount-2bytes][DataLength-1byte][Data-2*Nbytes][Checksum-2bytes]`

#### 2. Write Response (Device -> Master)

| Field | Length | Description |
|-------|--------|-------------|
| Address | 1 byte | 0x2E |
| Function Code | 1 byte | 0x10 |
| Start Register Address | 2 bytes | Starting register address |
| Register Count | 2 bytes | Number of registers written |
| Checksum | 2 bytes | Modbus CRC16 (low byte first) |

#### 3. Read Command (Master -> Device)

| Field | Length | Description |
|-------|--------|-------------|
| Address | 1 byte | 0x2E |
| Function Code | 1 byte | 0x03 |
| Start Register Address | 2 bytes | Starting register address |
| Register Count | 2 bytes | Number of registers N to read |
| Checksum | 2 bytes | Modbus CRC16 (low byte first) |

#### 4. Read Response (Device -> Master)

| Field | Length | Description |
|-------|--------|-------------|
| Address | 1 byte | 0x2E |
| Function Code | 1 byte | 0x03 |
| Data Length | 1 byte | Data length = 2xN |
| Data | 2xN bytes | Read data |
| Checksum | 2 bytes | Modbus CRC16 (low byte first) |

> **Notes:** Register address 00 xx indicates commands forwarded to STM32, 01 xx indicates screen local commands.
> Modbus uses 16-bit two's complement for signed integers.
> Checksum calculation uses Modbus CRC16, low byte first.

#### Examples

1. Read Main Controller Version: `2E 03 00 01 00 CRC1 CRC2`
   Response: `2E 03 00 01 02 00 0C CRC1 CRC2` (version V1.2)

2. Coordinate Control: `2E 10 00 04 0A XX XX XX XX XX XX XX XX XX XX CRC1 CRC2`
   Position feedback: `2E 10 00 5B 00 CRC1 CRC2`

### Modbus Command List

#### Basic Control

| Command (Reference Gcode) | Function Code | Register Address | Data Length | Data Format | Response Length | Response Format | Description |
|---------|---------------|------------------|-------------|-------------|-----------------|-----------------|-------------|
| G6 | 03 | 00 01 | 0 | - | 2 | byte1-2: Version (e.g., V1.2=12) | Read main controller version |
| G7 | 03 | 00 02 | 0 | - | 2 | byte1-2: Modify version | Read main controller modify version |
| G8 | 03 | 01 06 | 0 | - | 4 | byte1-4: 32-bit error status | Read error status (32-bit) |
| G11 | 16 | 01 08 | 0 | - | 2 | byte1: Success 1/Fail 0, byte2: Fail type | Upgrade STM32 firmware |
| M5 | 16 | 00 09 | 0 | - | 2 | byte1: Success 1/Fail 0, byte2: Fail type | Unlock after collision detection |
| M17 | 16 | 00 10 | 2 | byte1-2: J(0-4, 0=all joints) | 2 | byte1: Success 1/Fail 0, byte2: Fail type | Release motors |
| M18 | 16 | 00 11 | 2 | byte1-2: J(0-4, 0=all joints) | 2 | byte1: Success 1/Fail 0, byte2: Fail type | Lock motors |
| M22 | 03 | 00 12 | 0 | - | 5 | byte1-5: J1~J4+conveyor enable(1=enabled,0=disabled) | Read motor enable status |
| M200 | 03 | 01 07 | 0 | - | 1 | byte1: 1=moving,0=stopped | Read main controller running status |
| M600 | 03 | 01 09 | 0 | - | 2 | byte1-2: Buffer size | Get execution point buffer size |

#### Motion Control

| Command (Reference Gcode) | Function Code | Register Address | Data Length | Data Format | Response Length | Response Format | Description |
|---------|---------------|------------------|-------------|-------------|-----------------|-----------------|-------------|
| G0 | 16 | 00 03 | 10 | byte1-2:X*100, byte3-4:Y*100, byte5-6:Z*100, byte7-8:R*100, byte9-10:F*100 | - | Position feedback: `2E 10 00 5B 00 CRC` | Coordinate control (max speed) |
| G1 | 16 | 00 04 | 10 | byte1-2:X*100, byte3-4:Y*100, byte5-6:Z*100, byte7-8:R*100, byte9-10:F*100 | - | Position feedback: `2E 10 00 5B 00 CRC` | Coordinate control (specified speed) |
| G1 | 16 | 00 05 | 10 | byte1-2:A*100, byte3-4:B*100, byte5-6:C*100, byte7-8:D*100, byte9-10:F*100 | - | Position feedback: `2E 10 00 5B 00 CRC` | Multi-joint control |
| G1 | 16 | 00 06 | 6 | byte1-2:Axis(1-4=X-R), byte3-4:Data*100, byte5-6:F*100 | - | Position feedback: `2E 10 00 5B 00 CRC` | Single axis control |
| G1 | 16 | 00 07 | 6 | byte1-2:Joint(1-4=A-D), byte3-4:Angle*100, byte5-6:F*100 | - | Position feedback: `2E 10 00 5B 00 CRC` | Single joint control |
| M13 | 16 | 00 0A | 6 | byte1-2:Joint(1-4), byte3-4:Dir(0=neg,1=pos), byte5-6:F*100 | - | Position feedback: `2E 10 00 5B 00 CRC` | Continuous joint motion |
| M14 | 16 | 00 0B | 6 | byte1-2:Axis(1-4=X-R), byte3-4:Dir(0=neg,1=pos), byte5-6:F*100 | - | Position feedback: `2E 10 00 5B 00 CRC` | Continuous coordinate motion |
| M15 | 16 | 00 0F | 0 | - | 2 | byte1: Success 1/Fail 0, byte2: Fail type | Emergency stop |
| M19 | 16 | 00 0C | 6 | byte1-2:Joint(1-4), byte3-4:Step*100, byte5-6:F*100 | - | Position feedback: `2E 10 00 5B 00 CRC` | Joint step control |
| M20 | 16 | 00 0D | 6 | byte1-2:Axis(1-4=X-R), byte3-4:Step*100, byte5-6:F*100 | - | Position feedback: `2E 10 00 5B 00 CRC` | Coordinate step control |
| M37 | 16 | 00 1A | 0 | - | 2 | byte1: Success 1/Fail 0, byte2: Fail type | Force homing |
| M46 | 03 | 00 33 | 8 | byte1-2:X*100, byte3-4:Y*100, byte5-6:Z*100, byte7-8:R*100 | 8 | byte1-2:A*100, byte3-4:B*100, byte5-6:C*100, byte7-8:D*100 | Inverse kinematics |
| M47 | 03 | 00 34 | 8 | byte1-2:A*100, byte3-4:B*100, byte5-6:C*100, byte7-8:D*100 | 8 | byte1-2:X*100, byte3-4:Y*100, byte5-6:Z*100, byte7-8:R*100 | Forward kinematics |
| M51 | 03 | 00 1D | 8 | byte1-2:X*100, byte3-4:Y*100, byte5-6:Z*100, byte7-8:R*100 | 2 | byte1: Reachable 1/Unreachable 0, byte2: Fail type | Preview mode |

#### Configuration

| Command (Reference Gcode) | Function Code | Register Address | Data Length | Data Format | Response Length | Response Format | Description |
|---------|---------------|------------------|-------------|-------------|-----------------|-----------------|-------------|
| M23 | 16 | 00 13 | 6 | byte1-2:R(0-255), byte3-4:G(0-255), byte5-6:B(0-255) | 2 | byte1: Success 1/Fail 0, byte2: Fail type | RGB end effector light control |
| M30 | 16 | 00 14 | 2 | byte1-2:J(0-4, 0=all joints) | - | Position feedback: `2E 10 00 5B 00 CRC` | Zero calibration |
| M32 | 16 | 00 16 | 2 | byte1-2:J(1-4) | 2 | byte1: Success 1/Fail 0, byte2: Fail type | Clear zero calibration status |
| M35 | 16 | 00 18 | 0 | - | 2 | byte1: Success 1/Fail 0, byte2: Fail type | End effector button enable |
| M36 | 16 | 00 19 | 0 | - | 2 | byte1: Success 1/Fail 0, byte2: Fail type | End effector button disable |
| M40 | 16 | 00 1C | 0 | - | 2 | byte1: Success 1/Fail 0, byte2: Fail type | Clear error status |
| M119 | 03 | 00 1F | 0 | - | 4 | byte1-4: J1~J4 calibration(1=calibrated,0=not) | Read zero calibration status |

#### IO Control

| Command (Reference Gcode) | Function Code | Register Address | Data Length | Data Format | Response Length | Response Format | Description |
|---------|---------------|------------------|-------------|-------------|-----------------|-----------------|-------------|
| M60 | 16 | 00 28 | 6 | byte1-2:P(1-10), byte3-4:K(0=input,1=output), byte5-6:S(0=low,1=high) | 2 | byte1: Success 1/Fail 0, byte2: Fail type | Set base IO output |
| M61 | 03 | 00 29 | 0 | - | 10 | byte1-10: 10 IO states(0=input low,1=input high,2=output low,3=output high) | Read base IO status |
| M62 | 16 | 00 2A | 4 | byte1-2:P(3-4), byte3-4:S(0=low,1=high) | 2 | byte1: Success 1/Fail 0, byte2: Fail type | Set end effector IO output |
| M63 | 03 | 00 2B | 0 | - | 4 | byte1-4: 4 IO states(0=input low,1=input high,2=output low,3=output high) | Read end effector IO status |
| M64 | 03 | 01 0C | 0 | - | 1 | byte1: 1=pressed,0=released | Read end effector button status |

#### Gripper Control

| Command (Reference Gcode) | Function Code | Register Address | Data Length | Data Format | Response Length | Response Format | Description |
|---------|---------------|------------------|-------------|-------------|-----------------|-----------------|-------------|
| M24 | 16 | 00 22 | 4 | byte1-2:J(1-69), byte3-4:L(0-65535) | 2 | byte1: Success 1/Fail 0, byte2: Fail type | Set gripper parameters |
| M25 | 16 | 00 21 | 4 | byte1-2:P(1-100)*100, byte3-4:F(1-100) | 2 | byte1: Success 1/Fail 0, byte2: Fail type | Control gripper angle |
| M26 | 03 | 00 23 | 2 | byte1-2:J(1-69) | 2 | byte1-2: Gripper parameter value | Read gripper parameters |
| M27 | 03 | 00 24 | 0 | - | 2 | byte1-2: Motion state(0=stopped,1=moving) | Read gripper motion state |
| M28 | 16 | 00 25 | 2 | byte1-2:S(0=disabled,1=enabled) | 2 | byte1: Success 1/Fail 0, byte2: Fail type | Set gripper enable state |
| M29 | 16 | 00 26 | 0 | - | 2 | byte1: Success 1/Fail 0, byte2: Fail type | Gripper zero calibration |
| M50 | 03 | 00 20 | 0 | - | 2 | byte1-2: Gripper angle*100 | Read gripper angle |

#### Vacuum Pump/Laser/PWM/Conveyor

| Command (Reference Gcode) | Function Code | Register Address | Data Length | Data Format | Response Length | Response Format | Description |
|---------|---------------|------------------|-------------|-------------|-----------------|-----------------|-------------|
| M70 | 16 | 00 27 | 2 | byte1-2:S(0=suck,1=blow,2=off) | 2 | byte1: Success 1/Fail 0, byte2: Fail type | Set vacuum pump state |
| M80 | 16 | 00 2C | 2 | byte1-2:K(0=off,1=on) | 2 | byte1: Success 1/Fail 0, byte2: Fail type | Laser on/off |
| M81 | 16 | 00 2D | 2 | byte1-2:S(0-255) | 2 | byte1: Success 1/Fail 0, byte2: Fail type | Set PWM level (laser) |
| M82 | 16 | 00 2E | 2 | byte1-2:K(0=off,1=on) | 2 | byte1: Success 1/Fail 0, byte2: Fail type | Custom PWM on/off |
| M83 | 16 | 00 2F | 2 | byte1-2:S(0-255) | 2 | byte1: Success 1/Fail 0, byte2: Fail type | Set PWM level (custom) |
| M84 | 03 | 00 32 | 0 | - | 4 | byte1:Laser state, byte2:Laser level, byte3:Custom PWM state, byte4:Custom PWM level | Read PWM settings |
| M38 | 16 | 00 1B | 10 | byte1-2:J(0-1), byte3-4:K(0=forward,1=backward), byte5-8:L(50-500000), byte9-10:S(0=continuous,1-1200mm) | 2 | byte1: Success 1/Fail 0, byte2: Fail type | Conveyor control |
| M39 | 16 | 00 35 | 0 | - | 2 | byte1: Success 1/Fail 0, byte2: Fail type | Conveyor stop |

#### I2C Communication

| Command (Reference Gcode) | Function Code | Register Address | Data Length | Data Format | Response Length | Response Format | Description |
|---------|---------------|------------------|-------------|-------------|-----------------|-----------------|-------------|
| M300 Write | 16 | 00 36 | N+6 | byte1:I(0-255), byte2:U(0-255), byte3:L(0-125), byte4-5:H(0-65535), byte6:N(1-255), byte7-(6+N):K data | 2 | byte1: Success 1/Fail 0, byte2: Fail type | I2C data write |
| M300 Read | 03 | 00 37 | 6 | byte1:I(0-255), byte2:U(0-255), byte3:L(0-125), byte4-5:H(0-65535), byte6:N(1-255) | 1-255 | byte1~N: Read data (hex) | I2C data read |

#### Status Query

| Command (Reference Gcode) | Function Code | Register Address | Data Length | Data Format | Response Length | Response Format | Description |
|---------|---------------|------------------|-------------|-------------|-----------------|-----------------|-------------|
| M52 | 03 | 00 38 | 0 | - | 1 | byte1: Limit switch value | Get limit switch status |

#### Screen Related

| Command (Reference Gcode) | Function Code | Register Address | Data Length | Data Format | Response Length | Response Format | Description |
|---------|---------------|------------------|-------------|-------------|-----------------|-----------------|-------------|
| M401 | 03 | 01 01 | 0 | - | 2 | byte1-2: Screen version | Read screen major/minor version |
| M402 | 03 | 01 02 | 0 | - | 2 | byte1-2: Screen modify version | Read screen modify version |
| M405 | 03 | 01 04 | 0 | - | 8 | byte1-2:J1*100, byte3-4:J2*100, byte5-6:J3*100, byte7-8:J4*100 | Read angles |
| M406 | 03 | 01 05 | 0 | - | 8 | byte1-2:X*100, byte3-4:Y*100, byte5-6:Z*100, byte7-8:R*100 | Read coordinates |
| M601 | 03 | 01 0A | 0 | - | 2 | byte1: 1=control page,0=non-control page, byte2: Fail type | Get screen control page status |
| M602 | 16 | 01 0B | 2 | byte1-2:S(1=PC controlling,0=PC not controlling) | 2 | byte1: Success 1/Fail 0, byte2: Fail type | Notify screen of PC control status |

#### Auto Report

| Event | Function Code | Register Address | Data Length | Data Format | Response Length | Response Format | Description |
|-------|---------------|------------------|-------------|-------------|-----------------|-----------------|-------------|
| Limit Error | 16 | 00 30 | - | - | 1 | byte1: Error value(1=J1,2=J2,4=J3,8=J4,16=coupling) | Auto-report when limit triggered |
| Collision Detection | 16 | 00 31 | - | - | 1 | byte1: Collision joint(1=J1,2=J2,3=J3) | Auto-report when collision detected |

## Error Messages

### Error Status Bits (High Frequency Data Reading byte43-46, 32 bits total)

| Bit | Description | Bit | Description | Bit | Description |
|-----|-------------|-----|-------------|-----|-------------|
| 0 | J1 joint limit | 8 | J1 encoder error | 16 | J1 driver overtemp |
| 1 | J2 joint limit | 9 | J2 encoder error | 17 | J2 driver overtemp |
| 2 | J3 joint limit | 10 | J3 encoder error | 18 | J3 driver overtemp |
| 3 | J4 joint limit | 11 | J4 encoder error | 19 | J4 driver overtemp |
| 4 | J1 joint collision | 12 | J1 joint disabled | 20 | Conveyor driver overtemp |
| 5 | J2 joint collision | 13 | J2 joint disabled | 21 | J2/J3 coupling |
| 6 | J3 joint collision | 14 | J3 joint disabled | 22 | No coordinate solution |
| 7 | J4 joint collision | 15 | J4 joint disabled | 23~31 | Reserved |

### Limit Error (LimitError)

| Value | Description |
|-------|-------------|
| 0 | Normal |
| 1 | J1 joint limit |
| 2 | J2 joint limit |
| 4 | J3 joint limit |
| 8 | J4 joint limit |
| 16 | J2/J3 coupling |

### Collision Detection Error (CollisionDetectionError)

| Value | Description |
|-------|-------------|
| 1 | J1 joint collision |
| 2 | J2 joint collision |
| 3 | J3 joint collision |

### No Coordinate Solution

| Value | Description |
|-------|-------------|
| 1 | No solution (auto-report after moving to farthest reachable position) |

### Inverse/Forward Kinematics Errors

| Error Code | Description |
|------------|-------------|
| ERROR:1 | Joint limit |
| ERROR:2 | Joint coupling |
| ERROR:3 | No kinematic solution |

## FAQ

1. Robot cannot move?
   - Check if robot is enabled (M22), check for motor errors (G8), check for joint limits
   - Execute M5 to unlock after collision

2. No response to commands?
   - Check if XOR checksum is correct
   - Check if command format matches `$Command*XX\r\n` specification
   - Check if baud rate matches

3. No coordinate solution?
   - Check if target coordinates are within robot workspace
   - Use M51 preview mode to verify reachability
   - Use joint control to move away from singularity

---

[← Previous Chapter](../6.3-ROS2/6.3.5-Troubleshooting.md) | [Next Chapter →](../../7.SuccessfulCase/README.md)
