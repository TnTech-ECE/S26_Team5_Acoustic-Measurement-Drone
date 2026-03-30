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

# Comparative Analysis of Potential Solutions

## Frame Configuration

### Options Considered

**1. X-Frame**
- Most common quadcopter layout with symmetrical geometry
- Pros: balanced and symmetrical layout, good maneuverability and control response, widely used with abundant replacement parts and design knowledge
- Cons: less central space for payload mounting, harder to isolate sensors from propulsion-related vibration, optimized for agility over instrument carrying
- Estimated Price: $30–$150 (carbon-fiber kits)

**2. H-Frame**
- Wider center body with extended arms creating larger middle section for electronics and payload
- Pros: larger center area for electronics and sensors, easier to mount payloads and custom hardware, better suited for research and experimental platforms, convenient for battery placement and vibration isolation
- Cons: slightly bulkier than X-frame, creates somewhat more drag, less agile than racing-focused frames
- Estimated Price: $30–$80 (hobby-grade carbon-fiber)

**3. Deadcat Frame**
- Front arms moved outward to keep propellers out of forward camera view
- Pros: keeps front propellers out of camera view, decent center space for components, useful for forward-facing imaging
- Cons: less symmetrical layout, not ideal for balanced sensor placement, more beneficial for video than acoustic work
- Estimated Price: $40–$90 (carbon-fiber kits)

### Selected Configuration
H-Frame

### Justification
The H-frame best matches the project's functional priorities for an acoustic measurement drone. Unlike racing-oriented layouts, this drone requires adequate room for microphone placement, acoustic electronics, onboard processor (Raspberry Pi), battery and wiring, and vibration-isolation mounts. The wider central body reduces packaging constraints and improves sensor placement flexibility. The slight increase in bulk is an acceptable tradeoff for stability, mounting space, and structural practicality.

---

## Frame Material

### Options Considered

**1. Carbon Fiber**
- Widely used in drone structures for high stiffness and low weight
- Pros: very high strength-to-weight ratio, stiff and lightweight, common in drone frame construction, maintains structural rigidity in flight
- Cons: more expensive than plastic, harder to machine and repair, damaged parts need replacement rather than reshaping

**2. Aluminum**
- Durable and easy to machine, useful for prototypes and support brackets
- Pros: durable, easy to machine, readily available, good for brackets and support pieces
- Cons: heavier than carbon fiber, transmits more vibration, less ideal where low mass is critical

**3. 3D-Printed Plastic**
- Useful for prototypes, custom brackets, and sensor mounts
- Pros: inexpensive, easy to prototype and customize, good for sensor holders and non-critical mounts
- Cons: weaker than carbon fiber or aluminum for primary load-bearing structure, can deform or crack under repeated stress, not ideal for full structural arms

### Selected Material
Hybrid Carbon Fiber + 3D-Printed Parts

### Justification
This hybrid approach provides the structural advantages of carbon fiber while preserving the flexibility of additive manufacturing. Carbon-fiber arms and primary structural members provide the stiffness and low weight needed for efficient and stable flight. 3D-printed parts enable easy customization of microphone mounts, electronics brackets, and experimental sensor placements during development. This combination supports both structural performance and iterative design capability.

---

## 3D Printing Filament

### Options Considered

**1. PLA**
- Standard entry-level 3D printing filament
- Pros: easy to print, inexpensive, good for quick prototypes
- Cons: brittle, lower heat resistance, not ideal for load-bearing or long-term drone parts
- Estimated Price: ~$20–$25 per 1 kg spool

**2. PETG**
- Tougher and more heat-resistant than PLA
- Pros: tougher than PLA, better heat resistance, easier to print than nylon, good layer adhesion, suitable for indoor/outdoor parts
- Cons: not as stiff as carbon-fiber reinforced materials, less ideal than engineering nylon for high-performance brackets
- Estimated Price: ~$20–$30 per 1 kg spool

