# Conceptual Design

This document outlines the objectives of a conceptual design. After reading your conceptual design, the reader should understand:

- The fully formulated problem.
- The fully decomposed conceptual solution.
- Specifications for each of the atomic pieces of the solution.
- Any additional constraints and their origins.
- How the team will accomplish their goals given the available resources.

With these guidelines, each team is expected to create a suitable document to achieve the intended objectives and effectively inform their stakeholders.


## General Requirements for the Document
- Submissions must be composed in Markdown format. Submitting PDFs or Word documents is not permitted.
- All information that is not considered common knowledge among the audience must be properly cited.
- The document should be written in the third person.
- An introduction section should be included.
- The latest fully formulated problem must be clearly articulated using explicit "shall" statements.
- A comparative analysis of potential solutions must be performed
- The document must present a comprehensive, well-specified high-level solution.
- The solution must contain a hardware block diagram.
- The solution must contain an operational flowchart.
- For every atomic subsystem, a detailed functional description, inputs, outputs, and specifications must be provided.
- The document should include an acknowledgment of ethical, professional, and standards considerations, explaining the specific constraints imposed.
- The solution must include a refined estimate of the resources needed, including: costs, allocation of responsibilities for each subsystem, and a Gantt chart.


## Introduction

The introduction is intended to reintroduce the fully formulated problem. 


## Restating the Fully Formulated Problem

The fully formulated problem is the overall objective and scope complete with the set of shall statements. This was part of the project proposal. However, it may be that the scope has changed. So, state the fully formulated problem in the introduction of the conceptual design and planning document. For each of the constraints, explain the origin of the constraint (customer specification, standards, ethical concern, broader implication concern, etc).


## Comparative Analysis of Potential Solutions

### Power and Propulsion Subsystem Design Considerations

**Potential Solutions**

&nbsp; &nbsp; &nbsp; &nbsp; Several configurations were considered for the power and propulsion subsystem, focusing on battery type, motor selection, propeller size, and electronic speed controller (ESC) configuration.

&nbsp; &nbsp; &nbsp; &nbsp; For the battery, both lithium-polymer (LiPo) and lithium-ion (Li-Ion) options were evaluated. LiPo batteries provide higher discharge rates and are commonly used in high-performance drones, while Li-Ion batteries offer higher energy density and improved endurance.

&nbsp; &nbsp; &nbsp; &nbsp; For motor selection, two primary approaches were considered. The first involved larger, low-KV motors such as the Tarot 4112 300KV, which are typically paired with larger propellers (15–16 inches) for maximum efficiency. The second approach involved smaller, lighter motors such as the SunnySky V4008 380KV, which are better suited for mid-sized propellers (12–13 inches) and reduced overall system weight.

&nbsp; &nbsp; &nbsp; &nbsp; Propeller sizes ranging from 12-inch to 15-inch were evaluated. Larger propellers provide higher efficiency and thrust at lower RPMs but require a larger frame and increase system size. Smaller propellers allow for a more compact design but may reduce efficiency and increase power consumption.

&nbsp; &nbsp; &nbsp; &nbsp; For ESC configuration, both 4-in-1 ESCs and individual ESCs were considered. A 4-in-1 ESC offers compact integration and reduced wiring, while individual ESCs provide better thermal distribution, easier replacement, and greater flexibility in larger custom frames.

---

**Design Considerations**

&nbsp; &nbsp; &nbsp; &nbsp; The design process was influenced by several key factors:

- **Endurance Requirements:** The system must support extended flight time for mapping operations, prioritizing efficiency over speed.
- **Weight Constraints:** Reducing total aircraft mass is critical for improving flight time and reducing required thrust.
- **Frame Size Limitations:** The selected 16 in × 16 in frame restricts the maximum propeller size that can be used.
- **Power Efficiency:** The propulsion system must operate efficiently at hover and low-speed cruise conditions.
- **Component Compatibility:** All components must support a 6S power system and operate within safe electrical limits.
- **Thermal and Reliability Considerations:** ESC and motor selection must ensure safe operation under continuous load conditions.
- **Integration Simplicity:** The design should allow for straightforward integration with the flight controller and payload systems.

---

**Factors Influencing Final Selection**

&nbsp; &nbsp; &nbsp; &nbsp; The final configuration was selected based on a balance between efficiency, weight, and compatibility with the frame and mission requirements.

