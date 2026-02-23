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

&nbsp; &nbsp; &nbsp; &nbsp; Have you ever been in a large stadium or venue where the sound was loud and clear, but something felt slightly off depending on where you were sitting? Sometimes music or speech can seem a little late, uneven, or less clear in certain areas. These issues often come from timing misalignment between speakers or from incomplete acoustic measurements of the space.

&nbsp; &nbsp; &nbsp; &nbsp; This project focuses not only on aligning delay towers, but also on improving overall acoustic measurement accuracy across the entire sound system. By using acoustic measurement tools and system-tuning software, we aim to create a practical, repeatable workflow for measuring system performance, making adjustments, and verifying results.

&nbsp; &nbsp; &nbsp; &nbsp; The goal is to ensure that speaker timing, coverage, and tonal balance are optimized throughout the venue, not just in one location. By improving how measurements are taken and applied, the project seeks to deliver clearer, more consistent sound for the entire audience.

&nbsp; &nbsp; &nbsp; &nbsp; The remainder of this proposal outlines the project goals, technical background, methodology, implementation plan, and the metrics used to evaluate overall system performance.

## Formulating the Problem

### Background

&nbsp; &nbsp; &nbsp; &nbsp; Modern live-event production depends on sound reinforcement systems capable of delivering consistent coverage, transparency, and tonal balance across diverse venues. Achieving these outcomes depends on accurately characterizing the propagation of acoustic energy throughout the listening environment. This includes the combined influences of direct sound, reflections, and reverberation. Sound-system engineering relies on heavily accurate acoustic measurements that are essential for properly aligning loudspeakers, applying equalization, and optimizing overall system performance. [1][2].

&nbsp; &nbsp; &nbsp; &nbsp; In current professional workflows, calibrated microphones are manually relocated throughout the audience space to gather measurement data. The data is then used to inform equalization, time alignment, and level optimization decisions. Although this methodology is well established and widely accepted, manually repositioning microphones limits how many measurement locations can realistically be sampled during the tight setup windows common in live-event production. As a result, the spatial resolution of the collected acoustic data is often limited, particularly in large, architecturally complex, or multi-tiered venues.

&nbsp; &nbsp; &nbsp; &nbsp; Using measurement-based system optimization (microphones and software to measure how a sound system actually performs in a room, then adjusting the speakers and settings to make it sound as clear, even, and balanced as possible) is commonly conducted using transfer-function and impulse-response techniques that quantify the relationship between the excitation signal produced by a loudspeaker system and the signal received at a measurement microphone. Industry-standard software such as Smaart is widely used in this process to capture, analyze, and interpret these measurements in real time.

&nbsp; &nbsp; &nbsp; &nbsp; The measured response at any given listener position reflects the combined influence of both the loudspeaker system and the acoustic environment. Consequently, spatial sampling across multiple listening positions is necessary to accurately characterize overall venue performance [1]. Because each acoustic measurement represents a localized response influenced by nearby boundaries and reflections, distributed measurement positions are required to evaluate coverage uniformity and system consistency throughout the audience area [2].

&nbsp; &nbsp; &nbsp; &nbsp; The acoustic response at a listener position may be modeled as the convolution of the excitation signal x(t) with the acoustic impulse response h(t) of the environment, producing the measured signal y(t):

$$
y(t)=x(t)∗h(t) \quad (1)
$$ 

&nbsp; &nbsp; &nbsp; &nbsp; Accurate estimation of h(t) across numerous spatial coordinates is therefore essential for developing reliable representations of venue acoustic behavior and achieving consistent system performance throughout the listening area. Spatial variations in reflections, room geometry, boundary conditions, and audience loading can produce measurable differences in response between nearby listener positions, underscoring the importance of spatially distributed measurement techniques [3].

