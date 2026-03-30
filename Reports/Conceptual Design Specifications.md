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

### Power and Propulsion Subsystem Design Considerations
**Potential Solutions**

&nbsp; &nbsp; &nbsp; &nbsp; Several configurations were considered for the power and propulsion subsystem, focusing on battery type, motor selection, propeller size, and electronic speed controller (ESC) configuration.

&nbsp; &nbsp; &nbsp; &nbsp; For the battery, both lithium-polymer (LiPo) and lithium-ion (Li-Ion) options were evaluated. LiPo batteries provide higher discharge rates and are commonly used in high-performance drones, while Li-Ion batteries offer higher energy density and improved endurance.

&nbsp; &nbsp; &nbsp; &nbsp; For motor selection, two primary approaches were considered. The first involved larger, low-KV motors such as the Tarot 4112 300KV, which are typically paired with larger propellers (15–16 inches) for maximum efficiency. The second approach involved smaller, lighter motors such as the SunnySky V4008 380KV, which are better suited for mid-sized propellers (12–13 inches) and reduced overall system weight.

&nbsp; &nbsp; &nbsp; &nbsp; Propeller sizes ranging from 12-inch to 15-inch were evaluated. Larger propellers provide higher efficiency and thrust at lower RPMs but require a larger frame and increase system size. Smaller propellers allow for a more compact design but may reduce efficiency and increase power consumption.

&nbsp; &nbsp; &nbsp; &nbsp; For ESC configuration, both 4-in-1 ESCs and individual ESCs were considered. A 4-in-1 ESC offers compact integration and reduced wiring, while individual ESCs provide better thermal distribution, easier replacement, and greater flexibility in larger custom frames.

**Design Considerations**

&nbsp; &nbsp; &nbsp; &nbsp; The design process was influenced by several key factors:

- **Endurance Requirements:** The system must support extended flight time for mapping operations, prioritizing efficiency over speed.
- **Weight Constraints:** Reducing total aircraft mass is critical for improving flight time and reducing required thrust.
- **Frame Size Limitations:** The selected 16 in × 16 in frame restricts the maximum propeller size that can be used.
- **Power Efficiency:** The propulsion system must operate efficiently at hover and low-speed cruise conditions.
- **Component Compatibility:** All components must support a 6S power system and operate within safe electrical limits.
- **Thermal and Reliability Considerations:** ESC and motor selection must ensure safe operation under continuous load conditions.
- **Integration Simplicity:** The design should allow for straightforward integration with the flight controller and payload systems.


**Factors Influencing Final Selection**

&nbsp; &nbsp; &nbsp; &nbsp; The final configuration was selected based on a balance between efficiency, weight, and compatibility with the frame and mission requirements.

- The **Li-Ion battery** was chosen over LiPo due to its higher energy density, enabling longer flight times.
- The **SunnySky V4008 380KV motors** were selected instead of heavier alternatives to reduce total system weight while maintaining sufficient thrust capability.
- The **13-inch propellers** were selected as a compromise between efficiency and frame constraints, providing improved performance over 12-inch props while remaining compatible with the existing frame.
- **Individual ESCs** were selected instead of a 4-in-1 configuration to improve thermal performance and simplify integration within the larger frame.


**Final Design Selection**

&nbsp; &nbsp; &nbsp; &nbsp; The final power and propulsion subsystem configuration consists of:

- 6S 8000 mAh Li-Ion battery  
- SunnySky V4008 380KV brushless motors (×4)  
- HobbyWing XRotor 40A ESCs (×4)  
- APC 13×4.5 multirotor propellers (×4)  
- 16 in × 16 in 3D-printed H-frame  

&nbsp; &nbsp; &nbsp; &nbsp; This configuration provides a balanced solution that meets the endurance requirements of the project while maintaining compatibility with the mechanical design. The selected components reduce unnecessary weight, improve efficiency during hover, and allow the system to achieve an estimated flight time near 20 minutes under optimized operating conditions.


**Justification Summary**

