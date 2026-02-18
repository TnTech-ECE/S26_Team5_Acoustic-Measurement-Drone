# Project Proposal

This document provides a comprehensive explanation of what a project proposal should encompass. The content here is detailed and is intended to highlight the guiding principles rather than merely listing expectations. The sections that follow contain all the necessary information to understand the requirements for creating a project proposal.


## General Requirements for the Document
- All submissions must be composed in markdown format.
- All sources must be cited unless the information is common knowledge for the target audience.
- The document must be written in third person.
- The document must identify all stakeholders including the instuctor, supervisor, and customer.
- The problem must be clearly defined using "shall" statements.
- Existing solutions or technologies that enable novel solutions must be identified.
- Success criteria must be explicitly stated.
- An estimate of required skills, costs, and time to implement the solution must be provided.
- The document must explain how the customer will benefit from the solution.
- Broader implications, including ethical considerations and responsibilities as engineers, must be explored.
- A list of references must be included.
- A statement detailing the contributions of each team member must be provided.


## Introduction

The introduction must be the opening section of the proposal. It acts as the "elevator pitch" of the project, briefly introducing the objective, its importance, and the proposed solution. Because readers may only read this section, it should effectively capture their attention and encourage them to read further.

Toward the end of the introduction, include a subsection that outlines what the proposal will cover. This helps set reader expectations for the ensuing sections.


## Formulating the Problem

Formulating the problem or objective involves clearly defining it through background information, specifications, and constraints. Think of it as "fencing in" the objective to make it unambiguously clear what is and is not being addressed and why.

Questions to consider:
- Who does the problem affect (i.e. who is your customer)?
- Why do we need this solution?
- What challenges necessitate a dedicated, multi-person engineering team?
- Why aren’t off-the-shelf solutions sufficient?

### Background

Provide context and details necessary to define the problem clearly and delineate its boundaries.

### Specifications and Constraints

Specifications and constraints define the system's requirements. They can be positive (do this) or negative (don't do that). They can be mandatory (shall or must) or optional (may). They can cover performance, accuracy, interfaces, or limitations. Regardless of their origin, they must be unambiguous and impose measurable requirements.

#### Specifications

Specifications are requirements imposed by **stakeholders** to meet their needs. If a specification seems unattainable, it is necessary to discuss and negotiate with the stakeholders.

#### Constraints

Constraints often stem from governing bodies, standards organizations, and broader considerations beyond the requirements set by stakeholders.

Questions to consider:
- Do governing bodies regulate the solution in any way?
- Are there industrial standards that need to be considered and followed?
- What impact will the engineering, manufacturing, or final product have on public health, safety, and welfare?
- Are there global, cultural, social, environmental, or economic factors that must be considered?


## Survey of Existing Solutions

Research existing solutions, whether in literature, on the market, or within the industry. Present these findings in a coherent, organized manner. Remember to cite all information that is not common knowledge.


## Measures of Success

Define how the project’s success will be measured. This involves explaining the experiments and methodologies to verify that the system meets its specifications and constraints.


## Resources

The autonomous acoustic measurement drone will require a complete system-level design encompassing both the physical airframe and the hardware and software architectures that govern flight and data acquisition. This project demands a broad range of technical skills, including embedded systems design, CAD, digital signal processing and filtering, audio engineering, and control systems. Each of these disciplines must function cohesively to produce a platform capable of collecting clean acoustic data while maintaining stable, safe, and autonomous flight. Achieving both high-quality signal acquisition and reliable aerial performance requires careful integration across electrical, mechanical, and software subsystems.

For safe and reliable autonomous pathing in dynamic environments, the drone will require onboard sensing, state estimation, and real-time control logic. Although modern flight controllers provide basic stabilization, mission-level autonomy demands accurate positioning, obstacle awareness, and robust fail-safe behavior. The feasibility of consistent autonomous performance under varying conditions will be evaluated through iterative testing. This may involve sensor fusion between GPS, inertial measurement units, and proximity sensors, along with control tuning to maintain flight stability while executing onboard data processing.

Another major technical challenge of this project is maintaining acoustic signal quality during flight. Rotor vibration and wind turbulence can introduce significant disturbances into the audio captured by the onboard microphone. The team will evaluate mechanical isolation methods, sensor placement, and digital filtering techniques to reduce these disturbances. Controlled testing will be conducted to determine whether sufficient signal clarity can be achieved while remaining within budget constraints, power limitations, and microphone selection.

Throughout development, the team will utilize university laboratory equipment, open-source flight firmware, and commercially available components to support efficient prototyping and validation. Rapid prototyping tools such as 3D printing will enable iterative refinement of mounting structures and sensor placement as integration progresses.

As development advances, the team will apply systematic testing and iterative design practices to address challenges in autonomy and acoustic performance. By maintaining a strong focus on system-level integration and practical constraints, the team is confident in delivering a functional prototype that demonstrates the feasibility of autonomous acoustic measurement in real-world environments.

### Budget

Provide a budget proposal with justifications for expenses such as software, equipment, components, testing machinery, and prototyping costs. This should be an estimate, not a detailed bill of materials.

| **Item**                         | **Description**                                         | **Estimated Cost**      |
|----------------------------------|---------------------------------------------------------|--------------------------|
| **3D Printing Filament**         | Material for producing the body of the drone.           | $40 – $80                |
| **Motors (4x)**                  | Brushless motors selected for stable thrust             | $160 – $240              |
| **Electronic Speed Controllers (ESCs)** | Motor controllers that ensure stable and efficient propulsion. | $70 – $100              |
| **Propellers (Sets + Spares)**   | Propellers optimized for stability and efficiency | $10 – $30                |
| **Battery (LiPo/Li-ion)**               | Primary onboard power source sized for payload and flight duration | $100 – $200              |
| **Flight Controller**            | Central flight control unit responsible for stabilization, navigation, and autonomous path execution. | $150 - $200       |
| **GPS Module**                   | Provides positioning data for autonomous navigation | $40 – $60                |
| **Vibration Isolation Materials**| Damping materials and mounting hardware to reduce mechanical interference | $30 – $80       |
| **Wiring, Connectors, Hardware** | Electrical connectors, cabling, and integration components | $25 – $50        |


### Personel

**Faculty:**

Instructor - 

Advisor - 

Customer - 

**Team Members:**

Bernie Friesel -

Jackson Phillips - 

Sean Ike - 

Mashoud Modi

Elliot Lovins - 



### Timeline

Provide a detailed timeline, including all major deadlines and tasks. This should be illustrated with a professional Gantt chart.


## Specific Implications

Explain the implications of solving the problem for the customer. After reading this section, the reader should understand the tangible benefits and the worthiness of the proposed work.


## Broader Implications, Ethics, and Responsibility as Engineers

Consider the project’s broader impacts in global, economic, environmental, and societal contexts. Identify potential negative impacts and propose mitigation strategies. Detail the ethical considerations and responsibilities each team member bears as an engineer.


## References

All sources used in the project proposal that are not common knowledge must be cited. Multiple references are required.


## Statement of Contributions

Each team member must contribute meaningfully to the project proposal. In this section, each team member is required to document their individual contributions to the report. One team member may not record another member's contributions on their behalf. By submitting, the team certifies that each member's statement of contributions is accurate.
