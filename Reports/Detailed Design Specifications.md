# Detailed Design

This document delineates the objectives of a comprehensive system design. Upon reviewing this design, the reader should have a clear understanding of:

- How the specific subsystem integrates within the broader solution
- The constraints and specifications relevant to the subsystem
- The rationale behind each crucial design decision
- The procedure for constructing the solution


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

*Note: These technical documentation elements are mandatory only when relevant to the particular subsystem.


## Function of the Subsystem

The power and propulsion subsystem is responsible for storing electrical energy, distributing that energy to propulsion and control components, and generating thrust required for takeoff, hover, maneuvering, and landing.

The subsystem consists of:

- 6S Li-Ion battery (22.2 V, 177.6 Wh)  
- Power module (regulated 5.2 V output)  
- Four ESCs (40 A continuous)  
- Four brushless motors (380 KV, 500 W max)  
- Four propellers (13×4.5 in)  

The battery provides high-current DC power to the ESCs and regulated power to the flight controller through the power module. The ESCs convert DC power into 3-phase AC signals to drive the motors. The motors convert electrical energy into rotational motion, and the propellers convert this motion into thrust.

The subsystem must also support the additional acoustic payload:

- beyerdynamic MM 1 measurement microphone  
- Xvive P1 phantom power supply  

The phantom power system is self-powered and does not significantly draw from the main battery, but it adds mass that directly impacts thrust and flight time.

The subsystem ensures stable and efficient autonomous flight by maintaining sufficient thrust, proper power regulation, and balanced weight distribution.

## Specifications and Constraints

### Specifications

- Battery voltage: 22.2 V nominal  
- Battery capacity: 8000 mAh  
- Battery energy: 177.6 Wh  
- Usable energy: 142–151 Wh  

- Motor KV: 380 KV  
- Motor max power: 500 W  
- Motor quantity: 4  

- ESC rating: 40 A continuous, 60 A peak  
- ESC quantity: 4  

- Propeller size: 13×4.5 in  

- Estimated aircraft mass (updated):  
  - Propulsion mass: 1460.4 g  
  - Non-propulsion mass: 960 g  
  - Total mass: ~2420 g (2.42 kg)  

### Thrust Requirements

- Required hover thrust:  
  - Total: ~2420 g  
  - Per motor: ~605 g  

- Estimated max thrust per motor: ~1500–1600 g  
- Total max thrust: ~6400 g  

- Thrust-to-weight ratio:  
  - ~2.6 : 1  

### Torque

- Motor shaft torque:  
  - ~0.13 N·m at hover  
  - ~0.35 N·m at max thrust  

- Control torque about CG:  
  - ~1.2 N·m (hover)  
  - ~3.2 N·m (max)  

### Power Consumption

- Average flight power:  
  - 425–575 W  

- Audio payload power:  
  - ~0.09 W (negligible to main system)  

### Flight Time

- At 425 W: ~20–21 min  
- At 500 W: ~17–18 min  
- At 575 W: ~14–16 min  

- Realistic mission estimate:  
  - **17–19 minutes**

### Constraints

- **Voltage constraint:** must operate within 6S system limits  
- **Current constraint:** must not exceed ESC and motor ratings  
- **Weight constraint:** added payload increases power demand and reduces endurance  
- **Frame constraint:** must fit within 16 in × 16 in geometry  
- **Power regulation constraint:** Pixhawk requires regulated 5 V input  
- **Safety constraint:** must include voltage/current monitoring for failsafe operation  
- **Socio-economic constraint:** must use cost-effective, commercially available components  

## Overview of Proposed Solution

The subsystem uses a dual-path power architecture:

### High-Power Path
Battery → Power distribution → ESCs → Motors → Propellers  

### Low-Power Path
Battery → Power module → Flight controller  

### Audio Path
Xvive P1 (internal battery) → Microphone  

This architecture ensures:

- efficient power delivery to propulsion components  
- safe voltage regulation for avionics  
- minimal electrical interference between subsystems  
- adequate thrust for increased payload  

The added microphone subsystem increases system mass but does not significantly increase electrical load on the main battery. The propulsion system remains capable of supporting the increased weight without requiring redesign, though endurance is reduced.

The system remains optimized for:

- stable hover  
- efficient lift generation  
- autonomous mapping operations  

The primary limitation introduced by the new payload is reduced flight time rather than insufficient thrust.

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
| Xvive P1 → Microphone | Electrical + Signal | Output/Input | XLR | Phantom power + audio signal |

The subsystem interfaces with:

- **Flight control subsystem** through power and PWM signals  
- **Sensor subsystem** through regulated power and shared control  
- **Acoustic subsystem** through physical mounting and independent power supply  

All interfaces are designed to ensure reliable communication, safe power distribution, and minimal interference between high-power and low-power systems.


## 3D Model of Custom Mechanical Components

Should there be mechanical elements, display diverse views of the necessary 3D models within the document. Ensure the image's readability and appropriate scaling. Offer explanations as required.


## Buildable Schematic 

Integrate a buildable electrical schematic directly into the document. If the diagram is unreadable or improperly scaled, the supervisor will deny approval. Divide the diagram into sections if the text and components seem too small.

The schematic should be relevant to the design and provide ample details necessary for constructing the model. It must be comprehensive so that someone, with no prior knowledge of the design, can easily understand it. Each related component's value and measurement should be clearly mentioned.


## Printed Circuit Board Layout

Include a manufacturable printed circuit board layout.


## Flowchart

For sections including a software component, produce a chart that demonstrates the decision-making process of the microcontroller. It should provide an overview of the device's function without exhaustive detail.


## BOM

| Ref | Component Name                  | Manufacturer | Part Number        | Distributor   | Distributor Part # | Qty | Unit Price ($) | Total ($) | URL |
|-----|--------------------------------|--------------|--------------------|---------------|--------------------|-----|----------------|-----------|-----|
| B1  | Li-Ion Battery (6S 8000mAh)    | iFlight      | Fullsend 6S 8000mAh| RobotShop     | IF-FS-8000-6S      | 1   | 179.81         | 179.81    | https://www.robotshop.com/products/iflight-fullsend-6s-2p-222v-8000mah-xt60h |
| M1  | Brushless Motor               | T-Motor      | F90 1500KV         | RCDrone       | TM-F90-1500KV      | 4   | 52.34          | 209.36    | https://rcdrone.top/products/t-motor-f90-kv1300-kv1500-brus |
| P1  | Propeller (10x4.5 MR)         | APC          | LP10045MR          | Nankin Hobby  | APC10045MR         | 4   | 3.49           | 13.96     | https://nankinhobby.com/products/apc10045mr-10x45-multi-rotor-propeller |
| P2  | Propeller (Spare Set)         | APC          | APC10045MR         | RC Hobby Pros | APC10045MR         | 4   | 3.32           | 13.28     | https://rchobbypros.com/products/multi-rotor-propeller-10-x-4-5 |
| HW1 | Motor Mounting Hardware       | Generic      | Aluminum Mount Kit | Amazon/Local  | N/A                | 4   | 5.00           | 20.00     | N/A |
| HW2 | Battery Mount/Straps          | Generic      | XT60 Strap Mount   | Amazon/Local  | N/A                | 1   | 10.00          | 10.00     | N/A |
| HW3 | Vibration Dampening Mount     | Generic      | Rubber Damper Kit  | Amazon/Local  | N/A                | 1   | 8.00           | 8.00      | N/A |

## Analysis

Deliver a full and relevant analysis of the design demonstrating that it should meet the constraints and accomplish the intended function. This analysis should be comprehensive and well articulated for persuasiveness.

## References

All sources that have contributed to the detailed design and are not considered common knowledge should be duly cited, incorporating multiple references.
