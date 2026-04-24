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

- Estimated aircraft mass:  
  - Propulsion mass: 1460.4 g  
  - Non-propulsion mass: 1160 g  
  - Total mass: ~2620 g (2.62 kg)  

### Thrust Requirements

- Required hover thrust:  
  - Total: ~2620 g  
  - Per motor: ~655 g  

- Estimated max thrust per motor: ~1500–1600 g  
- Total max thrust: ~6400 g  

- Thrust-to-weight ratio:  
  - ~2.4 : 1  

### Torque

- Motor shaft torque:  
  - ~0.14 N·m at hover  
  - ~0.35 N·m at max thrust  

- Control torque about CG:  
  - ~1.3 N·m (hover)  
  - ~3.2 N·m (max)  

### Power Consumption

- Average flight power:  
  - 450–600 W  

### Flight Time

- At 450 W: ~18–20 min  
- At 525 W: ~16–17 min  
- At 600 W: ~14–15 min  

- Realistic mission estimate:  
  - **16–17 minutes**

### Constraints

The power and propulsion subsystem is governed by several critical constraints that arise from physical limitations, component capabilities, system integration requirements, and practical considerations. These constraints directly influence component selection, system performance, and overall feasibility.



#### **Electrical Voltage Constraint (Physics-Based)**  
All components within the subsystem must operate within a 6S voltage architecture (nominal 22.2 V, maximum ~25.2 V fully charged). This constraint is dictated by the selected battery and must be adhered to by the ESCs, motors, and power module. Operating outside of this range can result in component damage, reduced efficiency, or system failure. This constraint ensures electrical compatibility across all propulsion components and prevents overvoltage conditions.



#### **Current and Thermal Constraint (Physics + Hardware Limitations)**  
The ESCs are rated at 40 A continuous and 60 A peak, while the motors draw current based on load and propeller size. The system must be designed such that current draw during all operating conditions (hover, climb, maneuvering) does not exceed these limits. Exceeding current ratings leads to excessive heat generation, which can cause ESC failure, motor degradation, or wiring damage. This constraint also impacts wire gauge selection and necessitates adequate airflow for cooling, particularly since ESCs are mounted on the arms.



#### **Thrust-to-Weight Constraint (Performance Requirement)**  
The propulsion system must maintain a thrust-to-weight ratio greater than 2:1 to ensure stable flight, controlled maneuvering, and safe takeoff/landing. This constraint arises from fundamental multirotor flight physics. If the ratio is too low, the drone will struggle to hover efficiently or respond to control inputs. This requirement directly influenced the selection of low-KV motors and 13-inch propellers to maximize thrust efficiency while supporting the total system mass.



#### **Energy and Endurance Constraint (Physics-Based)**  
The total available flight time is limited by the battery energy capacity (177.6 Wh) and the system’s average power consumption (~450–600 W). Only 80–85% of battery capacity is usable to prevent over-discharge, resulting in approximately 142–151 Wh of usable energy. This constrains the achievable flight time to roughly 16–20 minutes. This limitation requires careful balancing between weight, thrust, and efficiency to meet mission requirements.



#### **Mechanical and Geometric Constraint (Subsystem Integration)**  
All propulsion components must fit within the physical limits of the 16 in × 16 in frame while maintaining proper spacing between propellers to avoid interference. Propeller diameter (13 inches) and motor placement must ensure no overlap during operation. Additionally, components must be mounted such that the center of gravity remains near the geometric center of the frame. Improper placement would result in unstable flight and increased control effort.



#### **Power Regulation Constraint (Subsystem Dependency)**  
The flight controller cannot be powered directly from the 6S battery and requires a regulated 5 V supply. This necessitates the use of a power module capable of stepping down voltage and providing stable output under varying load conditions. Failure to meet this constraint would result in unreliable flight controller operation or system shutdown.



#### **Safety Constraint (Standards and Operational Safety)**  
The subsystem must include voltage and current monitoring to ensure safe operation. This allows the flight controller to detect low-voltage conditions and initiate failsafe procedures such as controlled landing. Additionally, wiring, connectors, and components must be rated appropriately to prevent overheating, short circuits, or electrical faults. This constraint aligns with general electrical safety standards and protects both the system and operators.



#### **Socio-Economic Constraint (Cost and Availability)**  
The subsystem must be constructed using cost-effective, commercially available components. This ensures that the design remains feasible within budget constraints and that replacement parts are readily accessible. Custom or highly specialized components were avoided in favor of widely available solutions to reduce cost, simplify procurement, and improve maintainability.