- The **Li-Ion battery** was chosen over LiPo due to its higher energy density, enabling longer flight times.
- The **SunnySky V4008 380KV motors** were selected instead of heavier alternatives to reduce total system weight while maintaining sufficient thrust capability.
- The **13-inch propellers** were selected as a compromise between efficiency and frame constraints, providing improved performance over 12-inch props while remaining compatible with the existing frame.
- **Individual ESCs** were selected instead of a 4-in-1 configuration to improve thermal performance and simplify integration within the larger frame.

---

**Final Design Selection**

&nbsp; &nbsp; &nbsp; &nbsp; The final power and propulsion subsystem configuration consists of:

- 6S 8000 mAh Li-Ion battery  
- SunnySky V4008 380KV brushless motors (×4)  
- HobbyWing XRotor 40A ESCs (×4)  
- APC 13×4.5 multirotor propellers (×4)  
- 16 in × 16 in 3D-printed H-frame  

&nbsp; &nbsp; &nbsp; &nbsp; This configuration provides a balanced solution that meets the endurance requirements of the project while maintaining compatibility with the mechanical design. The selected components reduce unnecessary weight, improve efficiency during hover, and allow the system to achieve an estimated flight time near 20 minutes under optimized operating conditions.

---

**Justification Summary**

&nbsp; &nbsp; &nbsp; &nbsp; The chosen design represents a compromise between competing design constraints. While larger propellers and motors could improve efficiency, they would require a larger frame and increase system complexity. Conversely, smaller components would reduce size but negatively impact endurance. The selected configuration achieves an effective balance by maximizing efficiency within the constraints of the existing frame and mission requirements.

&nbsp; &nbsp; &nbsp; &nbsp; Overall, the final design supports stable, efficient, and reliable autonomous mapping operation while remaining practical for implementation and integration.



## High-Level Solution

This section presents a comprehensive, high-level solution aimed at efficiently fulfilling all specified requirements and constraints. The solution is designed to maximize stakeholder goal attainment, adhere to established constraints, minimize risks, and optimize resource utilization. Please elaborate on how your design accomplishes these objectives.


### Hardware Block Diagram

