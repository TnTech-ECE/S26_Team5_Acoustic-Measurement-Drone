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

&nbsp; &nbsp; &nbsp; &nbsp; In large stadiums and performance venues, sound systems are designed to provide clear and consistent audio coverage across wide audience areas. However, listeners often experience variations in clarity, timing, and tonal balance depending on their seating location. Music or speech may appear delayed, uneven, or less intelligible in certain regions of the venue. These issues commonly arise from timing misalignment between distributed loudspeaker systems and from insufficient spatial resolution in acoustic measurements during system tuning.

&nbsp; &nbsp; &nbsp; &nbsp; This project addresses these challenges by focusing not only on the alignment of delay systems, but also on improving the spatial density, consistency, and usability of acoustic measurement data across the entire sound system. By leveraging acoustic measurement tools and system-tuning software such as Smaart, the project aims to develop a practical and repeatable workflow that enables engineers to evaluate system performance more effectively and make informed adjustments based on spatially distributed data.

&nbsp; &nbsp; &nbsp; &nbsp; The objective is to optimize loudspeaker timing, coverage, and tonal balance throughout the venue rather than at isolated measurement locations. By improving how acoustic data is collected and utilized during system tuning, the proposed workflow seeks to support clearer, more consistent sound reproduction across the entire audience area while aligning with real-world measurement practices.

## Restating the Fully Formulated Problem

&nbsp; &nbsp; &nbsp; &nbsp; Modern live-event production depends on sound reinforcement systems capable of delivering consistent coverage, clarity, and tonal balance across diverse venues. Achieving these outcomes requires accurate characterization of acoustic energy propagation throughout the listening environment, including the combined effects of direct sound, reflections, and reverberation. Sound-system engineering relies heavily on objective acoustic measurements to support loudspeaker alignment, equalization, and overall system optimization.

&nbsp; &nbsp; &nbsp; &nbsp; In current professional workflows, acoustic measurements are performed by manually repositioning a measurement microphone throughout the audience area. These measurements are analyzed using industry-standard tools such as Smaart to evaluate system performance in terms of frequency response, timing, and level consistency. While this methodology is well established and widely accepted, it is inherently limited by the time, labor, and accessibility constraints associated with manual microphone placement.

&nbsp; &nbsp; &nbsp; &nbsp; As a result, only a limited number of spatial measurement locations can typically be sampled during the setup period of a live event. This restricts the spatial resolution of the collected acoustic data, particularly in large, architecturally complex, or multi-tiered venues. Areas such as elevated seating sections, balconies, or obstructed regions may be under-sampled or entirely unmeasured, requiring engineers to approximate system performance between measurement points.

&nbsp; &nbsp; &nbsp; &nbsp; The measured acoustic response at a given listener position reflects the combined influence of both the loudspeaker system and the acoustic environment. This response may be modeled as the convolution of the excitation signal x(t) with the acoustic impulse response h(t), producing the measured signal y(t):

$$
y(t)=x(t)∗h(t) \quad (1)
$$ 

&nbsp; &nbsp; &nbsp; &nbsp; Because the impulse response varies spatially due to room geometry, reflections, and boundary conditions, accurate characterization of venue performance requires measurements across many distributed positions. Spatial variations in acoustic behavior can occur even between nearby listener locations, making dense and repeatable sampling essential for achieving consistent system optimization.

&nbsp; &nbsp; &nbsp; &nbsp; Although modern measurement tools provide highly accurate analysis capabilities, they remain dependent on manual data acquisition and therefore inherit practical limitations related to efficiency and coverage. These limitations reduce the ability to fully characterize complex acoustic environments within the time constraints of real-world production workflows.

&nbsp; &nbsp; &nbsp; &nbsp; Therefore, the fundamental problem is the lack of an efficient and repeatable method for collecting spatially dense acoustic measurement data across a venue. Current approaches limit measurement coverage and rely heavily on manual effort, which can result in incomplete system characterization and reduced consistency in sound system performance.

### Specifications

