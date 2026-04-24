# Detailed Design

---

This document delineates the objectives of a comprehensive system design. Upon reviewing this design, the reader should have a clear understanding of:

- How the specific subsystem integrates within the broader solution  
- The constraints and specifications relevant to the subsystem  
- The rationale behind each crucial design decision  
- The procedure for constructing the solution  

---

## General Requirements for the Document

The document should include:

- Explanation of the subsystem’s integration within the overall solution  
- Detailed specifications and constraints specific to the subsystem  
- Synopsis of the suggested solution  
- Interfaces to other subsystems  
- 3D models of customized mechanical elements*  
- A buildable diagram*  
- A Printed Circuit Board (PCB) design layout*  
- An operational flowchart*  
- A comprehensive Bill of Materials (BOM)  
- Analysis of crucial design decisions  

*Note: These technical documentation elements are mandatory only when relevant to the particular subsystem.*

---

## Function of the Subsystem

The power and propulsion subsystem is responsible for storing electrical energy, distributing that energy throughout the system, and generating the thrust required for all phases of flight, including takeoff, hover, maneuvering, and landing.

The subsystem consists of:

- 6S Li-Ion battery (22.2 V, 177.6 Wh)  
- Power module (regulated 5.2 V output with telemetry)  
- Four ESCs (40 A continuous, 60 A peak, no BEC)  
- Four brushless motors (380 KV, 500 W max)  
- Four propellers (13×4.5 in)  

The battery supplies high-current DC power to the ESCs and a regulated branch through the power module to the flight controller. The ESCs convert DC power into 3-phase AC signals to drive the motors. The motors convert electrical energy into rotational motion, and the propellers convert that motion into thrust.

The subsystem ensures stable and efficient autonomous flight by maintaining sufficient thrust, proper power regulation, and balanced weight distribution.

---

## Specifications and Constraints

### Specifications

- Battery voltage: 22.2 V nominal (25.2 V fully charged)  
- Battery capacity: 8000 mAh  
- Battery energy: 177.6 Wh  
- Usable energy: 142–151 Wh  
- Battery discharge rating: 17.5C (~140 A max continuous)  
- Battery size: ~42 × 64 × 147 mm  

- Motor KV: 380 KV  
- Motor max power: 500 W  
- Motor max continuous current: ~20 A  
- Motor quantity: 4  

- ESC rating: 40 A continuous, 60 A peak  
- ESC BEC: None (external regulation required)  
- ESC quantity: 4  

- Propeller size: 13×4.5 in  

- Estimated aircraft mass:  
  - Propulsion mass: 1460.4 g  
  - Non-propulsion mass: 1160 g  
  - Total mass: ~2620 g (2.62 kg)  

---

### Thrust Requirements

- Required hover thrust:  
  - Total: ~2620 g  
  - Per motor: ~655 g  

- Estimated max thrust per motor: ~1500–1600 g  
- Total max thrust: ~6400 g  

- Thrust-to-weight ratio:  
  - ~2.4 : 1  

---

### Torque

- Motor shaft torque:  
  - ~0.14 N·m at hover  
  - ~0.35 N·m at max thrust  

- Control torque about CG:  
  - ~1.3 N·m (hover)  
  - ~3.2 N·m (max)  

---

### Power Consumption

- Average flight power:  
  - 450–600 W  

---

### Flight Time

- At 450 W: ~18–20 min  
- At 525 W: ~16–17 min  
- At 600 W: ~14–15 min  

- Realistic mission estimate:  
  - **16–17 minutes**

---

### Constraints

The subsystem is governed by constraints derived from physics, component limitations, system integration requirements, and practical implementation considerations.

#### Electrical Voltage Constraint  
All components must operate within a 6S voltage range (nominal 22.2 V, max 25.2 V). This ensures compatibility between the battery, ESCs, motors, and power module. Exceeding this range risks component failure.

#### Current and Thermal Constraint  
The ESCs (40 A continuous) and motors (~20 A continuous) impose strict limits on allowable current draw. The battery supports up to ~140 A, but the system must remain within safe operating limits to avoid overheating. This constraint directly influences propeller selection, throttle limits, and wiring gauge.

#### Thrust-to-Weight Constraint  
A minimum thrust-to-weight ratio greater than 2:1 is required for stable flight. The selected propulsion system satisfies this requirement, ensuring sufficient control authority and safe maneuvering.

#### Energy and Endurance Constraint  
Only 80–85% of battery capacity is usable, limiting total flight time. The system must balance power consumption and efficiency to meet mission requirements within this constraint.

#### Mechanical and Geometric Constraint  
All components must fit within a 16 in × 16 in frame while maintaining proper propeller clearance. Additionally, mass must be distributed to maintain the center of gravity near the geometric center of the frame.

#### Power Regulation Constraint  
The flight controller requires a regulated 5 V input. Since the ESCs do not include a BEC, a dedicated power module is required to safely supply regulated voltage and telemetry.

#### Wiring and Distribution Constraint  
High-current paths require appropriate conductor sizing (12 AWG main, 16 AWG branches). Improper wiring can lead to voltage drop, overheating, or failure. Power modules should not be used as the sole high-current distribution path if their wiring is undersized.

#### Safety Constraint  
The system must monitor voltage and current to prevent over-discharge and electrical faults. Proper connectors, insulation, and thermal management are required to ensure safe operation.

#### Socio-Economic Constraint  
All components must be commercially available and cost-effective. This ensures feasibility, ease of replacement, and accessibility of parts.

---

## Overview of Proposed Solution

The subsystem uses a dual-path power architecture:

### High-Power Path  
Battery → Power distribution → ESCs → Motors → Propellers  

### Low-Power Path  
Battery → Power module → Flight controller  

This architecture ensures:

- efficient power delivery to propulsion components  
- safe and stable voltage regulation for avionics  
- minimal interference between power and control systems  
- sufficient thrust generation for stable flight  

The system is optimized for:

- stable hover  
- efficient lift generation  
- endurance-focused operation  

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

---

## 3D Model of Custom Mechanical Components

(Section intentionally left for future inclusion of CAD models and mounting designs.)

---

## Buildable Schematic 

The electrical schematic is designed to clearly represent power flow, signal routing, and component relationships in a manner that is directly buildable. The schematic uses standardized symbols, clearly labeled components, and defined wiring paths to ensure readability and ease of construction.

---

## Printed Circuit Board Layout

A custom PCB is not required because all components are commercially integrated and designed for direct wiring. Power distribution is achieved using connectors and appropriately sized wiring, reducing complexity and improving maintainability.

---

## Flowchart

A software flowchart is not required because this subsystem contains no programmable logic. Control decisions are handled externally by the flight controller.

---

## BOM

*(Table unchanged)*

---

## Analysis

The power and propulsion subsystem meets all performance requirements for the intended application. The selected components provide sufficient thrust, operate within safe electrical limits, and support the required flight duration.

The system maintains a thrust-to-weight ratio of approximately 2.4:1, ensuring stable and responsive flight. Power consumption remains within the limits of the battery capacity, yielding a realistic flight time of 16–17 minutes.

Component selection reflects a balance between efficiency, cost, and reliability. The use of individual ESCs improves thermal performance and fault tolerance, while the power module ensures safe operation of the flight controller.

Overall, the subsystem is capable of delivering stable, efficient, and reliable performance for autonomous operation.

---

## References

All sources used for component specifications and design decisions should be cited here.