&nbsp; &nbsp; &nbsp; &nbsp; The chosen design represents a compromise between competing design constraints. While larger propellers and motors could improve efficiency, they would require a larger frame and increase system complexity. Conversely, smaller components would reduce size but negatively impact endurance. The selected configuration achieves an effective balance by maximizing efficiency within the constraints of the existing frame and mission requirements.

&nbsp; &nbsp; &nbsp; &nbsp; Overall, the final design supports stable, efficient, and reliable autonomous mapping operation while remaining practical for implementation and integration.

### Computing Architecture

#### Options Considered

**1. Flight Controller (FC)** [[1](#References)]
- Examples: Pixhawk 6C
- Integrated IMU, barometer, flight firmware (PX4 / ArduPilot)
- Pros: built-in stabilization, integrated sensors, fast development, reliable
- Cons: limited low-level control, less flexibility, firmware-dependent, higher cost ($80–$200)

**2. Custom Microcontroller**
- Example: STM32, Raspberry Pi
- Pros: full control, highly customizable, lightweight, very low cost ($10–$30)
- Cons: must build control + sensor fusion, high development time

**3. Flight Controller + Companion Microcontroller**
- FC → low-level control
- MCU → mission-specific tasks
- Pros: combines reliability + flexibility, supports custom processing, scalable
- Cons: added complexity, communication required, higher power, highest total cost ($100–$250)

#### Selected Architecture
Flight Controller Only

#### Justification
Given the simplified mission scope — preset waypoints in a flat-box venue — a standalone flight controller provides sufficient processing capability. Flight controller firmware natively handles stabilization, waypoint navigation, sensor integration, and obstacle avoidance responses without requiring a companion microcontroller. This reduces system complexity, weight, and cost.

---

### Flight Controller Selection

#### Options Considered

**1. Pixhawk 6C (Holybro)** [[1](#References)]
- Price: ~$180–$220 (higher-end, expanded capability)
- Processor: STM32H743
- Sensors: redundant IMUs, onboard barometer and magnetometer
- Connectivity: multiple telemetry ports, dual power inputs — higher expandability and redundancy
- Integration: easier for complex or expanding systems

**2. Pixhawk 6C Mini (Holybro)** [[2](#References)]
- Price: ~$120–$150 (cost-effective, compact design)
- Processor: STM32H743
- Sensors: redundant IMUs, onboard barometer and magnetometer (identical to 6C)
- Connectivity: reduced port availability, single power input — more constrained system design
- Integration: sufficient for fixed, well-defined systems; less flexible for future expansion

#### Selected Flight Controller
Pixhawk 6C Mini

![Pixhawk 6C Mini](Photos/6cmini.png)

#### Justification
- Provides identical processing and sensing performance to the Pixhawk 6C
- Meets all required connectivity needs for the system
- Reduces cost and avoids unnecessary expansion capability
- Simplifies overall system design while maintaining reliability

---


### State Estimation (Pose Estimation)

The state estimation subsystem determines the drone's orientation and relative motion during flight using the onboard sensors of the Pixhawk 6C Mini.

#### Options Considered

**IMU** [[2](#References)]
- Sensors: ICM-42688-P and BMI055 (dual accel/gyro)
- Provides angular velocity and linear acceleration used to estimate roll, pitch, and yaw
- High update rate enables real-time stabilization; dual IMUs improve reliability
- Subject to drift over time and requires vibration isolation for accurate measurements

**Magnetometer** [[2](#References)]
- Sensor: IST8310 (onboard)
- Provides heading reference to correct yaw drift from the IMU
- Improves directional stability during navigation between measurement points
- Sensitive to magnetic interference from motors, wiring, and environment

**Barometer** [[2](#References)]
- Sensor: MS5611 (onboard)
- Provides relative altitude estimation for vertical control and level transitions
- Lightweight and directly integrated with flight controller firmware
- Affected by pressure variation and airflow, limiting precision at small height changes

**Optical Flow / VIO**
- Considered as an optional addition for relative horizontal motion estimation
- Can improve short-range position hold and reduce drift during hover
- Adds additional hardware and integration complexity; performance depends on surface texture and lighting

#### Selected Configuration
- Onboard IMUs (ICM-42688-P, BMI055)
- Onboard magnetometer (IST8310)
- Onboard barometer (MS5611)

#### Justification
- Provides sufficient orientation and relative motion estimation for stable flight and control
- Fully supported by flight controller firmware with minimal additional integration
- Optical flow was evaluated for state estimation but is instead implemented as a dedicated localization sensor, covered in the Localization subsection.

---

### Localization

The localization subsystem estimates the drone's position across the horizontal plane and altitude during autonomous indoor flight to support stable hover and waypoint execution.

#### Options Considered

**1. GPS** [[3](#References)]
- Examples: Here3+, u-blox M9N
- Standard satellite-based localization
- Pros: globally accurate, well supported by ArduPilot/PX4, no additional hardware
- Cons: unreliable indoors due to signal obstruction and multipath interference

**2. Ultra-Wideband (UWB)** [[3](#References)]
- Examples: Pozyx, Marvelmind
- RF time-of-flight ranging between fixed anchors and a drone-mounted tag
- Pros: centimeter-level indoor accuracy
- Cons: requires pre-installed anchor infrastructure throughout the venue; high setup overhead and cost

**3. Optical Flow + Downward Distance Sensor** [[4](#References)]
- Example: Holybro H-Flow
- Tracks surface features beneath the drone for horizontal velocity estimation; downward distance sensor provides altitude hold
- Pros: self-contained, no external infrastructure, lightweight, low cost, native ArduPilot/PX4 support
- Cons: drift over long distances; performance dependent on surface texture and lighting

#### Selected Localization Method
Optical Flow + Downward Distance Sensor (Holybro H-Flow)

![Holybro H-Flow](Photos/hflow.png)

#### Justification
GPS is unsuitable for indoor use. UWB offers accuracy but requires anchor installation incompatible with live-event time constraints. Optical flow requires no external infrastructure, integrates natively with the Pixhawk 6C Mini, and provides sufficient stability for a low-speed preset waypoint mission in a flat-box venue.

---

### Obstacle Detection

The obstacle detection subsystem is responsible for identifying objects within the drone's flight path during navigation between waypoints to prevent collisions and ensure safe operation.

#### Options Considered

**1. Ultrasonic Sensors** [[5](#References)]
- Examples: HC-SR04, MaxSonar EZ series
- Emit sound pulses and measure return time to estimate distance
- Pros: very low cost, simple integration
- Cons: narrow detection cone, slow update rate, susceptible to interference from drone motor noise and acoustic reflections in venue environments

**2. Single-Point ToF Sensor** [[6](#References)]
- Examples: Benewake TFMini, VL53L1X
- Single-axis distance measurement using time-of-flight
- Pros: lightweight, inexpensive, UART/I2C compatible
- Cons: extremely narrow field of view (~2-3°); multiple units required for adequate coverage, increasing wiring complexity

**3. 2D Scanning LiDAR** [[7](#References)]
- Example: SLAMTEC RPLIDAR C1
- Rotating laser scanner providing continuous 360° horizontal distance measurements
- Pros: full horizontal coverage with no blind spots, 12m range, 5KHz sample rate, TTL UART interface, lightweight at 110g, IP54 rated
- Cons: detects obstacles only in the horizontal plane; does not cover above or below the drone

#### Selected Sensor
SLAMTEC RPLIDAR C1

![SLAMTEC RPLIDAR C1](Photos/rplidarc1.png)

#### Justification
Ultrasonic sensors are susceptible to motor noise and provide insufficient coverage. Single-point ToF sensors require multiple units for adequate coverage, adding cost and payload weight. The RPLIDAR C1 provides full 360° coverage in a single lightweight unit, interfaces directly with the Pixhawk 6C Mini via TTL UART, and comfortably meets the demands of a low-speed waypoint mission.


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

### Internal Components Subsystem


#### Connections



The Pixhawk 6C Mini flight controller serves as the central hub of the internal components subsystem, interfacing with all onboard sensors. The Holybro H-Flow optical flow and distance sensor module connects to the Pixhawk 6C Mini via the CAN1 or CAN2 port using the DroneCAN protocol, providing horizontal velocity estimation and altitude data as digital output to the flight controller. The SLAMTEC RPLIDAR C1 2D lidar connects to the Pixhawk 6C Mini via the TELEM2 port using TTL UART serial communication, providing continuous 360° obstacle distance data as digital output to the flight controller. Both sensors receive power directly through their respective connection ports on the Pixhawk 6C Mini, with the flight controller itself powered through the external components subsystem.

#### Specifications

The Pixhawk 6C Mini shall serve as the central flight controller, managing stabilization, waypoint navigation, and sensor integration.
The flight controller shall navigate to each predefined waypoint with a positional accuracy of ±0.5 meters.
The flight controller shall maintain stable hover at each measurement waypoint within the venue.
The flight controller shall execute a predefined waypoint mission without requiring manual input during flight.
The flight controller shall actively maneuver the drone to maintain a minimum safe distance of 3 meters from any detected obstacle in the horizontal plane at all times.
The H-Flow sensor shall provide continuous optical flow and altitude data to the flight controller via DroneCAN protocol.
The H-Flow sensor shall support indoor position hold without reliance on GPS.
The RPLIDAR C1 shall perform continuous 360° horizontal scanning and transmit distance data to the flight controller via TTL UART.
The RPLIDAR C1 shall detect obstacles within a minimum range of 6 meters.
The RPLIDAR C1 shall be mounted with a fixed forward reference aligned to the drone's heading axis to enable directional obstacle response.
The internal components subsystem shall have a combined weight not exceeding 200g.

#### Description

The internal components subsystem integrates the flight controller, localization sensor, and obstacle detection sensor into a unified system responsible for autonomous navigation, position estimation, and collision avoidance during the acoustic measurement mission.

The Pixhawk 6C Mini serves as the central processing unit for all flight operations. It receives sensor data from the H-Flow and RPLIDAR C1, executes the predefined waypoint mission, and manages stabilization throughout flight. Upon arriving at each waypoint, the Pixhawk holds position while the acoustic measurement subsystem captures data, then proceeds to the next waypoint.

The Holybro H-Flow module provides continuous optical flow and downward distance data to the Pixhawk via DroneCAN, enabling stable indoor position hold without GPS. The sensor tracks surface features beneath the drone to estimate horizontal velocity and uses a time-of-flight distance sensor for altitude hold.

The SLAMTEC RPLIDAR C1 performs continuous 360° horizontal scanning and transmits angle and distance data to the Pixhawk via TTL UART. The flight controller monitors incoming scan data and actively maneuvers the drone to maintain a minimum safe distance of 3 meters from any detected obstacle in any horizontal direction at all times. The RPLIDAR C1 is mounted with a fixed forward reference aligned to the drone's heading axis, allowing the flight controller to map scan angles to real-world directions for accurate directional response.

#### Functional Flowchart

![Internal Components Flowchart](Photos/internal_components_flowchart_v3.png)

#### Applicable Standards

- **FAA Part 107:** Regulates autonomous drone operation under U.S. federal law, including maximum altitude, weight limits, and operational safety requirements. [[8](#References)]

#### Implementation & Compliance

- The Pixhawk 6C Mini firmware enforces altitude and speed limits in accordance with FAA Part 107 operational requirements.
- Emergency failsafe behaviors including controlled landing and return-to-home are configured within the flight controller firmware to ensure safe operation in fault conditions.
- The RPLIDAR C1 operates within Class 1 laser safety standards, posing no risk to personnel during venue operation.

#### Design Considerations

- The RPLIDAR C1 must be mounted with a consistent forward reference relative to the drone's heading axis to ensure accurate directional obstacle response.
- Vibration isolation should be considered for the Pixhawk 6C Mini to maintain accurate IMU measurements during flight.
- The H-Flow sensor must be mounted facing downward with an unobstructed view of the floor surface to ensure reliable optical flow performance.
- Surface texture and lighting conditions within the venue may affect H-Flow performance and should be evaluated during testing.

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

