# Detailed Design

This document outlines the objectives of a comprehensive system design. After reviewing this material, the reader should clearly understand:

- How the subsystem integrates within the overall system  
- The constraints and specifications that define its operation  
- The reasoning behind key design decisions  
- The process required to construct and implement the subsystem  


## General Requirements for the Document

The document should include:

- A clear explanation of how the subsystem integrates into the overall solution  
- Detailed specifications and constraints specific to the subsystem  
- A concise overview of the proposed solution  
- Defined interfaces with other subsystems  
- 3D models of any custom mechanical elements*  
- A buildable schematic diagram*  
- A Printed Circuit Board (PCB) layout, if applicable*  
- An operational flowchart*  
- A complete Bill of Materials (BOM)  
- Analysis supporting major design decisions  

\*Note: These elements are only required when relevant to the subsystem.

---

## Function of the Subsystem

The power and propulsion subsystem is responsible for storing electrical energy, distributing that energy to both propulsion and control components, and generating the thrust required for takeoff, hovering, maneuvering, and landing.

This subsystem includes:

- 6S Li-Ion battery (22.2 V, 177.6 Wh)  
- Power module (regulated 5.2 V output)  
- Four ESCs (40 A continuous)  
- Four brushless motors (380 KV, 500 W max)  
- Four propellers (13×4.5 in)  

The battery supplies high-current DC power to the ESCs and regulated power to the flight controller through the power module. The ESCs convert this DC power into three-phase AC signals that drive the motors. The motors then convert electrical energy into rotational motion, which the propellers translate into thrust.

Overall, the subsystem ensures stable and efficient autonomous flight by maintaining sufficient thrust, proper power regulation, and balanced weight distribution.

---

## Specifications and Constraints

### Specifications

- **Battery voltage:** 22.2 V nominal  
- **Battery capacity:** 8000 mAh  
- **Battery energy:** 177.6 Wh  
- **Usable energy:** 142–151 Wh  

- **Motor KV:** 380 KV  
- **Motor max power:** 500 W  
- **Motor quantity:** 4  

- **ESC rating:** 40 A continuous, 60 A peak  
- **ESC quantity:** 4  

- **Propeller size:** 13×4.5 in  

- **Estimated aircraft mass:**  
  - Propulsion mass: 1460.4 g  
  - Non-propulsion mass: 1160 g  
  - Total mass: ~2620 g (2.62 kg)  

### Thrust Requirements

- **Required hover thrust:**  
  - Total: ~2620 g  
  - Per motor: ~655 g  

- **Estimated max thrust per motor:** ~1500–1600 g  
- **Total max thrust:** ~6400 g  

- **Thrust-to-weight ratio:**  
  - ~2.4 : 1  

### Torque

- **Motor shaft torque:**  
  - ~0.14 N·m at hover  
  - ~0.35 N·m at max thrust  

- **Control torque about CG:**  
  - ~1.3 N·m (hover)  
  - ~3.2 N·m (max)  

### Power Consumption

- **Average flight power:**  
  - 450–600 W  

### Flight Time

- **At 450 W:** ~18–20 min  
- **At 525 W:** ~16–17 min  
- **At 600 W:** ~14–15 min  

- **Realistic mission estimate:**  
  - **16–17 minutes**

### Constraints

The subsystem operates under several important constraints that stem from physical limitations, component capabilities, and integration requirements. These constraints guide design decisions and ensure safe, reliable operation.

#### **Electrical Voltage Constraint**  
All components must operate within a 6S voltage architecture (nominal 22.2 V, maximum ~25.2 V fully charged). This ensures compatibility across the system and prevents damage due to overvoltage conditions.

#### **Current Constraint**  
The ESCs are rated at 40 A continuous and 60 A peak. The system must remain within these limits during all operating conditions. Exceeding these ratings can lead to overheating, component failure, or wiring damage. This constraint also influences wire selection and cooling considerations.

#### **Thrust-to-Weight Constraint**  
A thrust-to-weight ratio greater than 2:1 is required for stable flight and control authority. This requirement drove the selection of low-KV motors and larger propellers to maximize efficiency while supporting system mass.

#### **Energy and Endurance Constraint**  
Flight time is limited by battery capacity (177.6 Wh) and average power consumption (450–600 W). Only 80–85% of battery capacity is usable, resulting in 142–151 Wh available for operation. This directly limits flight time to approximately 16–20 minutes.

#### **Mechanical Constraint**  
All components must fit within a 16 in × 16 in frame while maintaining safe propeller spacing. Proper center-of-gravity placement is also critical for stable flight and efficient control.

#### **Power Regulation Constraint**  
The flight controller requires a regulated 5 V supply and cannot be powered directly from the battery. A power module is therefore necessary to provide stable voltage under varying loads.

