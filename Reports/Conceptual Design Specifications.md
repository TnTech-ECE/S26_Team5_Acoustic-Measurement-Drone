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

## **Acoustic Signal Processing Subsystem**

&nbsp; &nbsp; &nbsp; &nbsp; Designing the acoustic signal processing subsystem requires careful consideration of measurement quality, system weight, integration complexity, and compatibility with real-world audio workflows. Multiple approaches exist for microphone selection, signal conditioning, processing, and transmission, each presenting tradeoffs between accuracy, practicality, and system feasibility. The following approaches are evaluated to determine a solution that balances performance with the constraints of an airborne measurement platform.

### **Microphone Selection**

**Measurement Microphone Approach**

&nbsp; &nbsp; &nbsp; &nbsp; One potential solution is the use of a laboratory-grade measurement microphone such as the Earthworks M30. These microphones are designed to provide highly accurate and flat frequency response across a wide bandwidth, making them ideal for precision acoustic measurements. When paired with appropriate preamplification and phantom power, they can deliver highly reliable data for transfer-function and impulse-response analysis.

&nbsp; &nbsp; &nbsp; &nbsp; However, this approach introduces significant challenges when applied to an aerial system. Measurement microphones typically require 48 V phantom power, increasing power consumption and necessitating additional power conversion hardware. They are also physically larger and heavier, which negatively impacts drone payload capacity and flight stability. Integration complexity is increased due to the need for balanced XLR connections and external preamplifiers. As a result, while this approach offers the highest measurement accuracy, it is not well suited for a compact, lightweight, and mobile platform.

**Prosumer Lavalier Microphone Approach**

&nbsp; &nbsp; &nbsp; &nbsp; Another option is the use of a consumer or prosumer lavalier microphone such as the Rode Lavalier GO. These microphones are lightweight, inexpensive, and easy to integrate with systems that support standard 3.5 mm TRS inputs. They operate using plug-in power and can be directly connected to many embedded audio systems without additional circuitry.

&nbsp; &nbsp; &nbsp; &nbsp; This approach simplifies implementation and reduces development time; however, it provides limited control over signal conditioning and typically offers lower durability and consistency compared to professional-grade microphones. Additionally, the electrical interface may not be ideal for integration with custom analog front-end circuitry, and performance may vary depending on the specific input configuration. While this solution improves ease of use, it sacrifices robustness and flexibility in system design.

**Professional Lavalier Microphone with Custom Front-End Approach (Selected)**

&nbsp; &nbsp; &nbsp; &nbsp; The selected solution utilizes a professional lavalier microphone, specifically the Countryman B6, combined with a custom-designed analog front-end. The B6 provides an extremely small form factor, low weight, and high durability, making it well suited for aerial deployment. Unlike consumer lavalier microphones, it is designed for professional audio environments and offers improved reliability and consistency.

&nbsp; &nbsp; &nbsp; &nbsp; The use of a custom analog front-end allows for precise control over microphone biasing, AC coupling, and signal amplification. This ensures that the microphone signal is properly conditioned before being digitized, improving overall signal quality and enabling effective downstream processing. While this approach introduces additional design complexity, it provides a balance between performance, integration flexibility, and system feasibility. The selected configuration sacrifices absolute measurement accuracy in favor of portability and practical implementation while still producing acoustically meaningful data for comparative analysis

### **Embedded Processing**

&nbsp; &nbsp; &nbsp; &nbsp; Processing the microphone signal requires consideration of computational capability, power consumption, and system complexity. Multiple approaches were evaluated for implementing signal conditioning and noise reduction.

**No Onboard Processing Approach**

&nbsp; &nbsp; &nbsp; &nbsp; One option is to transmit the raw microphone signal directly to the ground station without any onboard processing. This approach minimizes system complexity and reduces onboard computational requirements. All filtering and analysis could then be performed externally.