![Alt text](https://github.com/TnTech-ECE/S26_Team5_Acoustic-Measurement-Drone/blob/Jackson's-Branch/Reports/Images/Hardware_Block_Diagram_Ext_Comp.png)

### Operational Flow Chart

Similar to a block diagram, the flow chart aims to specify the system, but from the user's point of view rather than illustrating the arrangement of each subsystem. It outlines the steps a user needs to perform to use the device and the screens/interfaces they will encounter. A diagram should be drawn to represent this process. Each step should be represented in the diagram to visually depict the sequence of actions and corresponding screens/interfaces the user will encounter while using the device.


## Atomic Subsystem Specifications

### Power and Propulsion Subsystem

**Functional Description**

&nbsp; &nbsp; &nbsp; &nbsp; The power and propulsion subsystem is responsible for storing electrical energy, distributing that energy to the propulsion hardware, and generating the thrust required for takeoff, hover, maneuvering, and landing. The subsystem consists of a 6S lithium-ion battery, four electronic speed controllers, four brushless motors, and four multirotor propellers.

&nbsp; &nbsp; &nbsp; &nbsp; The battery serves as the primary onboard energy source. Electrical power from the battery is delivered to the ESCs, which regulate power to each motor. The motors convert electrical energy into rotational motion, and the propellers convert that rotational motion into thrust. Together, these components provide the lift and control authority needed for stable autonomous mapping flight.

&nbsp; &nbsp; &nbsp; &nbsp; The updated propulsion configuration uses the iFlight Fullsend 6S 8000 mAh Li-Ion battery, HobbyWing XRotor 40A ESCs, SunnySky V4008 380KV motors, and APC 13x4.5 multirotor propellers. This updated selection reduces propulsion weight compared to the previous motor choice while maintaining sufficient power capability for the expected aircraft mass.

**Design Justification**

&nbsp; &nbsp; &nbsp; &nbsp; The selected components prioritize endurance, efficiency, and compatibility with the custom 16 in × 16 in frame. The 6S 8000 mAh battery was selected because it provides high energy density while keeping system mass lower than a comparable high-capacity LiPo pack. The SunnySky V4008 380KV motors were selected in place of heavier alternatives because they better match the 13-inch propeller size and reduce total aircraft weight, improving flight time potential.

&nbsp; &nbsp; &nbsp; &nbsp; The APC 13x4.5 propellers provide a practical compromise between efficiency and frame size. They fit the existing frame geometry while still offering better hover efficiency than smaller propellers. The HobbyWing XRotor 40A ESCs provide sufficient current capacity and 6S compatibility for the selected motor and battery combination.

&nbsp; &nbsp; &nbsp; &nbsp; Overall, the updated configuration is better aligned with the mission objective of stable, long-duration autonomous mapping flight. The design emphasizes efficient hover and moderate cruise performance rather than maximum speed or aggressive maneuverability.

**Subsystem Objectives**

The power and propulsion subsystem shall:
- store and distribute electrical energy for the aircraft
- provide sufficient thrust for takeoff, hover, maneuvering, and landing
- support stable and efficient autonomous mapping flight
- reduce total propulsion weight while maintaining adequate thrust margin
- operate from a 6S battery architecture
- provide an estimated flight time near 20 minutes under endurance-focused flight conditions

**Detailed Operation**

&nbsp; &nbsp; &nbsp; &nbsp; During operation, the battery supplies DC power to the propulsion system. Each ESC receives battery power and a control signal from the flight controller, then regulates the three-phase output delivered to its corresponding motor. Each motor rotates its propeller at the speed commanded by the flight controller. By varying motor speed across the four motors, the aircraft produces the thrust and control moments required for stable flight.

&nbsp; &nbsp; &nbsp; &nbsp; The lighter V4008 motors reduce total propulsion mass and improve the expected endurance of the aircraft. During hover and mapping flight, the propulsion subsystem is expected to operate well below maximum rated power. This allows the system to maintain safe electrical and thermal margins while supporting extended flight duration.

**Functional Flowchart**

![Alt text](https://github.com/TnTech-ECE/S26_Team5_Acoustic-Measurement-Drone/blob/Jackson's-Branch/Reports/Images/Power%20Distribution%20Flow%20Chart.png)  

**Performance Specifications**

The power and propulsion subsystem shall satisfy the following:
- Battery voltage: 22.2 V nominal
- Battery capacity: 8000 mAh
- Battery energy: 177.6 Wh
- Battery weight: 840 g
- Motor quantity: 4
- Motor KV: 380KV
- Motor maximum continuous power: 500 W each
- Motor weight: 105 g each
- ESC quantity: 4
- ESC current rating: 40 A continuous, 60 A peak
- ESC voltage compatibility: 2S–6S
- ESC weight: 26 g each
- Propeller size: 13 × 4.5 in
- Propeller weight: 24.1 g each
- Estimated propulsion subsystem mass: 1460.4 g
- Estimated total aircraft mass: 2182.4 g before additional wiring and mounting hardware
- Estimated realistic flight mass: approximately 2.2–2.3 kg
- Estimated usable battery energy: 142–151 Wh assuming 80–85% usable capacity
- Estimated average flight power for endurance operation: approximately 350–500 W
- Estimated flight time: approximately 17–26 minutes
- Realistic mission planning estimate: approximately 18–22 minutes

**Weight Breakdown**

Known non-propulsion mass:
- Flight controller: 46.8 g
- HFlow sensor: 15.2 g
- RPLIDAR C1: 110 g
- DSP/Teensy subsystem: 50 g
- 3D-printed carbon-fiber-reinforced nylon H-frame: 500 g

Non-propulsion subtotal:
- 722.0 g

Propulsion subsystem mass:
- Battery: 840 g
- Motors: 4 × 105 g = 420 g
- ESCs: 4 × 26 g = 104 g
- Propellers: 4 × 24.1 g = 96.4 g

Propulsion subtotal:
- 1460.4 g

Estimated total mass:
- 722.0 g + 1460.4 g = 2182.4 g

**Flight Time Calculation**

Battery energy:
- 22.2 V × 8.0 Ah = 177.6 Wh

Usable battery energy:
- 80% usable: 177.6 × 0.80 = 142.1 Wh
- 85% usable: 177.6 × 0.85 = 151.0 Wh

Estimated flight time:
- At 350 W average power: 142.1/350 to 151.0/350 = 24.4 to 25.9 minutes
- At 425 W average power: 142.1/425 to 151.0/425 = 20.1 to 21.3 minutes
- At 500 W average power: 142.1/500 to 151.0/500 = 17.1 to 18.1 minutes

This indicates that a flight time near 20 minutes is achievable if the aircraft maintains an average power draw of approximately 425–450 W during mapping flight.

### Detailed Shall Statements

**Functional Requirements**
- The subsystem shall provide electrical power for all propulsion components.
- The subsystem shall include one main battery, four ESCs, four motors, and four propellers.
- The subsystem shall generate sufficient thrust for takeoff, hover, maneuvering, and landing.
- The subsystem shall support endurance-focused autonomous mapping flight.

**Weight and Efficiency Requirements**
- The subsystem shall minimize propulsion mass while maintaining adequate thrust margin.
- The subsystem shall use a motor and propeller combination matched to a 16 in × 16 in frame.
- The subsystem shall target a total aircraft mass near 2.2 kg before final integration hardware.
- The subsystem shall support a realistic flight time of at least 18 minutes under normal mission conditions.
- The subsystem shall support a target flight time near 20 minutes under endurance-focused operation.

**Electrical Requirements**
- The subsystem shall operate from a 6S battery architecture.
- The subsystem shall use ESCs rated for at least 40 A continuous current.
- The subsystem shall use motors compatible with 6S operation.
- The subsystem shall operate within the rated voltage, current, and power limits of the selected components.

**Validation Requirements**
- The subsystem shall be verified through total weight measurement after integration.
- The subsystem shall be validated through hover and endurance flight testing.
- The subsystem shall demonstrate stable operation without exceeding motor or ESC thermal limits.
- The subsystem shall demonstrate sufficient endurance for mapping operations.

**Major Data Elements**

Calculated values:
- total propulsion mass
- total estimated aircraft mass
- usable battery energy
- expected average power draw
- estimated flight time range
## Ethical, Professional, and Standards Considerations

In the project proposal, each team must evaluate the broader impacts of the project on culture, society, the environment, public health, public safety, and the economy. Additionally, teams must consider relevant standards organizations that will inform the design process. A comprehensive discussion should be included on how these considerations have influenced the design. This includes detailing constraints, specifications, and practices implemented as a result, and how these address the identified considerations.


## Resources

You have already estimated the resources needed to complete the solution. Now, let's refine those estimates.

### Budget

| **Item**                         | **Description**                                         | **Estimated Cost**      |
|----------------------------------|---------------------------------------------------------|--------------------------|
| **Battery (6S 8000mAh Li-Ion)** | High energy-density battery for long endurance flight   | $99 – $115               |
| **Motors (4x SunnySky V4008)**  | Brushless motors optimized for 13-inch propellers       | $219.96 (4 × $54.99)     |
| **Electronic Speed Controllers (ESCs)** | HobbyWing XRotor 40A ESCs (4x)               | $71.96 (4 × $17.99)      |
| **Propellers (13x4.5 + Spares)**| APC multirotor propellers (4–6 total)                   | $23 – $35                |
| **Power Distribution / Wiring** | Power distribution, connectors, and integration hardware| $25 – $50                |
| **Mounting Hardware**           | Motor mounts, fasteners, and structural integration     | $15 – $40                |

Estimated total power and propulsion subsystem cost: ≈ $454 – $532

### Division of Labor

First, conduct a thorough analysis of the skills currently available within the team, and then compare these skills to the specific requirements of each subsystem. Based on this analysis, appoint a team member to take the specifications for each subsystem and generate a corresponding solution (i.e. detailed design). If there are more team members than subsystems, consider further subdividing the solutions into smaller tasks or components, thereby allowing each team member the opportunity to design a subsystem.

### Timeline

Revise the detailed timeline (Gantt chart) you created in the project proposal. Ensure that the timeline is optimized for detailed design. Address critical unknowns early and determine if a prototype needs to be constructed before the final build to validate a subsystem. Additionally, if subsystem $A$ imposes constraints on subsystem $B$, generally, subsystem $A$ should be designed first.


## References

All sources utilized in the conceptual design that are not considered common knowledge must be properly cited. Multiple references should be included.


## Statement of Contributions

Each team member is required to make a meaningful contribution to the project proposal. In this section, each team member is required to document their individual contributions to the report. One team member may not record another member's contributions on their behalf. By submitting, the team certifies that each member's statement of contributions is accurate.