**System Capabilities**
- The system shall autonomously navigate a defined measurement region within an indoor or outdoor performance venue.
- The system shall carry a lightweight microphone system capable of capturing acoustically meaningful data for comparative analysis of frequency response, timing, and sound pressure level trends.
- The system shall provide a continuous real-time audio signal suitable for acoustic analysis at the ground station.
- The system shall transmit audio data to a ground-station computer for real-time analysis using Smaart.
- The system shall support transfer-function-based measurement workflows using excitation signals generated by the venue loudspeaker system.
- The system shall include onboard processing hardware capable of performing signal conditioning and real-time digital signal processing prior to transmission.
  
**Modularity and Expandability**

- The system shall allow replacement or modification of sensing modules and processing components.
- The system may support alternative microphone types or upgraded front-end circuits for improved performance.
- The system may support additional onboard processing or filtering techniques for future system improvements.
- The system shall provide accessible signal and data interfaces to support integration with external analysis platforms.

**Physical Reliability**

- The system shall maintain stable operation under typical indoor venue airflow and vibration conditions.
- The system shall include protective structures to safeguard sensing and electronic components during operation and transport.
- The system shall be capable of safe landing or shutdown in the event of communication or navigation failure.
  
### Constraints

**Regulatory Compliance**

- The system shall comply with applicable aviation and indoor drone operation regulations within the deployment region.
- Wireless communication systems shall operate within approved frequency allocations for unlicensed devices.

**Operational Guidelines**

- The system shall operate only within controlled environments approved for autonomous flight testing.
- The system shall not interfere with venue audio systems or measurement signals during operation.
- The system shall provide a measurement signal that can be evaluated and captured externally by the system operator using Smaart.

**Safety and Environmental Guidelines**

- The system shall incorporate protective measures to prevent injury to personnel or damage to venue infrastructure.
- The system shall include emergency shutdown functionality to ensure safe operation in fault conditions.

## Comparative Analysis of Potential Solutions

In this section, various potential solutions are hypothesized, design considerations are discussed, and factors influencing the selection of a solution are outlined. The chosen solution is then identified with justifications for its selection.

### Coding Subsystem

The coding subsystem is the central control architecture for the autonomous acoustics measurement drone. It is responsible for executing the preset mission path, coordinating with onboard flight and sensing hardware, maintaining communication with the custom handheld controller, supporting immediate manual override during emergencies, and triggering the transfer of audio measurement information to a separate laptop running audio-processing software such as SMAART. Because this subsystem connects mission autonomy, operator supervision, safety response, and measurement coordination, several possible implementation approaches can be considered. This section outlines candidate solutions, discusses the main design considerations, and identifies the selected solution with justification.

#### Potential Solution 1: Fully Manual Control with Operator-Triggered Measurements

The first possible solution is a fully manual flight architecture in which the operator pilots the drone at all times using the handheld controller and manually positions the drone at each measurement location. Once the drone reaches a desired point, the operator triggers or allows the measurement to be transmitted to the laptop for audio processing.

This solution offers the advantage of simplicity. It avoids the complexity of autonomous path execution, waypoint management, and mission-resume logic. It also gives the operator complete control over drone movement at all times, which may appear safer during early prototyping because the pilot can respond directly to unexpected behavior.

However, this approach does not align well with the primary goal of the project, which is to create a fully autonomous acoustics measurement system. Manual operation introduces inconsistency in point placement, hover time, and measurement repeatability. It also increases pilot workload and makes measurements more dependent on operator skill. Since repeatability is important for comparing sound behavior across locations, a fully manual solution would reduce the technical value of the system. For this reason, it may be useful only as an early backup mode, not as the primary coding subsystem design.

#### Potential Solution 2: Fully Autonomous Drone with No Manual Override

A second possible solution is a fully autonomous architecture in which the drone executes a preset path from start to finish without any operator ability to interrupt mission control except through a full shutdown or mission abort. In this case, the handheld device would function mainly as a passive monitor rather than an active supervisory controller.

