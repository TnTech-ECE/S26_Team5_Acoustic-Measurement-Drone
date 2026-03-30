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

This section presents a comprehensive, high-level solution aimed at efficiently fulfilling all specified requirements and constraints. The solution is designed to maximize stakeholder goal attainment, adhere to established constraints, minimize risks, and optimize resource utilization. Please elaborate on how your design accomplishes these objectives.


### Hardware Block Diagram

Block diagrams are an excellent way to provide an overarching understanding of a system and the relationships among its individual components. Generally, block diagrams draw from visual modeling languages like the Universal Modeling Language (UML). Each block represents a subsystem, and each connection indicates a relationship between the connected blocks. Typically, the relationship in a system diagram denotes an input-output interaction.

In the block diagram, each subsystem should be depicted by a single block. For each block, there should be a brief explanation of its functional expectations and associated constraints. Similarly, each connection should have a concise description of the relationship it represents, including the nature of the connection (such as power, analog signal, serial communication, or wireless communication) and any relevant constraints.

The end result should present a comprehensive view of a well-defined system, delegating all atomic responsibilities necessary to accomplish the project scope to their respective subsystems.


### Operational Flow Chart

Similar to a block diagram, the flow chart aims to specify the system, but from the user's point of view rather than illustrating the arrangement of each subsystem. It outlines the steps a user needs to perform to use the device and the screens/interfaces they will encounter. A diagram should be drawn to represent this process. Each step should be represented in the diagram to visually depict the sequence of actions and corresponding screens/interfaces the user will encounter while using the device.


## Atomic Subsystem Specifications

Based on the high-level design, provide a comprehensive description of the functions each subsection will perform.

Include a description of the interfaces between this subsystem and other subsystems:
- Give the type of signal (e.g. power, analog signal, serial communication, wireless communication, etc).
- Clearly define the direction of the signal (input or output).
- Document the communication protocols used.
- Specifying what data will be sent and what will be received.

Detail the operation of the subsystem:
- Illustrate the expected user interface, if applicable.
- Include functional flowcharts that capture the major sequential steps needed to achieve the desired functionalities.

For all subsystems, formulate detailed "shall" statements. Ensure these statements are comprehensive enough so that an engineer who is unfamiliar with your project can design the subsystem based on your specifications. Assume the role of the customer in this context to provide clear and precise requirements.

### Coding Subsystem

#### 1. Revised Coding Subsystem Description

The coding subsystem is the central software system that coordinates the autonomous behavior of the acoustics measurement drone and manages its interaction with the handheld controller, the drone’s onboard electronics, and the external laptop used for audio processing. This subsystem serves as the operational backbone of the project by linking mission setup, autonomous flight, safety monitoring, operator override capability, measurement triggering, telemetry reporting, and mission completion behavior into one coordinated system.

In this project, the coding subsystem is intended to support a fully autonomous measurement mission in which the drone begins from a known launch point, such as the middle of a football field, follows a preset path to predefined measurement locations, pauses or stabilizes at those locations long enough to capture audio measurements, transmits the measurement information to a separate laptop running audio-processing software such as SMAART, and then returns to land at the designated endpoint. Under normal operation, this process occurs without manual piloting.

The subsystem works closely with both internal and external hardware subsystems. On the drone side, it must communicate with onboard systems such as the flight controller, lidar or optical-flow-based positioning aids, and other navigation-related sensors. On the operator side, it must communicate with the custom handheld controller, which acts as the primary mission oversight interface. This controller is not simply a remote control in the traditional sense; it is the mission management and safety interface for the entire platform. It allows the operator to monitor drone vitals, monitor mission progress, view whether the system is in autonomous or manual mode, interrupt autonomy immediately in emergencies, manually fly the drone when needed, and later restore autonomous control when safe to do so.

A key aspect of the project is that manual control and audio data transmission are handled through separate communication paths. One transmitter/receiver path is used between the handheld controller and the drone for command, telemetry, and manual override. A separate transmitter/receiver path is used between the drone’s audio measurement system and the laptop running SMAART. The coding subsystem must therefore manage these communication roles clearly and reliably so that supervisory control and audio measurement transfer do not interfere with each other.