**3. Nylon**
- Strong and impact resistant engineering material
- Pros: strong and impact resistant, better for functional parts than PLA, more durable under mechanical stress
- Cons: harder to print, absorbs moisture, more demanding storage and print setup requirements
- Estimated Price: ~$30–$50 per 1 kg spool

**4. Carbon-Fiber Reinforced Nylon**
- Example: MatterHackers NylonX (PA12 nylon with ~20% chopped carbon fiber)
- Pros: strong and stiff, better suited for engineering parts, useful for final brackets where rigidity matters
- Cons: expensive, requires better printer capability and filament handling, more difficult than PETG
- Estimated Price: ~$60–$70 per 0.5 kg spool

### Selected Filament
PETG for Prototyping, Carbon-Fiber Reinforced Nylon for Final Mounts

### Justification
PETG is selected for early prototypes because it is easier to print, durable, and reasonably priced. Carbon-fiber reinforced nylon is selected as the preferred final material for high-performance custom brackets and sensor mounts because it offers greater stiffness and strength. PLA is not selected for critical parts due to its brittleness and unsuitability for mechanically stressed drone components.

---

## Final Frame Design Summary

| Aspect | Selection |
|--------|-----------|
| Frame Configuration | H-Frame |
| Primary Structure | Carbon fiber |
| Custom Mounts | 3D-printed (PETG for prototyping, CF-reinforced nylon for final) |

### Justification
This design provides the optimal balance of stability, payload space, low weight, structural strength, and custom sensor integration. The H-frame configuration is especially well suited for an acoustic measurement drone because the larger central section improves packaging and simplifies microphone placement and vibration isolation—critical factors for acoustic measurement quality. Carbon fiber supports efficient and rigid flight, while 3D-printed custom parts allow the system to adapt during testing and refinement.



## High-Level Solution

This section presents a comprehensive, high-level solution aimed at efficiently fulfilling all specified requirements and constraints. The solution is designed to maximize stakeholder goal attainment, adhere to established constraints, minimize risks, and optimize resource utilization. Please elaborate on how your design accomplishes these objectives.


### Hardware Block Diagram

Block diagrams are an excellent way to provide an overarching understanding of a system and the relationships among its individual components. Generally, block diagrams draw from visual modeling languages like the Universal Modeling Language (UML). Each block represents a subsystem, and each connection indicates a relationship between the connected blocks. Typically, the relationship in a system diagram denotes an input-output interaction.

In the block diagram, each subsystem should be depicted by a single block. For each block, there should be a brief explanation of its functional expectations and associated constraints. Similarly, each connection should have a concise description of the relationship it represents, including the nature of the connection (such as power, analog signal, serial communication, or wireless communication) and any relevant constraints.

The end result should present a comprehensive view of a well-defined system, delegating all atomic responsibilities necessary to accomplish the project scope to their respective subsystems.


### Operational Flow Chart

Similar to a block diagram, the flow chart aims to specify the system, but from the user's point of view rather than illustrating the arrangement of each subsystem. It outlines the steps a user needs to perform to use the device and the screens/interfaces they will encounter. A diagram should be drawn to represent this process. Each step should be represented in the diagram to visually depict the sequence of actions and corresponding screens/interfaces the user will encounter while using the device.


## Atomic Subsystem Specifications

Based on the high-level design, provide a comprehensive description of the functions each subsection will perform.

Inclued a description of the interfaces between this subsystem and other subsystems:
- Give the type of signal (e.g. power, analog signal, serial communication, wireless communication, etc).
- Clearly define the direction of the signal (input or output).
- Document the communication protocols used.
- Specifying what data will be sent and what will be received.

Detail the operation of the subsystem:
- Illustrate the expected user interface, if applicable.
- Include functional flowcharts that capture the major sequential steps needed to achieve the desired functionalities.

For all subsystems, formulate detailed "shall" statements. Ensure these statements are comprehensive enough so that an engineer who is unfamiliar with your project can design the subsystem based on your specifications. Assume the role of the customer in this context to provide clear and precise requirements.


## Ethical, Professional, and Standards Considerations

Ethical, Professional, and Standards Considerations