&nbsp; &nbsp; &nbsp; &nbsp; Modern transfer-function measurement platforms provide highly accurate tools for analyzing magnitude, phase, and impulse-response characteristics of sound systems [4]. However, these tools remain dependent on manual microphone placement and therefore inherit practical limitations related to labor, accessibility, and time constraints. Large venues frequently include seating regions that are difficult to access, elevated balcony sections, or architectural features that limit measurement coverage. In such cases, engineers may be required to approximate acoustic performance in unmeasured areas, potentially resulting in uneven system optimization or additional adjustment time. These operational limitations motivate the exploration of automated measurement approaches capable of collecting spatially dense acoustic data in a more efficient and repeatable manner than traditional manual workflows.

### Specifications

**System Capabilities**

- The system shall autonomously navigate a defined measurement region within an indoor or outdoor performance venue.
- The system shall carry at least one calibrated measurement microphone capable of capturing impulse response, frequency response, and sound pressure level data.
- The system shall transmit collected acoustic data to a ground-station computer for real-time or post-measurement analysis.
- The system shall support synchronized excitation signals generated by the venue loudspeaker system for transfer-function measurement.
- The system shall include onboard processing hardware capable of managing sensor acquisition, navigation control, and data logging.

-  (Maybe) The system shall record spatial position data corresponding to each measurement location to enable three-dimensional acoustic mapping.


**Modularity and Expandability**

- The system shall allow replacement of sensing modules and processing components.
- The system may support additional environmental sensors, such as temperature or humidity sensors, to enable future acoustic-environment modeling.
- The system shall provide accessible data interfaces to support integration with external analysis platforms.

**Physical Reliability**

- The system shall maintain stable operation within typical indoor venue airflow conditions.
- The system shall include protective structures to safeguard sensing equipment during operation and transport.
- The system shall be capable of safe landing or shutdown in the event of communication or navigation failure.

### Constraints

**Regulatory Compliance**

- The system shall comply with applicable aviation and indoor drone operation regulations within the deployment region.
- Wireless communication systems shall operate within approved frequency allocations for unlicensed devices.

**Operational Guidelines**

- The system shall operate only within controlled environments approved for autonomous flight testing.
- The system shall not interfere with venue audio systems or measurement signals during operation.

**Safety and Environmental Guidelines**

- The system shall incorporate protective measures to prevent injury to personnel or damage to venue infrastructure.
- The system shall include emergency shutdown functionality to ensure safe operation in fault conditions.


## Survey of Existing Solutions

## Measures of Success

&nbsp; &nbsp; &nbsp; &nbsp; The success of this project will be evaluated based on the drone system’s ability to achieve precise flight control, maintain operational reliability, and perform accurate acoustic data acquisition during real-world testing and measurement scenarios.The following key performance indicators (KPIs) and verification methodologies will be used to ensure the system meets its control, sensing, safety, and acoustic measurement specifications while operating within defined technical and environmental constraints.

1. Flight Stability and Control Accuracy

Objective: Verify that the drone maintains stable and accurate flight control under varying operating conditions.

Methodology:

Hover Stability Testing: Conduct repeated hover trials to measure positional drift and stability over fixed time intervals.

Control Response Evaluation: Measure the drone’s response time and accuracy to pilot or programmed control inputs.

Disturbance Rejection: Introduce minor external disturbances (e.g., airflow variations) to evaluate stabilization performance.

Sensor Calibration Verification: Validate proper integration and calibration of IMU and onboard sensors for accurate orientation and motion tracking.

2. Autonomous and Signal Processing Performance

Objective: Ensure that the microcontroller and digital signal processing (DSP) components reliably interpret sensor data and execute control algorithms.

Methodology:

Data Processing Latency: Measure the time between sensor input acquisition and control output execution.

Algorithm Validation: Test filtering and control algorithms for consistency and noise reduction in sensor readings.

Communication Reliability: Verify stable data transmission between onboard modules and control interfaces.

Fail-Safe Logic Testing: Simulate signal loss or abnormal inputs to confirm proper autonomous safety responses.

3. System Integration and Hardware Reliability