This solution has the advantage of clean automation logic. The coding subsystem can focus entirely on mission execution, navigation through preset points, point-based measurement triggering, and automatic return-to-land behavior. Since the operator is not expected to intervene except in extreme cases, the control logic can be simpler than systems that switch between autonomy and manual authority.

The major disadvantage is that this design conflicts with the intended role of the custom handheld controller. Your concept clearly requires the controller to act as the main mission supervision system and to provide an autonomy kill switch that allows immediate takeover whenever needed. Removing that functionality would weaken safety, reduce operator confidence, and make the system less practical in real testing. It would also make future expansion harder, since supervisory control is one of the most valuable long-term features of the platform. As a result, this option is too rigid for the project goals.

#### Potential Solution 3: Autonomous Preset Path with Handheld Supervisory Controller and Manual Override

A third solution is a supervised autonomy architecture in which the drone normally flies a preset autonomous path, but the operator continuously oversees the mission through the custom handheld controller. The controller provides drone vitals, mission progress, mode indication, and an immediate autonomy kill switch. If an issue occurs, the operator can disable autonomy, assume manual control, resolve the issue, and then either resume the mission, command a return, or land the drone.

This solution directly matches the intended vision of the project. It combines the repeatability and reduced workload of automation with the flexibility and safety of human oversight. The drone can reliably move between predefined measurement points while the operator remains ready to intervene if needed. This approach is especially appropriate for a first implementation in a controlled environment such as a football field, where the mission is simple and predefined but still benefits from an emergency fallback.

Its main challenge is that the software must manage mode transitions correctly. The subsystem must switch cleanly between autonomous and manual flight authority, preserve mission state during interruption, and verify whether it is safe to resume autonomy afterward. Even though this adds complexity, it is a worthwhile tradeoff because it reflects both the practical needs of field testing and the long-term usefulness of the system.

#### Potential Solution 4: Autonomous Preset Path with Laptop as the Primary Control Interface

Another possible solution is to make the laptop the central mission-control interface. In this design, the laptop would manage mission initiation, show drone status, supervise mission progress, and perhaps even allow manual override, while also receiving measurement data for SMAART or related software.

This solution may appear attractive because the laptop already participates in the measurement process and can support a larger interface with more visual information. It could simplify development by consolidating mission setup, data monitoring, and audio-processing visibility into a single device.

Despite that advantage, this approach is not ideal for your project. Your concept clearly places the custom handheld controller at the center of operational control. The laptop’s main job is to receive and process the audio measurement information, not to function as the pilot or mission-supervision console. Combining these roles would blur the system architecture and reduce the clarity of the separate communications design. It would also make the platform less portable and less aligned with your vision of a dedicated handheld control device. Therefore, this solution is less suitable than one centered on the handheld controller.

#### Potential Solution 5: Integrated Single-Link Control and Audio Transmission Architecture

A fifth option is to design the coding subsystem so that both drone control/telemetry and audio measurement transfer share the same communication channel. This could reduce the number of radio interfaces and simplify some hardware integration choices.

The main advantage of this approach is reduced hardware complexity. Fewer links may lower cost, reduce wiring, and make initial integration easier.

However, this solution does not fit the architecture you described. Your vision uses separate transmitter/receiver paths: one for handheld controller communication with the drone and another for measurement data sent to the laptop. Keeping these communication roles separate improves system clarity and reduces the chance that heavy measurement-data transfer interferes with control and safety communication. Since supervisory control and emergency intervention are critical, combining them with measurement transport would create unnecessary risk and reduce modularity. For these reasons, the single-link approach is less desirable than a separated-link solution.

### Design Considerations

Several factors strongly influence the selection of the coding subsystem architecture.

#### Mission Repeatability

A major purpose of the system is to gather measurements at predefined locations in a consistent way. This means the coding subsystem should support autonomous movement to preset points and stable measurement triggering rather than depending on operator piloting skill.

#### Safety and Human Oversight