&nbsp; &nbsp; &nbsp; &nbsp; While simple, this method allows drone-induced noise, such as rotor and vibration artifacts, to remain embedded in the transmitted signal. This can degrade measurement quality and reduce the usability of the data, particularly in noisy environments. As a result, this approach does not fully address the challenges associated with airborne acoustic measurement.

**High-Performance Embedded System Approach**

&nbsp; &nbsp; &nbsp; &nbsp; Another option is the use of a high-performance embedded system such as the Raspberry Pi 4. This platform provides significant computational power and supports advanced signal processing techniques, including complex filtering and data handling.

&nbsp; &nbsp; &nbsp; &nbsp; However, this approach introduces increased power consumption, system complexity, and potential reliability concerns in real-time operation. Boot time, operating system overhead, and increased integration complexity make this solution less desirable for a lightweight, embedded application requiring deterministic real-time performance.

**Microcontroller-Based DSP Approach (Selected)**

&nbsp; &nbsp; &nbsp; &nbsp; The selected solution utilizes a microcontroller-based platform, specifically the Teensy 4.1 with an audio interface. This platform provides sufficient real-time processing capability to implement filtering techniques while maintaining low power consumption and a compact form factor.

&nbsp; &nbsp; &nbsp; &nbsp; The Teensy platform supports deterministic real-time operation and integrates well with embedded audio systems, making it suitable for continuous signal processing. It allows the implementation of targeted filtering to reduce predictable drone-induced noise prior to transmission. This approach provides a balance between performance and system simplicity while avoiding the overhead associated with more complex embedded systems.

### **Wireless Transmission**

&nbsp; &nbsp; &nbsp; &nbsp; The transmission of the processed audio signal from the drone to the ground station must be reliable, low-latency, and compatible with existing measurement workflows.

**Digital Wireless / Streaming Approach**

&nbsp; &nbsp; &nbsp; &nbsp; One potential solution is to use digital wireless transmission or network-based audio streaming. This approach offers flexibility and the ability to transmit high-quality audio data over modern communication protocols.

&nbsp; &nbsp; &nbsp; &nbsp; However, digital systems may introduce latency, synchronization challenges, and increased implementation complexity. Integration with existing measurement tools may also require additional hardware or software configuration.

**Wired Transmission Approach**

&nbsp; &nbsp; &nbsp; &nbsp; A wired connection would provide the highest signal integrity and eliminate concerns related to wireless interference or compression.

&nbsp; &nbsp; &nbsp; &nbsp; This approach is impractical for a mobile aerial system, as it restricts movement and introduces safety risks associated with tethering.

**Analog Wireless System Approach (Selected)**

&nbsp; &nbsp; &nbsp; &nbsp; The selected solution utilizes an analog wireless system, specifically the Shure ULX. This system provides reliable, low-latency audio transmission and is widely used in professional audio environments.

&nbsp; &nbsp; &nbsp; &nbsp; An additional factor influencing this decision is the availability of the system. The team already has access to the Shure ULX platform, reducing cost and enabling rapid integration and testing. Familiarity with the system also simplifies troubleshooting and deployment.

&nbsp; &nbsp; &nbsp; &nbsp; While analog wireless transmission may introduce some bandwidth limitations and signal coloration, these effects are acceptable for comparative acoustic analysis. The benefits of reliability, simplicity, and compatibility with industry workflows outweigh these limitations.

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

Block diagrams are an excellent way to provide an overarching understanding of a system and the relationships among its individual components. Generally, block diagrams draw from visual modeling languages like the Universal Modeling Language (UML). Each block represents a subsystem, and each connection indicates a relationship between the connected blocks. Typically, the relationship in a system diagram denotes an input-output interaction.

In the block diagram, each subsystem should be depicted by a single block. For each block, there should be a brief explanation of its functional expectations and associated constraints. Similarly, each connection should have a concise description of the relationship it represents, including the nature of the connection (such as power, analog signal, serial communication, or wireless communication) and any relevant constraints.