Objective: Confirm that all hardware components, including the flight controller, sensors, power system, and communication modules, operate cohesively and reliably.

Methodology:

End-to-End System Testing: Perform full-flight trials to validate coordinated subsystem operation.

Component Stress Testing: Evaluate system performance under extended operation to identify overheating or instability.

Power System Evaluation: Monitor voltage levels, current draw, and battery performance during various flight modes.

Connection Integrity Checks: Ensure secure wiring, solder joints, and module interfaces under vibration conditions.

4. Safety and Operational Robustness

Objective: Verify that the drone operates safely and maintains controlled behavior in both normal and abnormal scenarios.

Methodology:

Emergency Shutdown Testing: Confirm reliable activation of kill-switch or emergency stop mechanisms.

Controlled Landing Trials: Evaluate the system’s ability to execute safe landings during low power or signal interruption.

Thermal Monitoring: Assess operating temperatures of critical electronics to prevent overheating risks.

Flight Boundary Testing: Ensure the drone maintains stable operation within defined operational limits.

****************************************************************
5. Cost Efficiency and Expandability

Objective: Demonstrate that the drone system remains cost-effective while supporting future enhancements in control, sensing, and DSP capabilities.

Methodology:

Budget Assessment: Compare total system cost against initial project constraints and comparable commercial platforms.

Modular Design Review: Evaluate the ease of integrating additional sensors, DSP modules, or control upgrades.

Maintenance and Durability Analysis: Track component wear, repair frequency, and long-term usability.

Documentation Verification: Ensure clear technical documentation enables replication and future development.
*********************************************************************


By meeting these success criteria, the project will demonstrate a reliable, stable, and cost-effective drone system that successfully integrates microcontroller-based control and digital signal processing while maintaining safe operation and expandability for future research and development.
## Resources

The autonomous acoustic measurement drone will require a complete system-level design encompassing both the physical airframe and the hardware and software architectures that govern flight and data acquisition. This project demands a broad range of technical skills, including embedded systems design, CAD, digital signal processing and filtering, audio engineering, and control systems. Each of these disciplines must function cohesively to produce a platform capable of collecting clean acoustic data while maintaining stable, safe, and autonomous flight. Achieving both high-quality signal acquisition and reliable aerial performance requires careful integration across electrical, mechanical, and software subsystems.

For safe and reliable autonomous pathing in dynamic environments, the drone will require onboard sensing, state estimation, and real-time control logic. Although modern flight controllers provide basic stabilization, mission-level autonomy demands accurate positioning, obstacle awareness, and robust fail-safe behavior. The feasibility of consistent autonomous performance under varying conditions will be evaluated through iterative testing. This may involve sensor fusion between GPS, inertial measurement units, and proximity sensors, along with control tuning to maintain flight stability while executing onboard data processing.

Another major technical challenge of this project is maintaining acoustic signal quality during flight. Rotor vibration and wind turbulence can introduce significant disturbances into the audio captured by the onboard microphone. The team will evaluate mechanical isolation methods, sensor placement, and digital filtering techniques to reduce these disturbances. Controlled testing will be conducted to determine whether sufficient signal clarity can be achieved while remaining within budget constraints, power limitations, and microphone selection.

Throughout development, the team will utilize university laboratory equipment, open-source flight firmware, and commercially available components to support efficient prototyping and validation. Rapid prototyping tools such as 3D printing will enable iterative refinement of mounting structures and sensor placement as integration progresses.

As development advances, the team will apply systematic testing and iterative design practices to address challenges in autonomy and acoustic performance. By maintaining a strong focus on system-level integration and practical constraints, the team is confident in delivering a functional prototype that demonstrates the feasibility of autonomous acoustic measurement in real-world environments.

### Budget

The total estimated budget for this project is approximately $1,000–$1,500, which covers the essential components required to construct a functional prototype of an autonomous acoustic measurement drone. This includes the drone propulsion system, flight controller, onboard sensing hardware, battery systems, structural components, and acoustic isolation materials necessary for stable flight and reliable audio acquisition.

