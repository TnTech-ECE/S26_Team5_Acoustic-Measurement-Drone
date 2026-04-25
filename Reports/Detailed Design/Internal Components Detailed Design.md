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

The RPLIDAR C1 [3] connects via TTL UART to the Pixhawk's TELEM2 port and performs continuous 360° horizontal scanning at 5KHz. The flight controller monitors incoming distance data and maneuvers the drone to maintain a minimum 3 m clearance from any detected obstacle at all times.

This configuration meets all positional accuracy, obstacle avoidance, and weight requirements while remaining within budget and minimizing integration complexity.


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


## Bill of Materials

| Component | Manufacturer | Part Number | Distributor | Distributor Part Number | Qty | Unit Price | Total Price | URL |
|---|---|---|---|---|---|---|---|---|
| Pixhawk 6C Mini (w/ PM02 V3) | Holybro | 0906 | Holybro | 0906 | 1 | $149.98 | $149.95 | https://holybro.com/products/pixhawk-6c-mini |
| Holybro H-Flow | Holybro | H-Flow | Holybro | H-Flow | 1 | $124.90 | $125.00 | https://holybro.com/products/h-flow |
| RPLIDAR C1 - DTOF LiDAR 360° (12m, IP54) | SLAMTEC | RPLIDAR-C1 | DFRobot | DFR0445 | 1 | $69.00 | $69.00 | https://www.dfrobot.com/product-2803.html |

### Cost Summary

| Category | Cost |
|---|---:|
| Flight Controller | $149.95 |
| Optical Flow + Distance Sensor | $124.90 |
| 2D Scanning Lidar | $69.00 |
| **Total Subsystem Cost** | **$343.85** |


## Analysis


The internal components subsystem meets its intended function through three COTS components that together provide stable autonomous indoor flight, position hold, and obstacle avoidance [8].

The Pixhawk 6C Mini [1] was selected over the full-size Pixhawk 6C because it provides identical processing and sensor performance at reduced cost and size, with no sacrifice in the connectivity required for this system. It manages all flight operations through ArduPilot/PX4 firmware, which natively supports all connected sensors. Its dual IMU configuration provides redundant attitude estimation, and firmware-enforced speed limits and failsafe behaviors directly satisfy FAA 14 CFR Part 107 [4] compliance requirements.

The H-Flow [2] was selected over GPS and UWB alternatives because it requires no external infrastructure and integrates natively with the Pixhawk firmware. It resolves the indoor GPS constraint by supplying optical flow velocity and downward distance data via DroneCAN. Fused with IMU data through ArduPilot's EKF3 state estimator, this enables the ±0.5m positional accuracy specification to be met during hover at each waypoint.

The RPLIDAR C1 [3] was selected over ultrasonic and single-point ToF sensors because it provides full 360° horizontal coverage in a single lightweight unit, eliminating the blind spots and multi-sensor complexity of alternatives. With a 12m detection range at 5KHz, the flight controller continuously monitors incoming scan data and actively maneuvers the drone to maintain 3m clearance in all horizontal directions. If an obstacle is detected within that threshold, the drone halts and reroutes before the clearance constraint is violated, regardless of operating speed.

Combined subsystem mass is approximately 172g — Pixhawk 6C Mini at 46.8g, H-Flow at 15.2g, and RPLIDAR C1 at 110g — within the 200g limit. The primary risk is H-Flow performance on reflective venue floors, mitigated by barometer-assisted altitude hold as a fallback [5], [6].


## References

[1] Holybro, "Pixhawk 6C Mini," Holybro, 2024. [Online]. Available: https://holybro.com/collections/flight-controllers/products/pixhawk-6c-mini

[2] Holybro, "H-Flow Optical Flow and Distance Sensor Module," Holybro, 2024. [Online]. Available: https://holybro.com/products/h-flow

[3] SLAMTEC, "RPLIDAR C1 – Fusion DTOF Laser Scanner," DFRobot, 2024. [Online]. Available: https://www.dfrobot.com/product-2803.html

[4] Federal Aviation Administration, "14 CFR Part 107 – Small Unmanned Aircraft Systems," FAA. [Online]. Available: https://www.faa.gov/newsroom/small-unmanned-aircraft-systems-uas-regulations-part-107

[5] R. Merino-Martínez, M. Snellen, and D. G. Simons, "On-field noise measurements and acoustic characterisation of multi-rotor small unmanned aerial systems," Aerospace Science and Technology, vol. 140, 2023, Art. no. 108464.

[6] L. Wang and A. Cavallaro, "Acoustic sensing from a multi-rotor drone," IEEE Sensors Journal, vol. 18, no. 11, pp. 4570–4582, Jun. 2018.

[7] ArduPilot Dev Team, "Radio Control Systems," ArduPilot Copter Documentation. [Online]. Available: https://ardupilot.org/copter/docs/common-rc-systems.html

[8] Anthropic, "Claude," Anthropic, San Francisco, CA, USA. [Online]. Available: https://claude.ai.