These constraints collectively define the design space of the power and propulsion subsystem. Each constraint was considered during component selection and system integration to ensure that the final design is safe, feasible, and capable of achieving the intended performance. 

## Overview of Proposed Solution

The subsystem uses a dual-path power architecture:

### High-Power Path
Battery → Power distribution → ESCs → Motors → Propellers  

### Low-Power Path
Battery → Power module → Flight controller  

This architecture ensures:

- efficient power delivery to propulsion components  
- safe voltage regulation for avionics  
- minimal electrical interference between subsystems  
- adequate thrust for the system mass  

The system remains optimized for:

- stable hover  
- efficient lift generation    

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
| Power & Propulsion → Frame | Mechanical / Structural | Bidirectional | Mounting hardware (bolts, brackets, standoffs) | Structural support, load transfer, vibration transmission, and component mounting |


All interfaces are designed to ensure reliable communication, safe power distribution, and minimal interference between high-power and low-power systems.


## 3D Model of Custom Mechanical Components

Should there be mechanical elements, display diverse views of the necessary 3D models within the document. Ensure the image's readability and appropriate scaling. Offer explanations as required.


## Buildable Schematic 

Integrate a buildable electrical schematic directly into the document. If the diagram is unreadable or improperly scaled, the supervisor will deny approval. Divide the diagram into sections if the text and components seem too small.

The schematic should be relevant to the design and provide ample details necessary for constructing the model. It must be comprehensive so that someone, with no prior knowledge of the design, can easily understand it. Each related component's value and measurement should be clearly mentioned.


## Printed Circuit Board Layout

A custom printed circuit board (PCB) layout is not required for the power and propulsion subsystem because the design relies entirely on commercially available, fully integrated components that are intended to be interconnected using standard wiring rather than custom circuitry.

All major elements of the subsystem—including the battery, ESCs, motors, and power module—are designed as standalone units with built-in control electronics and standardized connectors. The ESCs already contain the necessary switching and control circuitry for driving the motors, while the power module provides regulated voltage and current sensing for the flight controller. As a result, there is no need to design additional circuitry to perform these functions.

Power distribution is achieved through direct wiring (e.g., XT60 connectors, appropriately gauged wires, and soldered joints), which is standard practice in multirotor systems. This approach simplifies construction, reduces design complexity, minimizes potential points of failure, and allows for easier maintenance and component replacement.


## Flowchart

A software flowchart is not required for the power and propulsion subsystem because this subsystem does not contain a dedicated microcontroller or custom software decision-making process. The subsystem is made up of hardware components such as the battery, power module, ESCs, motors, and propellers.

The ESCs and power module contain internal electronics, but they are commercial off-the-shelf components and do not require custom programming by the team. Motor control decisions are handled by the flight controller, which belongs to a different subsystem.


## BOM

| Ref | Component | Manufacturer | Part Number | Distributor | Distributor Part | Qty | Unit Price ($) | Total ($) | URL |
|-----|----------|-------------|-------------|-------------|------------------|-----|----------------|-----------|-----|
| B1 | Battery | iFlight | Fullsend 6S 8000mAh | iFlight | Pro1914 | 1 | 83.99 | 83.99 | https://shop.iflight.com |
| M1–M4 | Brushless Motor | SunnySky | V4008 380KV | SunnySky USA | V4008 | 4 | 54.99 | 219.96 | https://sunnyskyusa.com |
| ESC1–ESC4 | ESC | HobbyWing | XRotor 40A | HobbyWing Direct | XRotor-40A | 4 | 17.99 | 71.96 | https://www.hobbywingdirect.com |
| PM1 | Power Module | Holybro | PM02 | Holybro | PM02D | 1 | 24.99 | 24.99 | https://holybro.com |
| P1–P4 | Propellers | APC | 13×4.5MR-B4 | APC | MR-B4 | 1 set | 16.87 | 16.87 | https://www.apcprop.com |
| CONN1 | Battery Connectors | XT60 | XT60 Pair | Amazon | XT60 Set | 2 | 1.90 | 3.80 | https://www.amazon.com |
| WIRE1 | Power Wiring (12–16 AWG) | Generic | Silicone Wire | Amazon | Wire Kit | 1 | 15.00 | 15.00 | https://www.amazon.com |
| HS1 | Heat Shrink Tubing | Generic | Kit | Amazon | HS-Kit | 1 | 10.00 | 10.00 | https://www.amazon.com |
| MISC | Mounting Hardware (bolts, zip ties, standoffs) | Generic | — | Local/Amazon | — | — | 15.00 | 15.00 | — |

### **Total Cost: $461.57**

## Analysis