#### **Safety Constraint**  
Voltage and current monitoring must be included to enable failsafe behavior. Components must also be properly rated to prevent overheating, short circuits, or electrical failure.

#### **Socio-Economic Constraint**  
The design prioritizes cost-effective, commercially available components. This ensures affordability, ease of procurement, and long-term maintainability.

These constraints collectively define the design space and ensure that the subsystem remains safe, efficient, and practical.

---

## Overview of Proposed Solution

The subsystem uses a dual-path power architecture:

### High-Power Path
Battery → Power distribution → ESCs → Motors → Propellers  

### Low-Power Path
Battery → Power module → Flight controller  

This architecture provides:

- Efficient power delivery to propulsion components  
- Safe voltage regulation for avionics  
- Minimal electrical interference  
- Adequate thrust for system mass  

The system is optimized for stable hovering and efficient lift generation.

---

## Interface with Other Subsystems

| Interface | Signal Type | Direction | Method | Data / Function |
|----------|------------|----------|--------|----------------|
| Battery → ESCs | Electrical (DC) | Output | Power distribution | High-current propulsion power |
| Battery → Power Module | Electrical (DC) | Output | Direct wiring | Battery input for regulation |
| Power Module → Flight Controller | Electrical (DC) | Output | 5V regulated | Power supply |
| Power Module → Flight Controller | Analog | Output | ADC sensing | Voltage and current telemetry |
| Flight Controller → ESCs | Digital | Output | PWM | Motor speed control |
| ESCs → Motors | Electrical (3-phase) | Output | Direct wiring | Motor drive |
| Motors → Propellers | Mechanical | Output | Shaft coupling | Thrust generation |
| Power & Propulsion → Frame | Mechanical / Structural | Bidirectional | Mounting hardware | Structural support |

All interfaces are designed to ensure reliable operation, safe power distribution, and minimal interference.

---

## 3D Model of Custom Mechanical Components


The following models represent the major power and propulsion components used in the subsystem. These models are intended to show component placement, spacing, and general fit within the drone frame rather than internal electrical or mechanical construction. Dimensions were based on available component specifications and simplified where appropriate for clean CAD integration.

---

### Motor Model — SunnySky V4008 380KV

![Alt Text](Reports/tImages/Motor.png)

The motor model represents the SunnySky V4008 380KV brushless motor used at each arm end. The model includes the outer cylindrical motor body, top rotor detail, shaft region, and mounting base. For frame integration, the most important dimensions are the rotor diameter, body length, mounting area, and clearance around the propeller hub.

Relevant dimensions and specifications:

- Rotor diameter: **44.3 mm**
- Stator diameter: **40 mm**
- Body length: **20 mm**
- Stator thickness: **8 mm**
- Weight: **105 g**
- KV rating: **380KV**
- Maximum continuous power: **500 W**
- Maximum continuous current: **20 A**
- Recommended ESC: **30–40 A**
- Recommended single takeoff weight: **≤1000 g per motor**  
- Recommended propeller range includes **12–17 inch propellers** :contentReference[oaicite:0]{index=0}

This model is used to verify that the motors can be mounted symmetrically at the arm ends and that each motor has enough clearance for the selected 13-inch propellers.

### ESC Model — HobbyWing XRotor 40A ESC

![Alt Text](Reports/Images/ESC.png)

The ESC model represents one of the four individual ESCs mounted along the frame arms. The model includes the rectangular ESC body and wire bundles entering and leaving the component. Each ESC receives high-current DC power from the power distribution system, receives a PWM control signal from the flight controller, and outputs three-phase motor drive signals to the motor.

Relevant dimensions and specifications:

- Continuous current rating: **40 A**
- Peak current rating: **60 A**
- Input voltage range: **2S–6S**
- BEC: **None**
- Weight: **26 g**
- Dimensions: **68 mm × 25 mm × 8.7 mm**

### Battery Model — iFlight Fullsend 6S 8000mAh Li-Ion Battery

![Alt Text](Reports/Images/Battery.png)

The battery model represents the main onboard energy source. Since the battery is the heaviest component in the power and propulsion subsystem, its placement is critical for maintaining the drone’s center of gravity. The battery should be mounted near the center of the frame and secured with a strap or mechanical retention bracket.

Relevant dimensions and specifications:

- Voltage: **22.2 V nominal**
- Capacity: **8000 mAh**
- Energy: **177.6 Wh**
- Cell configuration: **6S2P**
- Size: **42 mm × 64 mm × 147 mm**
- Weight: **840 g**
- Discharge rating: **17.5C**
- Main connector: **XT60**

The battery model is simplified as a rectangular body with a visible label. This is sufficient for checking fit, mounting space, and center-of-gravity placement.

### Propeller Model — APC 13×4.5MR

![Alt Text](Reports/Images/Propeller.png)

