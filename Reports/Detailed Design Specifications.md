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

The power and propulsion subsystem stores electrical energy, distributes electrical power, and converts that power into lift and control forces for the drone. It consists of:

- iFlight Fullsend 6S 8000 mAh Li-Ion battery
- Holybro-compatible power module
- four HobbyWing XRotor 40A ESCs
- four SunnySky V4008 380 KV brushless motors
- four APC 13×4.5 multirotor propellers

The subsystem supplies high-current power to the four ESCs and regulated power to the Pixhawk 6C Mini flight controller. The ESCs convert battery DC power into 3-phase motor drive signals, and the motors convert electrical power into rotational motion that the propellers convert into thrust. The selected battery is rated at 22.2 V nominal, 177.6 Wh, 6S2P, and 840 g. The selected motor is rated at 380 KV, 4–6S, 500 W max continuous power, and 105 g. The selected ESC is rated at 40 A continuous, 60 A peak, 2–6S, and 26 g. The selected propeller bundle is 13×4.5 in.

Because the drone now carries an acoustic payload, the subsystem must support not only the original avionics and frame mass, but also the **beyerdynamic MM 1** microphone and the **Xvive P1** portable phantom power supply. The MM 1 requires 12–48 V phantom power, draws approximately 1.9 mA, weighs 73 g, and is 139 mm long. The Xvive P1 provides 12 V or 48 V phantom power, has an internal rechargeable lithium-ion battery, weighs 145 g, and measures 105 × 44 × 33 mm.


## Specifications and Constraints

### Subsystem Specifications

- Main battery architecture: **6S Li-Ion**
- Main battery nominal voltage: **22.2 V**
- Main battery energy: **177.6 Wh**
- Motor count: **4**
- Motor KV: **380 KV**
- ESC count: **4**
- Propeller size: **13 × 4.5 in**
- Flight controller supply voltage: **5.2 V regulated through power module**
- Target mission profile: **stable autonomous indoor mapping / acoustic measurement**
- Target endurance: **approximately 18–21 minutes under optimized operation**

### Constraints

1. **Voltage compatibility constraint**  
   The propulsion bus must remain compatible with a 6S battery. The battery, ESCs, motors, and power module all need to operate safely across the full 6S range. The battery is 22.2 V nominal, the motors are 4–6S, the ESCs are 2–6S, and the PM02-class power module supports up to 12S while regulating to 5.2 V for the flight controller.

2. **Current and thermal constraint**  
   The motors and ESCs must not be forced into continuous operation beyond their safe limits. Each motor is rated at 20 A continuous for 30 s and 500 W max continuous power, while each ESC is rated at 40 A continuous and 60 A peak.

3. **Weight constraint**  
   Increased aircraft mass directly raises hover thrust and average power demand, reducing flight time. With the microphone and portable phantom supply included, total estimated aircraft mass increases to about **2.42 kg**.

4. **Frame geometry constraint**  
   The system is built around a **16 in × 16 in** custom frame concept. The selected propulsion hardware must fit that geometry and still provide acceptable propeller clearance and moment arm.

5. **Power regulation constraint**  
   The Pixhawk 6C Mini cannot be powered directly from the 6S battery. It requires a regulated low-voltage input, so a dedicated power module is mandatory. Holybro lists the Pixhawk 6C Mini maximum input voltage as 6 V, and the selected power module outputs 5.2 V.

6. **Standards / safety constraint**  
   The subsystem must support safe operation with battery monitoring, current sensing, and low-voltage response. The power module provides voltage and current measurement to the flight controller, allowing battery-aware failsafes.

7. **Socio-economic / practical constraint**  
   The design relies on commercially available COTS components that are easy to source, replace, and document. This reduces fabrication risk and supports student-team assembly and maintenance.

---


## Overview of Proposed Solution

The proposed solution uses a centralized 6S battery architecture with two power branches:

1. **High-power propulsion branch**  
   Battery → power distribution → four ESCs → four motors → four propellers

2. **Low-power avionics branch**  
   Battery → power module → Pixhawk / sensors / telemetry

The acoustic payload is currently powered separately:

3. **Audio payload branch**  
   Xvive P1 internal battery → MM 1 microphone

This means the microphone subsystem adds significant **mass**, but very little additional load to the **main propulsion battery**, because the phantom-power unit is self-powered. The MM 1 itself only consumes about 1.9 mA from the phantom supply.

This architecture satisfies the subsystem requirements because it:
- preserves a simple 6S propulsion bus
- keeps the Pixhawk electrically isolated from high-current motor loads
- provides enough thrust margin for the updated aircraft mass
- maintains acceptable endurance for the mission, though with reduced margin compared to the original payload set

---


## Interface with Other Subsystems

| Interface | Signal / Transfer Type | Direction | Method | Data / Power Exchanged |
|---|---|---:|---|---|
| Battery → ESCs | High-current DC power | Output | Direct power distribution | Main propulsion power |
| Battery → Power Module | DC power | Output | Direct power feed | Battery voltage/current to power module |
| Power Module → Pixhawk 6C Mini | Regulated DC power | Output | 6-pin power connection | 5.2 V regulated power |
| Power Module → Pixhawk 6C Mini | Analog telemetry | Output | Voltage/current sensing | Battery voltage and current |
| Pixhawk 6C Mini → ESCs | Digital control | Output | PWM | Motor speed commands |
| ESCs → Motors | 3-phase electrical drive | Output | Direct wiring | Regulated motor drive current |
| Motors → Propellers | Mechanical | Output | Direct shaft coupling | Rotational speed and torque |
| Xvive P1 → MM 1 | Phantom power + balanced audio path | Output/Input | XLR | 12/48 V phantom power and audio signal |

The Pixhawk 6C Mini supports PWM outputs and a dedicated analog power input. The MM 1 uses a 3-pin XLR connection and requires 12–48 V phantom power, while the Xvive P1 provides XLR input/output and selectable 12 V / 48 V phantom power.

---


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

Provide a comprehensive list of all necessary components along with their prices and the total cost of the subsystem. This information should be presented in a tabular format, complete with the manufacturer, part number, distributor, distributor part number, quantity, price, and purchasing website URL. If the component is included in your schematic diagram, ensure inclusion of the component name on the BOM (i.e R1, C45, U4).

## Analysis

Deliver a full and relevant analysis of the design demonstrating that it should meet the constraints and accomplish the intended function. This analysis should be comprehensive and well articulated for persuasiveness.

## References

All sources that have contributed to the detailed design and are not considered common knowledge should be duly cited, incorporating multiple references.