Even though the mission is autonomous, the system must allow immediate operator intervention. The handheld controller is intended to serve as the primary control center, so manual takeover, autonomy cancellation, and controlled mission resumption are critical design features.

#### Controlled Initial Environment

The initial implementation assumes a relatively simple testing environment, such as a football field with minimal expected obstacles. This means the first version does not need the most complex adaptive path-planning architecture, but it should still be structured for future expansion.

#### Separation of Communication Roles

The project intentionally separates the control/telemetry link from the audio-data link. This separation should be preserved in the coding subsystem so that flight supervision and emergency control remain independent from audio measurement transport.

#### Expandability

The coding subsystem should support future growth. Improvements such as more advanced obstacle handling, more dynamic mission planning, additional telemetry features, or more sophisticated measurement coordination should be possible without replacing the whole architecture.

#### Operator Usability

The handheld controller must provide a clear and practical interface for monitoring drone vitals, mission progress, current mode, and emergency options. The software should therefore be structured around quick supervisory awareness rather than around a complex desktop-style interface.

### Comparative Summary

The fully manual solution is simple, but it fails to deliver the autonomy and repeatability that make the project valuable. The fully autonomous solution without override improves automation purity, but it does not satisfy the need for manual intervention and supervisory control. The laptop-centered control solution does not match the intended role of the handheld controller. The single-link communication solution reduces hardware complexity, but it weakens the clean separation between mission control and audio transport.

The strongest solution is the one that combines preset-path autonomy, continuous supervision through the custom handheld controller, manual takeover capability, and separate communication handling for control and audio data. This solution best matches the intended operating concept and provides the best balance of repeatability, safety, and future expandability.

### Chosen Solution

The selected solution for the coding subsystem is a supervised autonomous mission architecture with manual override and separate audio-data transport.

Under this approach, the drone executes a preset autonomous path to predefined measurement points using onboard mission logic. The custom handheld controller acts as the primary supervisory interface and allows the operator to monitor drone vitals, view mission progress, disable autonomy instantly, manually control the drone when required, and resume autonomous operation when safe. Measurement events are triggered at preset points, and audio information is sent through a separate transmission path to a laptop running SMAART.

### Justification for Selection

This solution was selected because it most accurately reflects the intended project vision while also offering the best engineering balance.

First, it supports the project’s primary goal of autonomous measurement collection. The drone can fly to repeatable preset points and perform measurement actions without requiring constant manual piloting.

Second, it preserves operator authority and safety. The handheld controller remains the main operational interface, and the operator can interrupt autonomy at any time. This is especially important during testing, where manual takeover provides a practical layer of protection.

Third, it preserves the separation between control functions and measurement transport. By keeping the controller link separate from the audio-data link, the system architecture remains cleaner and more robust.

Fourth, it supports future development. The same architecture can later be extended to more advanced environments, smarter mission logic, additional sensing, or more capable controller features without changing the fundamental structure of the coding subsystem.

Finally, it matches the real intended use of the system better than the alternatives. The coding subsystem is not just an autopilot, and it is not just a data logger. It is the core coordination layer that connects autonomous mission execution, emergency operator control, and measurement-system interaction into one practical field-ready workflow.

### Final Conclusion

After comparing several possible coding subsystem approaches, the most appropriate solution is a supervised autonomous architecture centered around a preset flight path, a custom handheld control center, immediate manual override capability, and a separate communication path for audio measurement delivery to the laptop. This approach best satisfies the project’s goals of autonomous operation, repeatable measurement collection, operator safety, and future expandability.

## High-Level Solution

&nbsp; &nbsp; &nbsp; &nbsp; The proposed solution is an autonomous aerial acoustic measurement system designed to improve the efficiency, consistency, and spatial resolution of sound system analysis in performance venues. The system integrates a multirotor drone platform with acoustic sensing hardware, onboard signal conditioning, and wireless communication to enable the collection of spatial acoustic data. This approach addresses the limitations of traditional manual measurement workflows by enabling repeatable, high-density sampling across large and complex environments.