This subsystem is also intended to be expandable. While the initial use case assumes a controlled environment with minimal obstacles and a simple preset mission, the coding subsystem should be designed so that future improvements can add more advanced path planning, more dynamic mission behavior, more robust obstacle handling, and additional operator tools without requiring a full redesign.

#### 2. Operational Intent of the Coding Subsystem

The coding subsystem is intended to perform the following sequence of operations during a standard mission:

- The operator powers on the drone, handheld controller, and laptop audio-processing system.
- The coding subsystem establishes the drone-to-controller telemetry and command connection.
- The subsystem verifies readiness of onboard systems such as the flight controller, localization aids, battery, and mission configuration.
- The subsystem confirms that the selected mission path and preset measurement points are loaded.
- The operator uses the handheld controller to arm the mission and authorize autonomous operation.
- The drone takes off and navigates to the preset measurement points in sequence.
- At each point, the drone stabilizes itself and triggers the collection/transmission of audio measurement data.
- The measurement information is sent to the laptop running SMAART using the separate audio link.
- Throughout the mission, the handheld controller displays drone vitals, mission state, and override options.
- If an unsafe condition occurs, the operator may immediately disable autonomy and assume manual control.
- Once the issue is resolved, the operator may command the subsystem to resume the autonomous mission if mission rules permit.
- After all points are visited, the drone returns and lands at the specified recovery location.

#### 3. Assumptions for Initial Implementation

To keep the first implementation practical and aligned with your concept, the coding subsystem may initially operate under these assumptions:

- The drone begins from a known starting location, such as midfield on a football field.
- The flight path is predefined before takeoff.
- Measurement locations are predefined before takeoff.
- The field is assumed to be clear of obstacles during normal operation.
- The operator remains present with the handheld controller for supervision.
- The laptop running SMAART is present and prepared to receive measurement data.
- Separate radio paths exist for:
    - drone control/telemetry with the handheld controller
    - audio measurement transmission to the laptop

These assumptions simplify the first design while still preserving the core autonomous and supervisory behavior of the project.

#### 4. Expected User Interface

The primary user interface for the coding subsystem is the custom handheld controller. The laptop is not the primary flight-management interface; it is mainly the destination for audio measurement data and audio processing.

##### 4.1 Handheld Controller Interface

The handheld controller should function as the mission command center. It should present the operator with the current condition of the drone and provide the controls necessary to start, stop, interrupt, and supervise the autonomous mission.

##### 4.2 Main Controller Screens

A. System Status Screen

This screen should display:
- controller-to-drone connection status
- autonomy state
- manual-control state
- battery level
- flight controller status
- sensor health status
- position/localization status
- mission loaded / not loaded state
- laptop audio-link status, if available

B. Mission Screen

This screen should display:
- current mission name
- total preset measurement points
- completed points
- current target point
- flight mode
- return-to-home or return-to-land status
- mission timer or elapsed time

C. Manual Override Screen

This screen should provide:
- autonomy kill switch
- manual takeover enable
- manual flight controls
- resume autonomy control
- mission abort
- emergency land

D. Alerts and Fault Screen

This screen should display:
- communication faults
- low battery warning
- sensor readiness failure
- inability to hold position at measurement point
- mission pause state
- return-to-land state
- autonomy disabled state

#### 5. Operation Flowchart for Coding Subsystem



#### 6. Shall Statements for Coding Subsystem

These are rewritten to match your clarified system concept.

##### A. General Purpose and Scope

- The coding subsystem shall coordinate autonomous mission execution, supervisory control, manual override behavior, measurement triggering, telemetry reporting, and mission completion for the acoustics measurement drone.
- The coding subsystem shall interface with the drone flight controller, onboard positioning and sensing devices, the handheld controller, and the external audio measurement transmission system.
- The coding subsystem shall support a mission concept in which the drone follows a preset path to predefined audio measurement locations and returns to land after mission completion.
- The coding subsystem shall support fully autonomous mission execution during normal operation without requiring continuous manual piloting.
- The coding subsystem shall permit the operator to disable autonomous control and take manual control at any point during flight.
- The coding subsystem shall permit restoration of autonomous control after manual takeover when system conditions and mission rules allow safe resumption.

