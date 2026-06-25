# Robotic Arm Anomaly Viewing and Handling Methods

When the robotic arm fails to execute a motion command, the corresponding exception information can be queried in the Python terminal. For example:

## Reading Robot Status

### Status Feedback Analysis

Normally, this interface returns `ok`. The robot status is read as follows:

```python
from ultraArm_P1_lib import UltraArmP1

ua = UltraArmP1('COM3', 1000000) # Serial port connection communication

ua.get_error_information() # Read robot status

```

Returns:

```bash
ok
```

This indicates that the robot has no joints exceeding limits, no joint hardware errors, and no joint software errors. If there are any errors or exceptions, the corresponding error information will be returned.

```python from ultraArm_P1_lib import UltraArmP1

ua = UltraArmP1('COM3', 1000000) # Serial port connection communication

ua.get_error_information() # Read robot status

```

Returns:

```bash
ok
```

This indicates that the robot has no joints exceeding limits, no joint hardware errors, and no joint software errors. If there are any errors or exceptions, the corresponding error information will be returned.` ### Troubleshooting

#### Joint Exceeding Limits

When a joint exceeds its limits, one of the following solutions can be used:

- Execute the over-limit reset interface:

```bash
ua.forced_reset_zero() # The robot will return to its origin slowly

```

- Execute joint release, manually moving the joint within its limits

```bash
ua.set_joint_release(0)

```

#### Joint Hardware Errors

Most hardware errors can be recovered using exception handling.

```bash
ua.clear_error_status()

```

If this problem persists after using exception handling or restarting the robot, please contact our engineers.

#### Joint Errors

Detailed software error reports for each joint:

| Bit (1 byte) | Error Status |
| :-----------: | :---------: |
| 0 | J1 Joint Overlimit (1-01-1) |
| 1 | J2 Joint Overlimit (1-01-2) |
| 2 | J3 Joint Overlimit (1-01-3) |
| 3 | J4 Joint Overlimit (1-01-4) |
| 4 | J1 Joint Collision (1-81-1) |
| 5 | J2 Joint Collision (1-81-2) |
| 6 | J3 Joint Collision (1-81-3) |
| 7 | J4 Joint Collision (1-81-4) |
| 8 | J1 Encoder Error (3-08-1) |
| 9 | J2 Encoder Error (3-08-2) |
| 10 | J3 encoder error (3-08-3) |
| 11 | J4 encoder error (3-08-4) |
| 12 | J1 joint dysfunction (3-09-1) |
| 13 | J2 joint dysfunction (3-09-2) |
| 14 | J3 joint dysfunction (3-09-3) |
| 15 | J4 joint dysfunction (3-09-4) |
| 16 | J1 drive board overheating (2-12-1) |
| 17 | J2 drive board overheating (2-12-2) |
| 18 | J3 drive board overheating (2-12-3) |
| 19 | J4 drive board overheating (2-12-4) |
| 20 | J5 (conveyor belt) drive board overheating (2-12-5) |
| 21 | J2, J3 coupling (1-50-0) |
| 22 | No solution for coordinates (1-32-0) |

---

[← Previous Page](./6_gripper.md) | [Next Section →](../6.2-ROS1/README.md)