The propeller model represents the selected APC 13×4.5 multirotor propellers. The model includes the hub and blade geometry needed to visualize clearance between adjacent propeller disks. Propeller clearance is one of the most important checks in the frame layout because each propeller sweeps a circular area equal to its diameter.

Relevant dimensions and specifications:

- Diameter: **13 in / 330 mm**
- Pitch: **4.5 in**
- Hub diameter: **0.65 in**
- Hub thickness: **0.36 in**
- Shaft diameter: **1/4 in**
- Weight: **0.85 oz / 24.1 g**

The propeller model is used to verify that the selected frame geometry provides enough spacing between propeller swept areas.

### Full Power and Propulsion Layout

![Alt Text](Reports/Images/PowerAndPropulsion.png)

The full layout shows the relative placement of the battery, ESCs, motors, and propellers. The battery is positioned near the center of the aircraft to reduce center-of-gravity offset. The ESCs are distributed near the arms to improve cooling and shorten motor wiring. The motors and propellers are placed symmetrically to maintain balanced thrust and predictable control response.

This layout supports the intended design because:

- the battery is centered to reduce imbalance
- ESCs are placed near the motors for shorter three-phase wiring
- motors are evenly spaced for symmetric thrust
- propellers are shown with enough spacing to evaluate clearance
- the arrangement reflects the physical wiring path of the subsystem

---

## Buildable Schematic 

A detailed electrical schematic should be included here. The diagram must be readable, properly scaled, and sufficiently detailed so that someone unfamiliar with the system can understand and build it.

---

## Printed Circuit Board Layout

A custom PCB is not required for this subsystem. All components are commercially available and designed to be interconnected using standard wiring. The ESCs and power module already include the necessary internal circuitry.

---

## Flowchart

A flowchart is not required since this subsystem does not include custom software or decision-making logic. Control is handled by the flight controller in a separate subsystem.

---

## BOM

| Ref | Component | Manufacturer | Part Number | Distributor | Distributor Part | Qty | Unit Price ($) | Total ($) | URL |
|-----|----------|-------------|-------------|-------------|------------------|-----|----------------|-----------|-----|
| B1 | Battery | iFlight | Fullsend 6S 8000mAh | iFlight | Pro1914 | 1 | 83.99 | 83.99 | https://shop.iflight.com/Fullsend-6S-8000mAh-Li-Ion-Battery-Pro1914 |
| M1–M4 | Brushless Motor | SunnySky | V4008 380KV | SunnySky USA | V4008 | 4 | 54.99 | 219.96 | https://sunnyskyusa.com/products/sunnysky-v4008-motors |
| ESC1–ESC4 | ESC | HobbyWing | XRotor 40A | HobbyWing Direct | XRotor-40A | 4 | 17.99 | 71.96 | https://www.hobbywingdirect.com/products/xrotor-40a-esc |
| P1–P4 | Propellers | APC | 13×4.5MR-B4 | APC | MR-B4 | 1 set | 16.87 | 16.87 | https://www.apcprop.com/product/13x4-5mr/ |
| CONN1 | Battery Connectors | Amass | XT60 Pair | Amazon | XT60 Set | 2 | 1.90 | 3.80 | — |
| WIRE1 | Power Wiring | BNTECHGO | 12–16 AWG Silicone Wire | Amazon | Wire Kit | 1 | ~15.00 | ~15.00 | — |
| HS1 | Heat Shrink | Eventronic | Heat Shrink Kit | Amazon | HS-Kit | 1 | ~10.00 | ~10.00 | — |
| MISC | Mounting Hardware | Generic | — | Amazon | Assorted Kit | — | ~15.00 | ~15.00 | — |

### **Total Cost: $461.57**

---

## Analysis

The power and propulsion subsystem is designed to provide reliable energy delivery, efficient thrust generation, and stable flight performance for the autonomous drone. The selected design uses a 6S Li-Ion battery, a regulated power module, four individual ESCs, four low-KV brushless motors, and four 13×4.5 propellers. This configuration is appropriate for the mission because it emphasizes stable hover, efficient lift, and dependable operation rather than high-speed or aggressive flight.

The estimated aircraft mass is approximately **2.62 kg**. For a quadcopter, the minimum hover thrust must equal the aircraft weight. This means each motor must provide approximately:

2620 g / 4 motors = 655 g per motor

The selected motor and propeller combination is estimated to provide approximately 1500–1600 g of thrust per motor, giving a total maximum thrust capability of roughly 6400 g. This produces a thrust-to-weight ratio of approximately:

6400 g / 2620 g ≈ 2.4 : 1

A thrust-to-weight ratio above 2:1 is considered sufficient for stable takeoff, hover, maneuvering, and landing. This margin shows that the propulsion system should not need to operate near its maximum output during normal flight. As a result, the drone should have enough control authority to respond to disturbances, correct altitude changes, and maintain stable autonomous flight.