##### B. Mission Configuration and Preset Path Control

- The coding subsystem shall store, load, and execute a preset mission path consisting of ordered measurement points.
- The coding subsystem shall allow the mission path and measurement points to be defined before takeoff.
- The coding subsystem shall command the drone to visit each preset measurement point in the defined sequence unless mission logic authorizes skipping, aborting, or resuming at a different point.
- The coding subsystem shall recognize a defined mission start point and a defined mission end or landing point.
- The coding subsystem shall verify that a valid mission path is loaded before allowing autonomous mission start.
- The coding subsystem shall prevent mission start when required path or point data is missing or invalid.

##### C. Handheld Controller Integration

- The coding subsystem shall use the handheld controller as the primary operator interface for mission supervision and emergency intervention.
- The coding subsystem shall provide the handheld controller with real-time telemetry including at minimum flight mode, battery state, mission progress, and autonomy status.
- The coding subsystem shall display whether the drone is in autonomous mode, manual mode, paused mode, abort mode, or landing mode.
- The coding subsystem shall accept operator commands from the handheld controller for mission start, mission pause, mission abort, manual takeover, autonomy resume, and landing actions.
- The coding subsystem shall continuously monitor the handheld controller input during autonomous operation for emergency override commands.
- The coding subsystem shall process the autonomy kill switch command with priority over normal autonomous mission commands.

##### D. Manual Takeover and Recovery

- The coding subsystem shall immediately disable autonomous waypoint-following commands when the operator activates manual takeover.
- The coding subsystem shall transfer flight authority to the operator when manual takeover is activated.
- The coding subsystem shall preserve the current mission state when manual takeover occurs unless a full mission abort is commanded.
- The coding subsystem shall allow the operator to either continue manual flight, resume autonomy, command return, or land after manual takeover.
- The coding subsystem shall verify that the drone is in a valid recoverable state before resuming autonomy after manual control.
- The coding subsystem shall prevent autonomy resumption when the drone location, mission state, or subsystem health makes safe continuation invalid.
- The coding subsystem shall provide a clear indication on the handheld controller whenever autonomy has been disabled or restored.

##### E. Communication Structure

- The coding subsystem shall support a command-and-telemetry communication path between the handheld controller and the drone.
- The coding subsystem shall support a separate measurement-data communication path between the drone and the laptop audio-processing system.
- The coding subsystem shall treat the handheld-control link and the audio-data link as separate communication functions.
- The coding subsystem shall continue autonomous mission logic based on mission rules when the audio-data link is interrupted, unless that interruption is defined as mission-critical.
- The coding subsystem shall detect and report loss or degradation of the handheld controller communication link.
- The coding subsystem shall detect and report loss or degradation of the audio-data communication link when that link status is available to the subsystem.

##### F. Autonomous Navigation Behavior

- The coding subsystem shall command takeoff, point-to-point navigation, position hold at measurement points, return flight, and landing through the onboard flight-control interface.
- The coding subsystem shall verify that the drone has reached the required tolerance around a preset point before triggering a measurement event.
- The coding subsystem shall verify that the drone is sufficiently stable before marking a measurement point as valid.
- The coding subsystem shall move to the next point only after the current point has been completed, skipped by rule, or aborted.
- The coding subsystem shall support a return-to-land or return-to-home sequence after all measurement points are completed.
- The coding subsystem shall support mission pause behavior that temporarily halts autonomous progression without immediately ending the mission.

##### G. Measurement Triggering and Data Handling

