# Detailed Design


## Function of the Subsystem

The internal components subsystem is the navigational core of the autonomous acoustic measurement drone. It handles flight stabilization, indoor positioning, waypoint navigation, and obstacle detection.
Pixhawk 6C Mini [1] — Central flight controller. Manages stabilization, executes the predefined waypoint mission, and coordinates all onboard sensors throughout flight.
Holybro H-Flow [2] — Provides indoor position hold via optical flow and downward distance sensing, replacing GPS which is unreliable in enclosed venues.
RPLIDAR C1 [3] — Performs continuous 360° horizontal scanning. The flight controller uses this data to maintain a minimum safe clearance from any detected obstacle during navigation.
Together these three components form the navigation stack that enables safe, stable, and repeatable autonomous flight within indoor performance venues.

## Specifications and Constraints

### Specifications

| Parameter | Value |
|---|---|
| Positional accuracy | ±0.5 m at each waypoint |
| Minimum obstacle clearance | 3 m in all horizontal directions |
| RPLIDAR C1 detection range | ≥ 6 m |
| Combined subsystem mass | ≤ 200 g |
| H-Flow operation | Indoor, GPS-free position hold |
| Waypoint navigation | Fully autonomous, no manual input required |

### Constraints

**Physics/Hardware** — The H-Flow [2] relies on surface texture and lighting conditions beneath the drone. Performance may degrade on reflective or featureless floors common in some venue environments.

**Subsystem Prerequisites** — The RPLIDAR C1 [3] must be mounted with a fixed forward reference aligned to the drone's heading axis to ensure accurate directional obstacle response. The H-Flow [2] must have an unobstructed downward view of the floor at all times.

**Standards** — The system shall comply with FAA 14 CFR Part 107 [4], which governs autonomous drone operation including altitude limits, failsafe requirements, and operational safety. This directly constrains how the flight controller failsafes and speed limits are configured.

**Socioeconomic** — Component selection is constrained by the project budget. All three components were selected as commercially available COTS hardware to minimize cost and development time.



## Overview of Proposed Solution

The internal components subsystem combines three COTS hardware components into a unified navigation stack: the Pixhawk 6C Mini flight controller [1], the Holybro H-Flow optical flow module [2], and the SLAMTEC RPLIDAR C1 scanning lidar [3].

The Pixhawk 6C Mini [1] runs ArduPilot firmware and manages all flight operations. A predefined waypoint mission is uploaded prior to flight, and the Pixhawk executes it autonomously — navigating to each measurement position, holding hover during data collection, and proceeding to the next waypoint without manual input.

The H-Flow [2] connects to the Pixhawk via DroneCAN and provides continuous optical flow velocity and downward distance data. This enables stable indoor position hold and altitude control without GPS.

The RPLIDAR C1 [3] connects via TTL UART to the Pixhawk's TELEM2 port and is configured in ArduPilot as a 360° proximity sensor using the Lidar360 serial protocol (`SERIAL2_PROTOCOL = 11`, `SERIAL2_BAUD = 460800`, and `PRX1_TYPE = 5`) [9]. During autonomous waypoint navigation, ArduPilot's BendyRuler object-avoidance algorithm is enabled using `OA_TYPE = 1`. Because the RPLIDAR C1 provides horizontal obstacle measurements, Horizontal BendyRuler (`OA_BR_TYPE = 1`) is used to search for alternate obstacle-free paths while continuing toward the commanded waypoint [10]. The obstacle-avoidance margin (`OA_MARGIN_MAX`) will be configured to the subsystem's required 3 m clearance, while the BendyRuler look-ahead distance (`OA_BR_LOOKAHEAD`) will be initially configured to 5 m and verified during indoor testing. When an obstacle enters the planned path, BendyRuler evaluates alternate horizontal directions and commands the vehicle along a locally adjusted path before continuing toward the original mission destination.

This configuration meets the intended positional accuracy, obstacle avoidance, and weight requirements while remaining within budget and minimizing integration complexity.

## Interface with Other Subsystems

The internal components subsystem interfaces primarily with the power and propulsion subsystem, the controller subsystem, and the external components subsystem. All communication between the Pixhawk 6C Mini [1] and its connected sensors uses digital protocols native to ArduPilot/PX4 firmware [7], minimizing integration complexity and ensuring reliable real-time data exchange.