The end result should present a comprehensive view of a well-defined system, delegating all atomic responsibilities necessary to accomplish the project scope to their respective subsystems.


### Operational Flow Chart

Similar to a block diagram, the flow chart aims to specify the system, but from the user's point of view rather than illustrating the arrangement of each subsystem. It outlines the steps a user needs to perform to use the device and the screens/interfaces they will encounter. A diagram should be drawn to represent this process. Each step should be represented in the diagram to visually depict the sequence of actions and corresponding screens/interfaces the user will encounter while using the device.


## Atomic Subsystem Specifications

### Acoustic Signal Processing Subsystem

**Functional Description**

&nbsp; &nbsp; &nbsp; &nbsp; The acoustic signal processing subsystem is responsible for acquiring, conditioning, and processing audio signals collected by a lightweight microphone system mounted on the drone. The subsystem provides a continuous, real-time audio signal suitable for acoustic analysis using Smaart at the ground station.

&nbsp; &nbsp; &nbsp; &nbsp; The subsystem utilizes a compact electret microphone interfaced through a custom-designed analog front-end circuit. This front-end provides microphone biasing, AC coupling, and low-noise preamplification to convert the raw microphone signal into a conditioned analog signal suitable for digitization.

&nbsp; &nbsp; &nbsp; &nbsp; The conditioned signal is digitized and processed by a Teensy-based embedded digital signal processing platform, where real-time filtering is applied to reduce predictable drone-induced noise such as rotor and vibration artifacts. The processed signal is continuously output as an analog signal.

&nbsp; &nbsp; &nbsp; &nbsp; The analog output is passed through an output conditioning stage and transmitted via a Shure wireless system to the ground station. The received signal is then analyzed using Smaart, where the system operator selects appropriate moments to capture measurements based on signal quality and stability.

**Design Justification**

&nbsp; &nbsp; &nbsp; &nbsp; The selection of a lightweight electret microphone and wireless transmission system represents a design tradeoff between measurement accuracy and system feasibility. Traditional acoustic measurement systems rely on calibrated condenser microphones requiring phantom power; however, these systems introduce significant weight, power consumption, and integration complexity, making them impractical for use on an aerial platform.

&nbsp; &nbsp; &nbsp; &nbsp; The proposed design utilizes a Countryman B6 electret microphone combined with a custom analog front-end circuit, allowing precise control over biasing, gain structure, and signal conditioning prior to digital processing. This approach enables improved signal integrity compared to directly interfacing the microphone with a wireless transmitter.

&nbsp; &nbsp; &nbsp; &nbsp; Onboard digital signal processing using the Teensy platform allows the subsystem to reduce predictable drone-induced noise before wireless transmission. Performing this processing at the source improves the usability of the transmitted signal and reduces reliance on post-processing.

&nbsp; &nbsp; &nbsp; &nbsp; The system provides a continuous audio stream rather than discrete measurement capture. This design aligns with professional measurement workflows, where the operator uses Smaart to evaluate signal quality, coherence, and environmental conditions before capturing measurement data.

&nbsp; &nbsp; &nbsp; &nbsp; While the system does not achieve laboratory-grade measurement accuracy, it provides sufficient fidelity for comparative acoustic analysis, including spatial variations in level, timing, and general frequency response behavior. This approach improves system robustness by maintaining a human-in-the-loop measurement process, reducing the risk of capturing invalid or noisy data.

**Subsystem Objectives**

The acoustic signal processing subsystem shall:
- acquire audio using a lightweight electret microphone suitable for airborne operation
- implement a custom analog front-end including biasing, AC coupling, and preamplification
- digitize and process the microphone signal using onboard DSP
- apply real-time digital filtering to reduce predictable drone-induced noise
- provide a continuous real-time audio output suitable for analysis using Smaart
- support wireless transmission of conditioned audio to the ground station
- maintain compatibility with industry-standard acoustic measurement workflows