&nbsp; &nbsp; &nbsp; &nbsp; The system operates by executing a predefined set of measurement waypoints distributed throughout the venue. As the drone navigates through these locations, it continuously captures and transmits acoustic data rather than relying on discrete measurement windows. The onboard acoustic subsystem provides a real-time, conditioned audio signal that is transmitted to a ground-station computer. At the ground station, the signal is analyzed using industry-standard tools such as Smaart, where the system operator determines when measurement conditions are appropriate and captures data accordingly.

&nbsp; &nbsp; &nbsp; &nbsp; To satisfy stakeholder requirements, the design prioritizes measurement usefulness, operational efficiency, and safety. Measurement usefulness is achieved through the use of a lightweight electret microphone system, specifically the Countryman B6, combined with a custom analog front-end and onboard digital signal processing. The analog front-end provides biasing, AC coupling, and low-noise preamplification, while the onboard DSP reduces predictable drone-induced noise such as rotor and vibration artifacts prior to wireless transmission. Although the system does not utilize a laboratory-calibrated measurement microphone, it is designed to produce acoustically meaningful data suitable for comparative spatial analysis.

&nbsp; &nbsp; &nbsp; &nbsp; Operational efficiency is improved through autonomous navigation and continuous data availability. By eliminating the need for manual microphone repositioning and discrete capture control, the system enables rapid data collection across multiple positions while allowing the operator to evaluate signal quality in real time. This supports increased spatial sampling density and more flexible measurement workflows within the time constraints typical of live-event environments.

&nbsp; &nbsp; &nbsp; &nbsp; Safety and regulatory compliance are addressed through controlled flight behavior, reduced operating speeds, and the inclusion of fail-safe mechanisms such as emergency shutdown and controlled landing procedures. The system is intended to operate within controlled indoor environments while minimizing risk to personnel and venue infrastructure.

&nbsp; &nbsp; &nbsp; &nbsp; The solution is decomposed into five primary subsystems: the drone frame, external components, internal components, digital signal processing (DSP), and the controller with associated software and user interface. Each subsystem performs a distinct function and is designed with clearly defined interfaces to ensure reliable integration.

&nbsp; &nbsp; &nbsp; &nbsp; The drone frame provides the structural platform and mounting points for all hardware components. The external components subsystem includes propulsion and power elements such as motors, propellers, electronic speed controllers, and the battery, which enable flight and power distribution. The internal components subsystem includes sensors and supporting electronics required for stabilization and state estimation. The DSP subsystem is responsible for acoustic signal acquisition, analog front-end conditioning, real-time digital processing, and wireless transmission. The controller subsystem manages flight control, autonomy, system coordination, and provides the interface between the drone and the operator.

&nbsp; &nbsp; &nbsp; &nbsp; Risks associated with the system, including positional inaccuracies, communication delays, and acoustic contamination from drone noise, are mitigated through design strategies such as stable waypoint hovering, continuous signal monitoring, and targeted onboard signal conditioning. The use of operator-controlled measurement capture further reduces the likelihood of recording invalid or low-quality data by ensuring that measurements are taken only under acceptable conditions.

&nbsp; &nbsp; &nbsp; &nbsp; Finally, the design optimizes resource utilization by leveraging commercially available drone platforms, wireless audio systems, and established acoustic measurement software. This approach reduces development complexity and cost while maintaining flexibility and compatibility with existing industry workflows.

&nbsp; &nbsp; &nbsp; &nbsp; Overall, the proposed solution provides a practical and technically feasible method for autonomous acoustic measurement, enabling improved data collection, reduced labor requirements, and enhanced sound system evaluation in real-world venue environments.

### Hardware Block Diagram

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

## Resources

The autonomous acoustic measurement drone requires a complete system-level design encompassing the physical airframe and the hardware and software architectures governing flight and data acquisition. This project demands a broad range of technical skills including embedded systems design, CAD, digital signal processing, audio engineering, and control systems. Each discipline must function cohesively to produce a platform capable of collecting clean acoustic data while maintaining stable, safe, and autonomous flight.