| Interface | Signal Type | Direction | Protocol / Format | Data |
|---|---|---|---|---|
| H-Flow → Pixhawk 6C Mini | Digital | Input | DroneCAN (CAN1/CAN2) | Optical flow velocity, downward distance |
| RPLIDAR C1 → Pixhawk 6C Mini | Digital | Input | TTL UART (TELEM2) | 360° obstacle distance and angle data |
| Pixhawk 6C Mini → ESCs | Digital | Output | PWM | Motor speed commands |
| Power Module → Pixhawk 6C Mini | Electrical (DC) | Input | Regulated 5V | Flight controller power |
| Power Module → Pixhawk 6C Mini | Digital | Input | Analog/ADC | Battery voltage and current telemetry |
| mLRS Receiver → Pixhawk 6C Mini | Digital | Input | CRSF / SBUS / MAVLink | RC control inputs and supervisory commands |
| Pixhawk 6C Mini → mLRS Receiver | Digital | Output | MAVLink (serial) | Telemetry, mode state, mission progress, fault data |

The H-Flow [2] connects via DroneCAN, supplying optical flow velocity and downward distance data for indoor position hold. The RPLIDAR C1 [3] transmits 360° scan data via TTL UART on TELEM2, enabling real-time obstacle detection. The Pixhawk [1] outputs PWM commands to the ESCs, translating control decisions into thrust. Power and battery telemetry are received from the power module. The mLRS receiver carries RC inputs and supervisory commands inbound and MAVLink telemetry outbound to the controller subsystem.

## 3D Model of Custom Mechanical Components

The following models show the placement and integration of the internal components subsystem within the drone assembly. All three components are mounted to custom 3D-printed brackets designed to satisfy placement, clearance, and vibration isolation requirements.