The power and propulsion subsystem is designed to supply electrical energy, regulate voltage for avionics, and generate sufficient thrust to support stable, efficient flight for an autonomous mapping drone. The system consists of a 6S Li-Ion battery, a regulated power module, four ESCs, four brushless motors, and four propellers. The design prioritizes endurance, efficiency, and reliability rather than aggressive maneuverability.

The primary energy source is a 6S 8000 mAh Li-Ion battery with a nominal voltage of 22.2 V and total energy of 177.6 Wh. In practical operation, only 80–85% of this energy is usable to prevent over-discharge and maintain battery health, giving approximately 142–151 Wh of usable energy. This energy supports both propulsion and avionics loads.

The estimated total mass of the aircraft for this subsystem analysis is approximately 2.6–2.7 kg. For a quadrotor, hover requires total thrust equal to weight. Therefore, the required thrust per motor is:

2620 g / 4 = 655 g per motor

The selected SunnySky V4008 380KV motors paired with 13×4.5 propellers are capable of producing approximately 1500–1600 g of thrust per motor at maximum output. This results in a total system thrust capability of approximately 6000–6400 g and a thrust-to-weight ratio of:

6400 g / 2620 g ≈ 2.4 : 1

A thrust-to-weight ratio above 2:1 ensures stable takeoff, sufficient control authority, and safe maneuvering. The selected configuration therefore satisfies the thrust requirement with adequate margin while maintaining efficiency.

The motor selection is appropriate for the intended application. The 380KV rating indicates a low-speed, high-torque motor, which is well suited for larger propellers. Larger propellers increase disk area, allowing more air to be moved at lower rotational speeds. This improves hover efficiency and reduces power consumption, which is critical for endurance-focused flight. The 13×4.5 propeller selection further supports this goal, as the moderate pitch reduces power demand while maintaining sufficient thrust.

Power consumption for the propulsion system is estimated based on aircraft mass and typical multirotor performance. The system is expected to operate within an average power range of approximately 450–600 W during normal flight. Using the usable battery energy, the estimated flight time is:

At 450 W: 142–151 Wh / 450 W ≈ 18–20 minutes  
At 525 W: 142–151 Wh / 525 W ≈ 16–17 minutes  
At 600 W: 142–151 Wh / 600 W ≈ 14–15 minutes

A realistic expected flight time for the subsystem is therefore approximately 16–17 minutes under typical operating conditions. This remains acceptable for the intended mapping mission profile.

The ESC selection supports the electrical demands of the motors. Each HobbyWing XRotor 40A ESC is rated for 40 A continuous current and 60 A peak current. The selected motors operate below this threshold during typical flight, ensuring that the ESCs are not thermally or electrically stressed. Using individual ESCs rather than a 4-in-1 ESC improves cooling, distributes heat along the frame arms, and increases system reliability by isolating potential failures.

The power module provides a regulated 5 V supply for the Pixhawk flight controller and monitors battery voltage and current. This is critical because the flight controller cannot be powered directly from the 6S battery. The inclusion of voltage and current sensing enables safe operation by allowing the flight controller to implement low-voltage failsafes and monitor system health.

Torque characteristics further support system feasibility. At hover conditions, each motor produces approximately 0.14 N·m of shaft torque, increasing to approximately 0.35 N·m at higher thrust levels. With a motor arm length of approximately 0.2 m, this results in control moments of:

Hover: ~1.3 N·m  
Max: ~3.2 N·m

These values provide sufficient rotational authority for pitch and roll control, ensuring stable flight and responsiveness to control inputs.

The subsystem satisfies key design constraints. It operates within the 6S voltage range required by all propulsion components, maintains current levels within ESC and motor limits, and uses regulated power for sensitive avionics. The design uses commercially available components, making it practical, cost-effective, and easy to assemble. Safety constraints are addressed through proper power regulation, current capacity margins, and battery monitoring.

From a physical integration perspective, the subsystem supports proper weight distribution and stability. The battery, as the heaviest component, is positioned near the center of gravity to minimize control effort. ESCs are distributed along the arms to improve cooling and balance. The motors are evenly spaced at 90° intervals, ensuring symmetric thrust and predictable control behavior.

Overall, the design demonstrates that the selected power and propulsion subsystem is capable of meeting the required performance criteria. It provides sufficient thrust for the aircraft mass, maintains safe electrical operation, supports efficient flight for the required mission duration, and integrates cleanly with the flight control system. Based on these analyses, the subsystem is expected to successfully accomplish its intended function of enabling stable, efficient, and reliable autonomous flight.

## References

All sources that have contributed to the detailed design and are not considered common knowledge should be duly cited, incorporating multiple references.