The system operates under a simplified flat-box venue assumption, eliminating the need for complex environment mapping or SLAM. Autonomous navigation is achieved through preset waypoints, with the Pixhawk 6C Mini managing stabilization and mission execution. Indoor position hold is provided by the Holybro H-Flow optical flow module, removing the dependency on GPS. Obstacle detection and avoidance is handled by the SLAMTEC RPLIDAR C1, which continuously monitors the horizontal plane and actively maneuvers the drone to maintain a minimum safe distance from any detected object.

Acoustic signal quality during flight remains a key technical challenge. Rotor vibration and airflow can introduce significant disturbances into onboard microphone data. The team will evaluate mechanical isolation methods, sensor placement, and digital filtering techniques to reduce these disturbances within budget and payload constraints.

Throughout development, the team will utilize university laboratory equipment, open-source flight firmware, and commercially available components to support efficient prototyping and validation. Rapid prototyping tools such as 3D printing will enable iterative refinement of mounting structures and sensor placement as integration progresses.


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
| **Item** | **Subsystem** | **Description** | **Qty** | **Estimated Cost** |
|---|---|---|---|---|
| **Pixhawk 6C Mini (w/ PM02 V3)** | Internal Components | Central flight controller with battery regulation module | 1 | $150 |
| **Holybro H-Flow** | Internal Components | Optical flow and distance sensor for indoor positioning | 1 | $125 |
| **SLAMTEC RPLIDAR C1** | Internal Components | 360° 2D scanning lidar for obstacle detection | 1 | $69 |
| **Internal Components Total** | | | | **$344** |
| | | | | |
| **PA6-CF Filament** | Frame | Carbon fiber reinforced nylon filament for drone frame fabrication | 1 kg | $80 |
| **Frame Total** | | | | **$80** |
| | | | | |
| **Battery** | External Components | TBD | TBD | TBD |
| **Motors (4x)** | External Components | TBD | 4 | TBD |
| **ESCs (4x)** | External Components | TBD | 4 | TBD |
| **External Components Total** | | | | **TBD** |
| | | | | |
| **Microcontroller** | DSP | TBD | TBD | TBD |
| **Microphone** | DSP | TBD | TBD | TBD |
| **Pre-amp** | DSP | TBD | TBD | TBD |
| **DSP Total** | | | | **TBD** |
| | | | | |
| **Code** | Code | No hardware costs — software only | — | $0 |
| | | | | |
| **PROJECT TOTAL** | | | | **$424 + TBD** |

### Division of Labor

First, conduct a thorough analysis of the skills currently available within the team, and then compare these skills to the specific requirements of each subsystem. Based on this analysis, appoint a team member to take the specifications for each subsystem and generate a corresponding solution (i.e. detailed design). If there are more team members than subsystems, consider further subdividing the solutions into smaller tasks or components, thereby allowing each team member the opportunity to design a subsystem.

As stated in our teams project proposal, each team members respective skills are listed as follows:

- Bernie Friesel - Experience in power systems, controls, and digital signal processing, supported by coursework and laboratory experience. Strong background in circuit design and construction. Proficient in C/C++ and MATLAB programming, with experience in digital system design, microcontrollers, and microprocessors.

- Jackson Phillips - Strong background in FPGA and microcontroller programming, supported by coursework in digital system design and computer architecture. Experience in signals and telecommunications with familiarity in DSP concepts. Proficient in C, C++, and VHDL, with foundational knowledge in power systems.

- Sean Ike - Strong background in CAD, FPGA development, and microcontroller-based systems. Experience in circuit design and construction, supported by coursework in power systems. Proficient in C, C++, and VHDL, with working knowledge of MATLAB and foundational experience in DSP through signals and telecommunications.

- Mashoud Modi - Strong background in embedded systems, microcontrollers, and digital system design. Coursework includes Signals and Systems, Digital System Design, Microcontrollers, PLCs, and Control Systems with lab experience focused on system modeling and implementation. Proficient in C programming and experienced in hardware/software integration and debugging.