![Full Assembly Transparent View](https://github.com/TnTech-ECE/S26_Team5_Acoustic-Measurement-Drone/blob/Rough_Draft_Project/Reports/Images/e/xray.png)
*Figure 1: Transparent full assembly view showing the placement of all three internal components relative to the drone frame. The Pixhawk 6C Mini is mounted centrally on the upper deck, the H-Flow is positioned on the underside for unobstructed downward view, and the RPLIDAR C1 is mounted at the rear of the lower deck with full horizontal clearance.*

![Pixhawk 6C Mini Closeup](https://github.com/TnTech-ECE/S26_Team5_Acoustic-Measurement-Drone/blob/Rough_Draft_Project/Reports/Images/e/Controla.png)
*Figure 2: Pixhawk 6C Mini mounted centrally on the upper deck of the frame. Center placement minimizes the offset between the IMU and the drone's center of mass, reducing attitude estimation error during flight.*

![H-Flow Closeup](https://github.com/TnTech-ECE/S26_Team5_Acoustic-Measurement-Drone/blob/Rough_Draft_Project/Reports/Images/e/Hflow.png)
*Figure 3: Holybro H-Flow mounted on the underside of the lower deck, facing downward with an unobstructed view of the floor surface. This placement satisfies the requirement for reliable optical flow and distance sensing during indoor position hold.*

![RPLIDAR C1 Closeup](https://github.com/TnTech-ECE/S26_Team5_Acoustic-Measurement-Drone/blob/Rough_Draft_Project/Reports/Images/e/rp.png)
*Figure 4: SLAMTEC RPLIDAR C1 mounted at the rear of the lower deck with full 360° horizontal clearance. The fixed forward reference is aligned to the drone's heading axis to ensure accurate directional obstacle response.*


## Buildable Schematic

The wiring diagram below shows all electrical and digital connections within the internal components subsystem and its interfaces to adjacent subsystems. Colored boxes indicate components owned by this subsystem. Gray boxes indicate external subsystem interfaces.

![Wiring Diagram](https://github.com/TnTech-ECE/S26_Team5_Acoustic-Measurement-Drone/blob/Rough_Draft_Project/Reports/Images/e/flowchaty.png)
*Figure 5: Internal components subsystem wiring diagram.*


## Flowchart

The following flowchart illustrates the decision-making logic of the Pixhawk 6C Mini [1] throughout an autonomous measurement mission.

![Flowchart](https://github.com/TnTech-ECE/S26_Team5_Acoustic-Measurement-Drone/blob/Rough_Draft_Project/Reports/Images/e/flowy.png)
*Figure 6: Internal components subsystem operational flowchart.*








## Sensor Failure and Fallback Behavior

The internal components subsystem uses ArduPilot sensor-health monitoring and failsafe functions to prevent a single sensor failure from causing uncontrolled autonomous flight. Before takeoff, ArduPilot pre-arm checks verify that required navigation and proximity sensors are connected and producing valid data. During flight, sensor health and estimator quality are continuously monitored. If a critical sensor becomes unavailable, autonomous waypoint navigation is suspended and the aircraft transitions to a degraded operating mode that prioritizes operator control and safe recovery.

| Sensor / Failure | Failure Detection | Fallback Behavior | Mitigation |
|---|---|---|---|
| RPLIDAR C1 communication or measurement failure | ArduPilot's RPLIDAR driver changes the proximity sensor state to `NoData` if valid distance data are not received for 200 ms. The driver also attempts to reset the RPLIDAR if the fault persists [11]. | Autonomous waypoint translation and BendyRuler navigation are suspended. A Lua safety script will monitor the ArduPilot proximity status and command AltHold when the sensor is no longer reported as healthy. The operator is alerted and takes manual control before continuing or landing. | ArduPilot pre-arm proximity checks prevent the mission from beginning if the sensor reports `NoData` or `NotConnected`. The RPLIDAR driver also includes an automatic reset attempt for persistent communication failures [11], [12]. |
| H-Flow optical-flow failure or unreliable horizontal position estimate | ArduPilot's EKF monitors position, velocity, and sensor consistency. If the navigation solution becomes unreliable, the EKF failsafe is triggered [13]. | The autonomous mission is aborted and `FS_EKF_ACTION` is configured for AltHold. The operator then assumes manual control and moves the aircraft to a safe landing location. If safe recovery cannot be maintained, a controlled landing is performed. | The H-Flow is mounted with an unobstructed downward view and its health is verified before flight. EKF failsafe monitoring provides an independent method of detecting an unreliable navigation solution. |
| H-Flow downward distance measurement failure | Loss or invalidity of downward range data prevents reliable comparison between the expected floor position and the measured surface beneath the aircraft. | Automatic landing-zone obstruction protection is disabled. The aircraft shall not continue an unsupervised autonomous landing; the operator must verify that the landing area is clear and supervise or manually perform the landing. | Downward range data are checked before an autonomous landing begins. Loss of this measurement produces a fault indication to the operator rather than allowing the system to assume that the landing zone is clear. |
| Single Pixhawk IMU failure | ArduPilot's estimator monitors IMU consistency and estimator health. | The Pixhawk's redundant IMU architecture allows the estimator to continue using a healthy sensor if a valid navigation solution remains available. If estimator health also becomes unacceptable, the EKF failsafe sequence is initiated. | The Pixhawk 6C Mini contains redundant inertial sensors, reducing dependence on a single IMU. |
| Barometer or altitude-estimation failure | ArduPilot monitors estimator consistency and sensor health through the EKF. | Autonomous waypoint operation is terminated. Because reliable altitude hold may no longer be available, the system will not depend on AltHold as the sole recovery method. The operator assumes manual control and performs a controlled landing as soon as safely possible. | Barometer data are cross-checked within the EKF against inertial and available downward range measurements, and sensor health is monitored before and during flight. |
| Multiple critical navigation-sensor failures | Loss of a trustworthy EKF solution or simultaneous critical sensor-health faults. | The autonomous mission is terminated. No additional waypoints are attempted, and the aircraft transitions to the safest available operator-controlled recovery mode followed by a controlled landing. | Multiple independent sensors, pre-arm checks, EKF health monitoring, manual override capability, and continuous telemetry reduce the likelihood that a single fault progresses into complete loss of control. |

For the RPLIDAR C1 specifically, ArduPilot's source code defines a 200 ms communication timeout. If no valid distance measurement is received within this period, the proximity sensor status is changed to `NoData`. If the condition persists, the driver attempts to reset the RPLIDAR after 10 seconds [11]. ArduPilot also prevents arming when a configured proximity sensor reports `NoData` or `NotConnected` [12]. A Lua safety script will use the exposed proximity-health status to supplement this built-in monitoring and suspend autonomous navigation if the C1 becomes unhealthy during flight.

For localization failures, the ArduPilot EKF failsafe will be used as the primary protection against an unreliable position estimate. The `FS_EKF_ACTION` parameter will be configured to select AltHold following an EKF failsafe during autonomous operation [13]. This provides the operator an opportunity to take manual control rather than commanding an immediate landing at an unknown location. If the aircraft cannot be safely recovered in AltHold or another manually controlled mode, the operator will command a controlled landing.

This layered approach ensures that sensor failures result in progressively safer degraded operation rather than continued autonomous navigation with unreliable sensor data.






## Indoor Test Plan and Pass/Fail Criteria

Indoor validation of the navigation and obstacle-avoidance system will be conducted in Memorial Gym on campus. Initial tuning and low-risk flight testing may be performed outdoors on the university football field prior to indoor validation. The indoor test will evaluate waypoint accuracy, horizontal obstacle avoidance, and landing-zone obstruction detection.

### Waypoint Accuracy Test

Three predefined waypoints will be marked on the floor of Memorial Gym and uploaded as an autonomous mission to the Pixhawk 6C Mini. The drone will autonomously take off, navigate to each waypoint, hold position, proceed to the next waypoint, and land. The complete mission will be repeated three times.

During each waypoint hover, the horizontal position of the drone will be projected or referenced to the floor using a fixed visual reference method while personnel remain outside the active flight area. The measured ground position of the drone's center will then be compared with the pre-marked commanded waypoint.

**Pass Criteria:** The measured horizontal position error shall be no greater than ±0.5 m at each waypoint during all three test missions.

**Fail Criteria:** A waypoint fails if the measured horizontal error exceeds 0.5 m or if the drone is unable to reach and maintain a stable hover at the commanded location.

### Horizontal Obstacle-Avoidance Test

A large cardboard or foam panel will be intentionally placed across the planned path between two autonomous waypoints. A 3 m exclusion boundary will be marked around the obstacle to provide a visible reference for the required minimum clearance. The drone will then execute the waypoint mission using the RPLIDAR C1 and ArduPilot BendyRuler obstacle-avoidance system.

**Pass Criteria:** The drone shall detect the obstacle, generate an alternate path around it, remain at least 3 m from the obstacle, and successfully continue to the commanded waypoint without manual intervention.

**Fail Criteria:** The test fails if the drone enters the 3 m exclusion boundary, fails to respond to the obstacle, makes contact with the obstacle, or cannot safely continue toward the waypoint.

### Landing-Zone Obstruction Test

A controlled landing test will evaluate the system's ability to identify an unexpected object beneath the aircraft. A lightweight foam or cardboard test object will be introduced into the designated landing area while all personnel remain outside the active flight zone. Downward distance measurements from the H-Flow sensor will be compared with the aircraft's expected height above the known floor level.

**Pass Criteria:** If an unexpected surface is detected beneath the drone during descent, the landing shall be aborted before contact occurs and the aircraft shall return to a stable hover or other predefined safe state.

**Fail Criteria:** The test fails if the aircraft continues descending onto the unexpected object, makes contact with the obstruction, or fails to enter the predefined safe state.








## Bill of Materials

| Component | Manufacturer | Part Number | Distributor | Distributor Part Number | Qty | Unit Price | Total Price | URL |
|---|---|---|---|---|---:|---:|---:|---|
| Pixhawk 6C Mini Model-A (revision) w/ PM02 V3 Power Module | Holybro | 11088+15010 | Holybro | 11088+15010 | 1 | $149.98 | $149.98 | https://holybro.com/products/pixhawk-6c-mini |
| H-Flow Optical Flow and Distance Sensor Module | Holybro | 19006 | Holybro | 19006 | 1 | $125.00 | $125.00 | https://holybro.com/products/h-flow |
| RPLIDAR C1 - DTOF LiDAR 360° (12m, IP54) | SLAMTEC | RPLIDAR-C1 | DFRobot | DFR0445 | 1 | $69.00 | $69.00 | https://www.dfrobot.com/product-2803.html |

**Total BOM Cost: $343.98**
### Cost Summary

| Category | Cost |
|---|---:|
| Flight Controller | $149.95 |
| Optical Flow + Distance Sensor | $124.90 |
| 2D Scanning Lidar | $69.00 |
| **Total Subsystem Cost** | **$343.85** |


## Analysis

The internal components subsystem meets its intended function through three COTS components that together provide stable autonomous indoor flight, position hold, and obstacle avoidance [8].

The Pixhawk 6C Mini [1] was selected over the full-size Pixhawk 6C because it provides identical processing and sensor performance at reduced cost and size, with no sacrifice in the connectivity required for this system. It manages all flight operations through ArduPilot firmware, which natively supports the connected navigation and proximity sensors. Its dual IMU configuration provides redundant attitude estimation, and firmware-enforced speed limits and failsafe behaviors directly satisfy FAA 14 CFR Part 107 [4] compliance requirements.

The H-Flow [2] was selected over GPS and UWB alternatives because it requires no external infrastructure and integrates natively with the Pixhawk firmware. It resolves the indoor GPS constraint by supplying optical flow velocity and downward distance data via DroneCAN. Fused with IMU data through ArduPilot's EKF3 state estimator, this enables the ±0.5 m positional accuracy specification to be met during hover at each waypoint.

The RPLIDAR C1 [3] was selected over ultrasonic and single-point ToF sensors because it provides full 360° horizontal coverage in a single lightweight unit, eliminating the blind spots and multi-sensor complexity of alternatives. The sensor is configured in ArduPilot as a 360° proximity sensor, and its measurements are provided to the BendyRuler obstacle-avoidance system [9], [10]. During autonomous waypoint navigation, Horizontal BendyRuler evaluates alternate obstacle-free directions whenever the commanded path is obstructed and generates a locally adjusted path while continuing toward the original mission waypoint. The obstacle-avoidance margin is configured to support the required 3 m horizontal clearance, and the look-ahead distance will be verified through indoor testing. This allows the system to respond to detected obstacles using ArduPilot's native path-planning logic rather than relying on a custom avoidance algorithm.

Combined subsystem mass is approximately 172 g — Pixhawk 6C Mini at 46.8 g, H-Flow at 15.2 g, and RPLIDAR C1 at 110 g — within the 200 g limit. The primary risk is H-Flow performance on reflective venue floors, mitigated by barometer-assisted altitude hold as a fallback [5], [6].


## References

[1] Holybro, "Pixhawk 6C Mini," Holybro, 2024. [Online]. Available: https://holybro.com/collections/flight-controllers/products/pixhawk-6c-mini

[2] Holybro, "H-Flow Optical Flow and Distance Sensor Module," Holybro, 2024. [Online]. Available: https://holybro.com/products/h-flow

[3] SLAMTEC, "RPLIDAR C1 – Fusion DTOF Laser Scanner," DFRobot, 2024. [Online]. Available: https://www.dfrobot.com/product-2803.html

[4] Federal Aviation Administration, "14 CFR Part 107 – Small Unmanned Aircraft Systems," FAA. [Online]. Available: https://www.faa.gov/newsroom/small-unmanned-aircraft-systems-uas-regulations-part-107

[5] R. Merino-Martínez, M. Snellen, and D. G. Simons, "On-field noise measurements and acoustic characterisation of multi-rotor small unmanned aerial systems," Aerospace Science and Technology, vol. 140, 2023, Art. no. 108464.

[6] L. Wang and A. Cavallaro, "Acoustic sensing from a multi-rotor drone," IEEE Sensors Journal, vol. 18, no. 11, pp. 4570–4582, Jun. 2018.

[7] ArduPilot Dev Team, "Radio Control Systems," ArduPilot Copter Documentation. [Online]. Available: https://ardupilot.org/copter/docs/common-rc-systems.html

[8] Anthropic, "Claude," Anthropic, San Francisco, CA, USA. [Online]. Available: https://claude.ai.