- The coding subsystem shall trigger measurement collection or measurement transmission at each preset point.
- The coding subsystem shall associate each measurement event with mission metadata including point identifier, mission time, and vehicle state.
- The coding subsystem shall transmit audio measurement information to the external laptop system during or immediately after each point event, according to implementation design.
- The coding subsystem shall record whether each measurement point was successfully completed, failed, skipped, or interrupted.
- The coding subsystem shall maintain a mission log containing measurement point events and mission state transitions.
- The coding subsystem shall support future expansion for additional measurement-related metadata without requiring a redesign of the mission-control logic.

##### H. Safety and Fault Handling

- The coding subsystem shall continuously monitor battery status, communication status, and required onboard subsystem health during mission execution.
- The coding subsystem shall notify the operator through the handheld controller when a fault or unsafe condition is detected.
- The coding subsystem shall pause, abort, return, land, or transfer to manual control according to defined fault-response logic.
- The coding subsystem shall inhibit autonomous mission start when required health checks fail.
- The coding subsystem shall support safe handling of controller-link loss according to predefined mission rules.
- The coding subsystem shall support safe handling of mission interruption during flight.
- The coding subsystem shall preserve mission log information when faults, override events, or abort conditions occur.

##### I. Status Monitoring and Operator Awareness

- The coding subsystem shall present the operator with current battery information sufficient to judge whether mission continuation is safe.
- The coding subsystem shall present the operator with current mission progress, including the current point and remaining points.
- The coding subsystem shall present the operator with current flight mode and autonomy status at all times during operation.
- The coding subsystem shall present active warnings and faults in a way that distinguishes critical faults from advisory messages.
- The coding subsystem shall indicate whether manual override is available and whether it is currently active.

##### J. Expandability and Future Improvement

- The coding subsystem shall be modular enough to support future improvements to mission logic, controller functionality, measurement coordination, and safety behavior.
- The coding subsystem shall support future expansion to more complex operating environments than the initial obstacle-free field assumption.
- The coding subsystem shall support future integration of additional sensors or decision logic without requiring complete replacement of the control architecture.
- The coding subsystem shall separate mission supervision functions from measurement-transport functions so that future audio-processing changes do not require redesign of the handheld-controller behavior.
- The coding subsystem shall preserve a clear interface boundary between autonomous mission control, manual override control, and audio-measurement transfer functions.

#### 7. Recommended Implementation View

Based on your explanation, the coding subsystem is best understood as these major software blocks:

- Mission Control Module
Executes the preset path and point order.
- Handheld Controller Interface Module
Displays vitals, mission status, alerts, and accepts override commands.
- Manual Override Manager
Handles autonomy kill switch logic, transfer to manual control, and safe autonomy resumption.
- Flight Interface Module
Exchanges commands and status with the flight controller and onboard navigation-related hardware.
- Measurement Trigger and Logging Module
Marks arrival at preset points, triggers measurement events, and records mission progress.
- Audio Telemetry Interface Module
Sends measurement information to the laptop running SMAART over the separate audio link.
- Safety Supervisor
Monitors for unsafe conditions and forces safe behavior when needed.

#### 8. Refined One-Sentence Summary

The coding subsystem is the central control software that supervises autonomous point-to-point flight, enables immediate manual takeover through a custom handheld controller, coordinates audio measurement events, and ensures the drone can safely complete or interrupt its mission as required.

This version is much closer to the system you described. The next strong step is to rewrite the comparative analysis / potential solutions section so it matches this exact architecture instead of the more generic one from earlier.

## Ethical, Professional, and Standards Considerations

In the project proposal, each team must evaluate the broader impacts of the project on culture, society, the environment, public health, public safety, and the economy. Additionally, teams must consider relevant standards organizations that will inform the design process. A comprehensive discussion should be included on how these considerations have influenced the design. This includes detailing constraints, specifications, and practices implemented as a result, and how these address the identified considerations.


## Resources

You have already estimated the resources needed to complete the solution. Now, let's refine those estimates.

### Budget

Develop a budget proposal with justifications for expenses associated with each subsystem. Note that the total of this budget proposal can also serve as a specification for each subsystem. After creating the budgets for individual subsystems, merge them to create a comprehensive budget for the entire solution.