- Elliot Lovins - Strong background in CAD, control systems, and physical system design. Competitive robotics experience has strengthened skills in system integration and troubleshooting. Proficient in C/C++ and MATLAB, with coursework in control systems, signals, and telecommunications. Hands-on experience with microcontrollers through robotics and project development.

With these skills in mind, Team 5 has unanimously decided that the division of labor for each described subsystem along with their respective operatives as well as their reasonings are as stated below:

|**Subsystem**|**Description**|**Assigned Operative**|**Reasoning**|
|-------------|---------------|----------------------|-------------|
|**Drone Frame**|This subsystem consists of drone frame configuration, Materials used to construct frame, compartment design for different subsystems, etc. |Mashoud Modi|Chosen for their general skills for the project to visualize the physical compartments for each respective system on the drone.|
|**Internal Components**|This subsystem consists of flight controller selection and configuration, sensor selection and implementation, etc.|Elliot Lovins|Chosen for their experience with robotics and control systems to construct a smart autonomous drone.|
|**External Components**|This subsystem consists of battery calculations and selection, motor calculations and selection, ESC (Electronic Speed Controllers) selection and configuration, etc.|Jackson Phillips|Chosen for their experience in power systems and circuitry to power and drive the drone with its many loads.|
|**Code**|This subsystem consists of autonomous code, handheld controller for emergency situations, flightpath control, etc. |Sean Ike|Chosen for their experience in coding and computer engineering to bring all the subsystems together in cooperation.|
|**DSP (Digital Signal Processing)**|This subsystem consists of a microcontroller for the DSP system, choice of microphone, configuration of digital filtering, etc.|Bernie Friesel|Chosen for their experience in signal processing and audio industry to provide clean, filtered audio data to the audio team for processing.|

### Timeline

Revise the detailed timeline (Gantt chart) you created in the project proposal. Ensure that the timeline is optimized for detailed design. Address critical unknowns early and determine if a prototype needs to be constructed before the final build to validate a subsystem. Additionally, if subsystem $A$ imposes constraints on subsystem $B$, generally, subsystem $A$ should be designed first.


## References

[1] Holybro. "Pixhawk 6C." Holybro, 2024. [Online]. Available: https://holybro.com/collections/flight-controllers/products/pixhawk-6c

[2] Holybro. "Pixhawk 6C Mini." Holybro, 2024. [Online]. Available: https://holybro.com/collections/flight-controllers/products/pixhawk-6c-mini

[3] NeedCode. "UWB vs GPS: When Ultra-Wideband Technology is the Superior Tracking Option." NeedCode, 2024. [Online]. Available: https://needcode.io/uwb-vs-gps-when-ultra-wideband-technology-is-the-superior-tracking-option/

[4] Holybro. "H-Flow Optical Flow and Distance Sensor Module." Holybro, 2024. [Online]. Available: https://holybro.com/products/h-flow

[5] Matha Electronics. "What is Ultrasonic Sensor? How to Use Ultrasonic Sensor?" Matha Electronics, 2022. [Online]. Available: https://www.mathaelectronics.com/a-brief-introduction-on-ultrasonic-sensorworkingapplications/

[6] Meskernel. "LiDAR Sensor vs Distance Sensor: Key Differences & Best Uses." Meskernel, 2024. [Online]. Available: https://meskernel.net/en/lidar-sensor-vs-distance-sensor/

[7] SLAMTEC. "RPLIDAR C1 – Fusion DTOF Laser Scanner." SLAMTEC, 2024. [Online]. Available: https://www.slamtec.com/en/c1

[8] Federal Aviation Administration. "Part 107 – Small Unmanned Aircraft Systems." FAA. [Online]. Available: https://www.faa.gov/newsroom/small-unmanned-aircraft-systems-uas-regulations-part-107

## Statement of Contributions

Jackson Phillips - Power and Propulsion System(Comparative Anlysis, Atomic Subsystem Specifications, Budget), Hardware Block Diagram, Timeline