The budget is intentionally constrained by prioritizing commercially available components, open-source flight firmware, and university-provided laboratory resources. By leveraging in-house prototyping tools such as 3D printing and existing testing equipment, the team aims to minimize cost while maintaining system performance and reliability.

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

Instructor - Dr. Johnson will be the team's instructor. Dr. Johnson will meet with the team weekly to review progress, provide feedback, and offer guidance as needed. He will help ensure the project remains on track and meets overall expectations.

Advisor - Owen O’Connor will be the project's advisor. He will provide technical guidance throughout the project with particular emphasis on embedded systems. He will assist with system design decisions and help address technical challenges as they arise to ensure a reliable and well-executed solution.

Customer - Audio Company will be as the project’s customer and provide industry-based direction throughout development. They will help define system requirements, performance expectations, and practical constraints to ensure the final solution aligns with current industry standards. Ongoing communication with Audio Company will help guide design decisions and ensure the completed project delivers real-world value.

**Team Members:**

Bernie Friesel - Experience in power systems, controls, and digital signal processing, supported by coursework and laboratory experience. Strong background in circuit design and construction. Proficient in C/C++ and MATLAB programming, with experience in digital system design, microcontrollers, and microprocessors.

Jackson Phillips - Strong background in FPGA and microcontroller programming, supported by coursework in digital system design and computer architecture. Experience in signals and telecommunications with familiarity in DSP concepts. Proficient in C, C++, and VHDL, with foundational knowledge in power systems.

Sean Ike - Strong background in CAD, FPGA development, and microcontroller-based systems. Experience in circuit design and construction, supported by coursework in power systems. Proficient in C, C++, and VHDL, with working knowledge of MATLAB and foundational experience in DSP through signals and telecommunications.

Mashoud Modi - Strong background in embedded systems, microcontrollers, and digital system design. Coursework includes Signals and Systems, Digital System Design, Microcontrollers, PLCs, and Control Systems with lab experience focused on system modeling and implementation. Proficient in C programming and experienced in hardware/software integration and debugging.

Elliot Lovins - Strong background in CAD, control systems, and physical system design. Competitive robotics experience has strengthened skills in system integration and troubleshooting. Proficient in C/C++ and MATLAB, with coursework in control systems, signals, and telecommunications. Hands-on experience with microcontrollers through robotics and project development.

### Timeline

The capstone team has one academic year to design and develop an autonomous acoustic measurement drone. During the first semester, the focus will be on research, system architecture, part selection, and detailed design to ensure all major decisions are finalized before summer. The second semester will concentrate on building, integrating, testing, and refining the prototype. If the team follows the timeline outlined in the Gantt chart, the project will result in a functional and validated prototype by December 2026.


## Specific Implications

Explain the implications of solving the problem for the customer. After reading this section, the reader should understand the tangible benefits and the worthiness of the proposed work.


## Broader Implications, Ethics, and Responsibility as Engineers

Consider the project’s broader impacts in global, economic, environmental, and societal contexts. Identify potential negative impacts and propose mitigation strategies. Detail the ethical considerations and responsibilities each team member bears as an engineer.


## References

[1] D. Davis and E. Patronis, Sound System Engineering, 4th ed. New York, NY, USA: Focal Press, 2013.

[2] G. Ballou, Ed., Handbook for Sound Engineers, 5th ed. New York, NY, USA: Focal Press, 2015.

[3] F. A. Everest and K. Pohlmann, Master Handbook of Acoustics, 6th ed. New York, NY, USA: McGraw-Hill, 2015.

[4] M. Lawrence, Between the Lines: Concepts in Sound System Design and Alignment. Petaluma, CA, USA: Rational Acoustics, 2016.

## Statement of Contributions

Each team member must contribute meaningfully to the project proposal. In this section, each team member is required to document their individual contributions to the report. One team member may not record another member's contributions on their behalf. By submitting, the team certifies that each member's statement of contributions is accurate.