The selected 380KV motors are appropriate because lower-KV motors provide higher torque and are better matched with larger propellers. The 13×4.5 propellers improve hover efficiency by moving a larger volume of air at a lower rotational speed. This is beneficial for an endurance-focused aircraft because it reduces unnecessary power consumption while still producing adequate thrust. The moderate 4.5-inch pitch also supports stable and efficient lift generation rather than prioritizing speed.

The battery provides 177.6 Wh of stored energy. Since only about 80–85% of the battery should be used during normal operation, the usable energy is approximately 142–151 Wh. Based on the expected average flight power of 450–600 W, the estimated flight time is:

At 450 W: ~18–20 minutes
At 525 W: ~16–17 minutes
At 600 W: ~14–15 minutes

A realistic expected flight time is therefore approximately 16–17 minutes under typical operating conditions. This satisfies the intended function of supporting short-duration autonomous measurement or mapping missions while preserving a safety margin for landing and battery protection.

The ESC selection also supports the design constraints. Each ESC is rated for 40 A continuous and 60 A peak, which provides adequate current margin for the selected motors during hover and moderate maneuvering. Using four separate ESCs improves cooling because each unit can be mounted along an arm with exposure to airflow. This also improves reliability and maintainability because one ESC can be replaced individually without replacing a complete 4-in-1 board.

The power regulation requirement is satisfied through the power module. The Pixhawk flight controller cannot be powered directly from the 6S battery, so the power module steps the battery voltage down to a regulated 5 V supply. It also provides voltage and current telemetry, allowing the flight controller to monitor battery status and support low-voltage failsafe behavior. This improves operational safety and helps prevent unexpected power loss during flight.

The subsystem also satisfies mechanical and integration constraints. The battery is the heaviest propulsion component, so it should be mounted near the center of gravity to reduce imbalance and minimize control effort. ESCs are distributed along the arms to improve cooling and maintain balanced mass distribution. Motors and propellers are placed symmetrically, giving balanced pitch and roll authority. This layout supports predictable control response and stable autonomous flight.

The design is also practical from a cost and construction standpoint. The subsystem uses commercially available components with standard connectors, wiring, and mounting methods. This reduces fabrication complexity, simplifies troubleshooting, and makes replacement parts easier to obtain. Since the system does not require a custom PCB, construction can focus on safe wiring, proper mounting, and validation testing.

Overall, the analysis shows that the selected power and propulsion subsystem meets the major design constraints. It operates within the required 6S voltage architecture, remains within ESC and motor current limits, provides sufficient thrust margin, supports safe voltage regulation, and offers a realistic flight time for the mission. Based on these calculations and design choices, the subsystem should accomplish its intended function of enabling stable, efficient, and reliable autonomous flight.

---

## References

[1] iFlight, *Fullsend 6S 8000mAh Li-Ion Battery Specifications*.  
https://shop.iflight.com/Fullsend-6S-8000mAh-Li-Ion-Battery-Pro1914

[2] SunnySky USA, *SunnySky V4008 High Efficiency Brushless Motors*.  
https://sunnyskyusa.com/products/sunnysky-v4008-motors

[3] UAV Model, *SunnySky V4008 High Efficiency Brushless Motors Detailed Specifications*.  
https://www.uavmodel.com/products/sunnysky-v4008-high-efficiency-brushless-motors

[4] HobbyWing Direct, *XRotor 40A ESC COB Specifications*.  
https://www.hobbywingdirect.com/products/xrotor-40a-esc

[5] HobbyWing, *XRotor 40A ESC Technical Overview*.  
https://www.hobbywing.com/en/products/xrotor-40a122

[6] PX4 Documentation, *Holybro PM02 Power Module*.  
https://docs.px4.io/v1.16/zh/power_module/holybro_pm02

[7] Holybro Documentation, *Pixhawk 6C Mini Technical Specification*.  
https://docs.holybro.com/autopilot/pixhawk-6c-mini/technical-specification

[8] APC Propellers, *13x4.5MR-B4 Multirotor Propeller Specifications*.  
https://www.apcprop.com/product/13x4-5mr-b4/

[9] J Perkins Distribution, *APC 13x4.5 Multirotor Propeller Technical Specifications*.  
https://www.jperkins.com/products/APCLP13045MR

[10] Oscar Liang, *How to Build an FPV Drone Tutorial*.  
https://oscarliang.com/how-to-build-fpv-drone/

[11] Engineering ToolBox, *AWG Wire Gauge Sizes and Current Ratings*.  
https://www.engineeringtoolbox.com/wire-gauges-d_419.html

[12] Federal Aviation Administration, *Unmanned Aircraft Systems*.  
https://www.faa.gov/uas