**External Components Interface**
| Interface                | Signal Type        |            Direction | Protocol / Format      | Data                     |
| ------------------------ | ------------------ | -------------------: | ---------------------- | ------------------------ |
| B6 microphone            | Analog (mic-level) |                Input | Electret biased analog | Acoustic pressure signal |
| Front-end → Teensy       | Analog             |                Input | Line-level analog      | Conditioned signal       |
| Teensy → transmitter     | Analog             |               Output | Conditioned line-level | Processed audio          |
| Wireless transmitter     | RF                 |               Output | Shure wireless system  | Audio signal             |
| Wireless receiver        | Analog             | Input (to interface) | Line-level             | Received audio           |
| Audio interface → Smaart | Digital            |                Input | USB / audio driver     | Measurement signal       |

**Internal Components Interface**
| Interface            | Signal Type | Direction | Protocol           | Data                 |
| -------------------- | ----------- | --------: | ------------------ | -------------------- |
| Mic front-end output | Analog      |     Input | ADC (audio shield) | Conditioned signal   |
| DSP processing       | Digital     |  Internal | Audio library      | Filtered samples     |
| Audio output         | Analog      |    Output | DAC                | Processed audio      |
| System timing        | Digital     |     Input | Clock              | DSP timing reference |

**Detailed Operation**

&nbsp; &nbsp; &nbsp; &nbsp; The acoustic signal processing subsystem operates as a continuous onboard audio conditioning and transmission chain. Its purpose is to acquire the acoustic signal at the drone, improve signal quality through analog conditioning and digital filtering, and deliver a real-time audio stream to the ground station for analysis.

&nbsp; &nbsp; &nbsp; &nbsp; During operation, the Countryman B6 microphone converts acoustic pressure into a low-level electrical signal. This signal is routed into a custom analog front-end, which provides microphone biasing, removes DC components through AC coupling, and amplifies the signal using a low-noise preamplifier to a level suitable for digitization.

&nbsp; &nbsp; &nbsp; &nbsp; The conditioned signal is then digitized by the Teensy audio system and processed in real time. The primary objective of this processing stage is to reduce predictable drone-induced noise, such as low-frequency rotor and vibration artifacts, while preserving the integrity of the acoustic signal.

&nbsp; &nbsp; &nbsp; &nbsp; Following processing, the signal is converted back to analog and passed through an output conditioning stage. This stage prepares the signal for compatibility with the Shure wireless transmitter by providing appropriate DC blocking, signal level control, and electrical interfacing.

&nbsp; &nbsp; &nbsp; &nbsp; The processed audio is transmitted continuously to the ground station, where it is received and analyzed using Smaart. Measurement capture is not controlled by the onboard system; instead, the operator monitors the live signal and determines when conditions are suitable for taking measurements. This allows for human verification of signal quality, coherence, and environmental conditions before accepting data.

&nbsp; &nbsp; &nbsp; &nbsp; Overall, the subsystem functions as a real-time signal conditioning and transmission path, enabling the drone to act as a mobile acoustic measurement platform while relying on external tools and operator judgment for analysis and data collection.

**Functional Flowchart**

**Performance Specifications**

The acoustic signal processing subsystem shall satisfy the following:
- Frequency analysis range: 20 Hz to 20 kHz (practical usable band)
- Relative frequency response consistency within ±3 dB across repeated measurements
- Sampling rate: ≥ 44.1 kHz
- Continuous real-time audio output suitable for analysis using Smaart
- Signal-to-noise ratio sufficient to allow meaningful analysis in typical venue conditions
- Drone-induced noise reduced such that it does not dominate the measurement signal within the usable frequency band
- Repeatability within ±3 dB across identical spatial positions under similar conditions

The subsystem is subject to the following constraints:
- The microphone system (Countryman B6) is not a laboratory-calibrated measurement microphone
- Wireless transmission may introduce:
  - bandwidth limitations
  - latency
  - dynamic range compression