The design and implementation of the Autonomous Acoustic Measurement Drone are influenced by ethical responsibilities, professional engineering standards, and regulatory requirements. These considerations directly impose constraints on system design, operation, and data handling to ensure safety, compliance, and responsible engineering practice.

### Public Safety and FAA Regulations

The operation of the Autonomous Acoustic Measurement Drone is subject to federal aviation regulations established by the Federal Aviation Administration under Title 14 of the Code of Federal Regulations (14 CFR) Part 107 – Small Unmanned Aircraft Systems (sUAS) [1]. These regulations impose strict operational and safety constraints that directly influence the system design and testing procedures.

### Applicable FAA Regulations (14 CFR Part 107)

The system shall comply with the following key FAA requirements:

§107.12 – Remote Pilot Certification
The drone shall be operated by a certified remote pilot or under the direct supervision of one [1].

§107.15 – Condition for Safe Operation
The system shall be in a safe and airworthy condition prior to each flight, requiring pre-flight inspection procedures [1].

§107.23 – Hazardous Operation
The drone shall not be operated in a careless or reckless manner that could endanger life or property [1].

§107.31 – Visual Line of Sight (VLOS)
The drone shall remain within the visual line of sight of the operator at all times [1].

§107.35 – Operation of Multiple Aircraft
The operator shall not control multiple drones simultaneously, ensuring full attention to one system [1].

§107.39 – Operation Over Human Beings
The drone shall not operate over people unless it meets specific safety categories (which this system does not), therefore all testing must occur in controlled environments [1].

§107.41 – Operation in Controlled Airspace
The drone shall not operate in controlled airspace without authorization, requiring approval (e.g., LAANC) if near airports [1].

§107.49 – Preflight Familiarization, Inspection, and Actions

The operator shall perform a preflight inspection, including checks of:

control systems
battery levels
communication links
structural integrity [1]

§107.51 – Operating Limitations for Small UAS
The system shall operate within the following limits:

Maximum altitude: 400 feet above ground level (AGL)
Maximum groundspeed: 100 mph (87 knots)
Daylight or civil twilight operations only (unless equipped for night operations) [1]

### Design Constraints Derived from FAA Regulations

These regulations impose direct constraints on the system design:

1. Flight Stability and Reliability
The drone shall maintain stable flight under normal operating conditions
A reliable flight controller (e.g., ArduPilot/Pixhawk) shall be used to ensure controlled operation

2. Fail-Safe Mechanisms
To comply with §107.23 (hazardous operation), the system shall include:

Return-to-home (RTH) in case of signal loss
Automatic landing during low battery conditions
Failsafe disarm or hover stabilization

3. Weight and Structural Safety
The frame and components shall be structurally secure to prevent mid-air failure
All mounted components (battery, sensors, microphone) shall be firmly secured

4. Controlled Testing Environment
All flights shall be conducted in designated test areas away from people
Indoor or isolated outdoor testing environments shall be prioritized

5. Operator Visibility and Control
The system shall support manual override via remote controller
The drone shall remain visible to the operator without reliance solely on FPV systems

### Remote Identification (Remote ID) Requirement

Under FAA rules, most drones are required to comply with Remote Identification (Remote ID) regulations [2].

The drone shall either include a Remote ID broadcast module or operate within a FAA-recognized identification area (FRIA)
The system shall transmit identification and location information during flight, if required [2]

This requirement influences:
communication system design
onboard electronics integration


### Ethical Responsibility: Safety of Users and Bystanders

As engineers, there is an ethical obligation to prioritize human safety and minimize harm. Improper drone operation could result in injury or equipment damage.

To mitigate these risks:

The system shall include propeller guards or protective design considerations
The drone shall not operate directly above individuals during testing
Pre-flight checks shall be required before each operation
The system shall include manual override capability for emergency intervention

These requirements ensure that the design aligns with the fundamental engineering principle of public safety first [3].

### Privacy and Data Ethics

The use of onboard sensors, including microphones and cameras, introduces privacy concerns, particularly in public or occupied environments.

To address ethical data use:

The system shall only collect acoustic data relevant to the experiment
The system shall not record or store personally identifiable information (PII)
Any recorded data shall be stored securely and used strictly for academic purposes
The drone shall be operated with transparency, informing stakeholders when data collection is occurring

These considerations align with ethical data handling practices in engineering systems [3].

### Professional Engineering Standards

The project adheres to the ethical principles outlined by the Institute of Electrical and Electronics Engineers Code of Ethics [3], which emphasizes:

safety, health, and welfare of the public
honesty and transparency in data reporting
responsible use of technology

To align with these standards:

The system shall produce accurate and reliable acoustic measurements
All results shall be documented truthfully without manipulation
The design shall be reviewed and tested to ensure functionality and safety
Industry and Technical Standards

The system design is also influenced by relevant technical and industry standards, including:

Radio Technical Commission for Aeronautics (RTCA) guidelines for UAV communication and reliability [4]
International Organization for Standardization (ISO) standards for safety, quality, and risk management [5]

Additionally, acoustic measurement practices are informed by established methods used in professional audio engineering tools such as Smaart [6].

To comply with these:

The system shall use calibrated sensors where possible
The system shall maintain signal integrity for accurate measurements
Communication systems shall be reliable and resistant to interference


### Environmental and Sustainability Considerations

The environmental impact of the system is minimal but still considered:

The drone shall use rechargeable battery systems to reduce waste
The system shall minimize energy consumption during operation
Materials used in the frame (e.g., carbon fiber, polymers) shall be selected for durability to reduce frequent replacement
Economic and Societal Impact

This project has the potential to reduce the cost and time associated with acoustic measurements in large venues. However, it also raises considerations regarding workforce impact.

The system shall be designed as an assistive tool, not a full replacement for skilled engineers
The technology shall aim to improve efficiency while maintaining human oversight


###  Summary of Design Constraints Imposed

As a result of these ethical, professional, and standards considerations, the system must:

* comply with FAA flight regulations
* ensure safe operation in all testing scenarios* protect user and public privacy
* adhere to IEEE ethical standards
* follow established acoustic measurement practices
* maintain reliable and accurate system performance


### References 

[1] Federal Aviation Administration, 14 CFR Part 107 – Small Unmanned Aircraft Systems, 2024.
[2] Federal Aviation Administration, Remote Identification of Unmanned Aircraft Final Rule, 2021.
[3] IEEE, IEEE Code of Ethics, 2020.
[4] RTCA, Minimum Operational Performance Standards for UAS, 2022.
[5] ISO, ISO 9001: Quality Management Systems, 2015.
[6] Rational Acoustics, Smaart Acoustic Measurement Software Documentation, 2023.


## Resources

You have already estimated the resources needed to complete the solution. Now, let's refine those estimates.

### Budget

Develop a budget proposal with justifications for expenses associated with each subsystem. Note that the total of this budget proposal can also serve as a specification for each subsystem. After creating the budgets for individual subsystems, merge them to create a comprehensive budget for the entire solution.

### Division of Labor

First, conduct a thorough analysis of the skills currently available within the team, and then compare these skills to the specific requirements of each subsystem. Based on this analysis, appoint a team member to take the specifications for each subsystem and generate a corresponding solution (i.e. detailed design). If there are more team members than subsystems, consider further subdividing the solutions into smaller tasks or components, thereby allowing each team member the opportunity to design a subsystem.

### Timeline

Revise the detailed timeline (Gantt chart) you created in the project proposal. Ensure that the timeline is optimized for detailed design. Address critical unknowns early and determine if a prototype needs to be constructed before the final build to validate a subsystem. Additionally, if subsystem $A$ imposes constraints on subsystem $B$, generally, subsystem $A$ should be designed first.


## References

All sources utilized in the conceptual design that are not considered common knowledge must be properly cited. Multiple references should be included.


## Statement of Contributions

Each team member is required to make a meaningful contribution to the project proposal. In this section, each team member is required to document their individual contributions to the report. One team member may not record another member's contributions on their behalf. By submitting, the team certifies that each member's statement of contributions is accurate.

