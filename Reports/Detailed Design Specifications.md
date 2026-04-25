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

The battery supplies high-current DC power to the ESCs and regulated power to the flight controller via the power module. The ESCs convert this DC power into three-phase AC signals that drive the motors. The motors then convert electrical energy into rotational motion, which the propellers translate into thrust.

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
| Power & Propulsion → Frame | Mechanical / Structural | Bidirectional | Mounting hardware | Structural support and load transfer |

All interfaces are designed to ensure reliable operation, safe power distribution, and minimal interference.

---

## 3D Model of Custom Mechanical Components

If custom mechanical components are used, this section should include multiple views of the 3D models with clear scaling and readability. Descriptions should be provided where necessary.

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
| B1 | Battery | iFlight | Fullsend 6S 8000mAh | iFlight | Pro1914 | 1 | 83.99 | 83.99 | https://shop.iflight.com |
| M1–M4 | Brushless Motor | SunnySky | V4008 380KV | SunnySky USA | V4008 | 4 | 54.99 | 219.96 | https://sunnyskyusa.com |
| ESC1–ESC4 | ESC | HobbyWing | XRotor 40A | HobbyWing Direct | XRotor-40A | 4 | 17.99 | 71.96 | https://www.hobbywingdirect.com |
| PM1 | Power Module | Holybro | PM02 | Holybro | PM02D | 1 | 24.99 | 24.99 | https://holybro.com |
| P1–P4 | Propellers | APC | 13×4.5MR-B4 | APC | MR-B4 | 1 set | 16.87 | 16.87 | https://www.apcprop.com |
| CONN1 | Battery Connectors | XT60 | XT60 Pair | Amazon | XT60 Set | 2 | 1.90 | 3.80 | https://www.amazon.com |
| WIRE1 | Power Wiring | Generic | Silicone Wire | Amazon | Wire Kit | 1 | 15.00 | 15.00 | https://www.amazon.com |
| HS1 | Heat Shrink | Generic | Kit | Amazon | HS-Kit | 1 | 10.00 | 10.00 | https://www.amazon.com |
| MISC | Mounting Hardware | Generic | — | Local/Amazon | — | — | 15.00 | 15.00 | — |

### **Total Cost: $461.57**

---

## Analysis

The subsystem is designed to provide reliable energy delivery, efficient propulsion, and stable flight performance. The selected components support a thrust-to-weight ratio of approximately 2.4:1, ensuring sufficient control authority and safe operation.

The battery provides adequate energy for a realistic flight time of 16–17 minutes. The motors and propellers are optimized for efficiency, while the ESCs operate within safe current limits.

The system also ensures proper voltage regulation, thermal management, and structural balance. All components are commercially available, making the design practical and maintainable.

Overall, the analysis demonstrates that the subsystem meets all functional and performance requirements for autonomous flight.

---

## References

## References

[1] iFlight. *Fullsend 6S 8000mAh Li-Ion Battery Specifications.*  
https://shop.iflight.com/Fullsend-6S-8000mAh-Li-Ion-Battery-Pro1914

[2] SunnySky. *V4008 Brushless Motor Datasheet and Specifications.*  
https://sunnyskyusa.com/products/sunnysky-v4008-brushless-motor

[3] HobbyWing. *XRotor 40A ESC Specifications and User Manual.*  
https://www.hobbywingdirect.com/products/xrotor-40a-esc

[4] Holybro. *PM02 Power Module Documentation.*  
https://holybro.com/products/pm02-power-module

[5] PX4 Documentation. *Holybro Power Module (PM02) Technical Details.*  
https://docs.px4.io/main/en/power_module/holybro_pm02.html

[6] APC Propellers. *13×4.5MR Multirotor Propeller Specifications.*  
https://www.apcprop.com/product/13x4-5mr/

[7] Pixhawk. *Pixhawk 6C Flight Controller Specifications.*  
https://holybro.com/products/pixhawk-6c

[8] Betaflight / Multirotor Theory. *Basic Principles of ESC and Motor Control.*  
https://betaflight.com/docs/wiki/guides/current/ESC

[9] Oscar Liang. *The Ultimate Guide to Building a Drone.*  
https://oscarliang.com/build-drone/

[10] DroneBot Workshop. *How Brushless Motors and ESCs Work.*  
https://dronebotworkshop.com/brushless-motors/

[11] Engineering Toolbox. *Electrical Wire Gauge and Current Ratings.*  
https://www.engineeringtoolbox.com/wire-gauges-d_419.html

[12] FAA. *Small Unmanned Aircraft Systems Safety Guidelines.*  
https://www.faa.gov/uas