- Drone-generated noise, airflow, and movement may affect measurements
- Onboard processing is limited by the computational capability of the Teensy platform
- Power and payload constraints of the aerial platform limit hardware complexity

### Detailed Shall Statements

**Functional Requirements**

- The subsystem shall acquire audio using a lightweight electret microphone integrated with the drone platform.
- The subsystem shall implement a custom analog front-end including biasing, AC coupling, and preamplification.
- The subsystem shall digitize and process the audio signal using onboard DSP.
- The subsystem shall apply real-time filtering to reduce predictable drone-induced noise prior to transmission.
- The subsystem shall output a continuous conditioned audio signal for external acoustic analysis.
- The subsystem shall transmit processed audio to the ground station via a wireless audio link.

**Signal Integrity Requirements**

- The subsystem shall preserve sufficient signal fidelity to enable comparative acoustic analysis across spatial positions.
- The subsystem shall reduce low-frequency vibration and rotor noise through filtering techniques.
- The subsystem shall maintain stable gain and frequency response during operation.
- The subsystem shall minimize distortion introduced by analog and digital processing stages.
- The subsystem shall avoid time-varying artifacts that negatively impact real-time acoustic analysis.
  
**Interface Requirements**

- The subsystem shall accept the microphone signal through the custom analog front-end.
- The subsystem shall provide a conditioned analog output compatible with the Shure wireless transmitter.
- The subsystem shall provide a continuous audio signal suitable for use with Smaart at the ground station.
- The subsystem shall not require communication with the control or autonomy subsystem for normal operation. 

**Reliability Requirements**

- The subsystem shall operate continuously during flight without requiring manual reset.
- The subsystem shall maintain stable operation under vibration and motion conditions.
- The subsystem shall function within the electrical and thermal limits of the drone platform.
  
**Validation Requirements**

- The subsystem shall produce audio suitable for real-time acoustic analysis using Smaart.
- The subsystem shall demonstrate repeatable signal behavior at identical spatial positions.
- The subsystem shall allow comparison with traditional measurement workflows and reference equipment.

**Major Data Elements**

Sent Data:
- continuous processed audio signal (via wireless link)

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

You have already estimated the resources needed to complete the solution. Now, let's refine those estimates.

### Budget

Develop a budget proposal with justifications for expenses associated with each subsystem. Note that the total of this budget proposal can also serve as a specification for each subsystem. After creating the budgets for individual subsystems, merge them to create a comprehensive budget for the entire solution.

### Division of Labor

First, conduct a thorough analysis of the skills currently available within the team, and then compare these skills to the specific requirements of each subsystem. Based on this analysis, appoint a team member to take the specifications for each subsystem and generate a corresponding solution (i.e. detailed design). If there are more team members than subsystems, consider further subdividing the solutions into smaller tasks or components, thereby allowing each team member the opportunity to design a subsystem.

### Timeline

Revise the detailed timeline (Gantt chart) you created in the project proposal. Ensure that the timeline is optimized for detailed design. Address critical unknowns early and determine if a prototype needs to be constructed before the final build to validate a subsystem. Additionally, if subsystem $A$ imposes constraints on subsystem $B$, generally, subsystem $A$ should be designed first.


## References

[1] Federal Aviation Administration, 14 CFR Part 107 – Small Unmanned Aircraft Systems, 2024.
[2] Federal Aviation Administration, Remote Identification of Unmanned Aircraft Final Rule, 2021.
[3] IEEE, IEEE Code of Ethics, 2020.
[4] RTCA, Minimum Operational Performance Standards for UAS, 2022.
[5] ISO, ISO 9001: Quality Management Systems, 2015.
[6] Rational Acoustics, Smaart Acoustic Measurement Software Documentation, 2023.


## Statement of Contributions

Each team member is required to make a meaningful contribution to the project proposal. In this section, each team member is required to document their individual contributions to the report. One team member may not record another member's contributions on their behalf. By submitting, the team certifies that each member's statement of contributions is accurate.