### Division of Labor

First, conduct a thorough analysis of the skills currently available within the team, and then compare these skills to the specific requirements of each subsystem. Based on this analysis, appoint a team member to take the specifications for each subsystem and generate a corresponding solution (i.e. detailed design). If there are more team members than subsystems, consider further subdividing the solutions into smaller tasks or components, thereby allowing each team member the opportunity to design a subsystem.

As stated in our teams project proposal, each team members respective skills are listed as follows:

- Bernie Friesel - Experience in power systems, controls, and digital signal processing, supported by coursework and laboratory experience. Strong background in circuit design and construction. Proficient in C/C++ and MATLAB programming, with experience in digital system design, microcontrollers, and microprocessors.

- Jackson Phillips - Strong background in FPGA and microcontroller programming, supported by coursework in digital system design and computer architecture. Experience in signals and telecommunications with familiarity in DSP concepts. Proficient in C, C++, and VHDL, with foundational knowledge in power systems.

- Sean Ike - Strong background in CAD, FPGA development, and microcontroller-based systems. Experience in circuit design and construction, supported by coursework in power systems. Proficient in C, C++, and VHDL, with working knowledge of MATLAB and foundational experience in DSP through signals and telecommunications.

- Mashoud Modi - Strong background in embedded systems, microcontrollers, and digital system design. Coursework includes Signals and Systems, Digital System Design, Microcontrollers, PLCs, and Control Systems with lab experience focused on system modeling and implementation. Proficient in C programming and experienced in hardware/software integration and debugging.

- Elliot Lovins - Strong background in CAD, control systems, and physical system design. Competitive robotics experience has strengthened skills in system integration and troubleshooting. Proficient in C/C++ and MATLAB, with coursework in control systems, signals, and telecommunications. Hands-on experience with microcontrollers through robotics and project development.

With these skills in mind, Team 5 has unanimously decided that the division of labor for each described subsystem along with their respective operatives as well as their reasonings are as stated below:

|**SubSystem**|**Description**|**Assigned Operative**|**Reasoning**|
|-------------|---------------|----------------------|-------------|
|**Drone Frame**|This subsystem consists of drone frame configuration, Materials used to construct frame, compartment design for different subsystems, etc. |Mashoud Modi|Chosen for their general skills for the project to visualize the physical compartments for each respective system on the drone.|
|**Internal Components**|This subsystem consists of flight controller selection and configuration, sensor selection and implementation, etc.|Elliot Lovins|Chosen for their experience with robotics and control systems to construct a smart autonomous drone.|
|**External Components**|This subsystem consists of battery calculations and selection, motor calculations and selection, ESC (Electronic Speed Controllers) selection and configuration, etc.|Jackson Phillips|Chosen for their experience in power systems and circuitry to power and drive the drone with its many loads.|
|**Code**|This subsystem consists of autonomous code, handheld controller for emergency situations, flightpath control, etc. |Sean Ike|Chosen for their experience in coding and computer engineering to bring all the subsystems together in cooperation.|
|**DSP (Digital Signal Processing)**|This subsystem consists of a microcontroller for the DSP system, choice of microphone, configuration of digital filtering, etc.|Bernie Friesel|Chosen for their experience in signal processing and audio industry to provide clean, filtered audio data to the audio team for processing.|

### Timeline

Revise the detailed timeline (Gantt chart) you created in the project proposal. Ensure that the timeline is optimized for detailed design. Address critical unknowns early and determine if a prototype needs to be constructed before the final build to validate a subsystem. Additionally, if subsystem $A$ imposes constraints on subsystem $B$, generally, subsystem $A$ should be designed first.


## References

All sources utilized in the conceptual design that are not considered common knowledge must be properly cited. Multiple references should be included.


## Statement of Contributions

Each team member is required to make a meaningful contribution to the project proposal. In this section, each team member is required to document their individual contributions to the report. One team member may not record another member's contributions on their behalf. By submitting, the team certifies that each member's statement of contributions is accurate.

