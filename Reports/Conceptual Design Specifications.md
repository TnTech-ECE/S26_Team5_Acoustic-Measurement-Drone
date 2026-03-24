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

&nbsp; &nbsp; &nbsp; &nbsp; In large stadiums and performance venues, sound systems are designed to provide clear and consistent audio coverage across wide audience areas. However, listeners often experience variations in clarity, timing, or tonal balance depending on their seating location. Music or speech may appear slightly delayed, uneven, or less intelligible in certain regions of the venue. These issues commonly arise from timing misalignment between distributed loudspeaker systems or from incomplete acoustic measurements during system tuning.

&nbsp; &nbsp; &nbsp; &nbsp; This project addresses these challenges by focusing not only on the alignment of delay towers, but also on improving the overall accuracy and spatial coverage of acoustic measurements across the entire sound system. Using acoustic measurement tools and system-tuning software, the project aims to develop a practical and repeatable workflow for evaluating system performance, implementing adjustments, and verifying the results.

&nbsp; &nbsp; &nbsp; &nbsp; The objective is to optimize loudspeaker timing, coverage, and tonal balance throughout the venue rather than at isolated measurement locations. By improving how acoustic data is collected and applied during system tuning, the proposed workflow seeks to support clearer, more consistent sound reproduction for the entire audience area.

## Restating the Fully Formulated Problem

&nbsp; &nbsp; &nbsp; &nbsp; Modern live-event production depends on sound reinforcement systems capable of delivering consistent coverage, clarity, and tonal balance across diverse venues. Achieving these outcomes requires accurate characterization of acoustic energy propagation throughout the listening environment, including the combined effects of direct sound, reflections, and reverberation. Sound-system engineering relies heavily on objective acoustic measurements to support loudspeaker alignment, equalization, and overall system optimization [1][2].

&nbsp; &nbsp; &nbsp; &nbsp; In current professional workflows, microphones are manually repositioned throughout the audience space to collect measurement data. This data is used to inform equalization, time alignment, and level optimization decisions. Although this methodology is well established and widely accepted, manual microphone placement limits the number of measurement locations that can realistically be sampled within the tight setup windows typical of live-event production. As a result, the spatial resolution of collected acoustic data is often limited, particularly in large, architecturally complex, or multi-tiered venues.

&nbsp; &nbsp; &nbsp; &nbsp; Measurement-based system optimization is commonly performed using transfer-function and impulse-response techniques, which quantify the relationship between the excitation signal produced by a loudspeaker system and the signal received at a measurement microphone. Industry-standard software such as Smaart is widely used to capture, analyze, and interpret these measurements in real time.

&nbsp; &nbsp; &nbsp; &nbsp; The measured response at any given listener position reflects the combined influence of both the loudspeaker system and the acoustic environment. Consequently, spatial sampling across multiple listening positions is necessary to accurately characterize overall venue performance [1]. Because each acoustic measurement represents a localized response influenced by nearby boundaries and reflections, distributed measurement positions are required to evaluate coverage uniformity and system consistency throughout the audience area [2].

&nbsp; &nbsp; &nbsp; &nbsp; The acoustic response at a listener position may be modeled as the convolution of the excitation signal x(t) with the acoustic impulse response h(t) of the environment, producing the measured signal y(t):

$$
y(t)=x(t)∗h(t) \quad (1)
$$ 

&nbsp; &nbsp; &nbsp; &nbsp; Accurate estimation of h(t) across numerous spatial coordinates is therefore essential for developing reliable representations of venue acoustic behavior and achieving consistent system performance throughout the listening area. Spatial variations in reflections, room geometry, boundary conditions, and audience loading can produce measurable differences in response between nearby listener positions, underscoring the importance of spatially distributed measurement techniques [3].

&nbsp; &nbsp; &nbsp; &nbsp; Modern transfer-function measurement platforms provide highly accurate tools for analyzing magnitude, phase, and impulse-response characteristics of sound systems [4]. However, these tools remain dependent on manual microphone placement and therefore inherit practical limitations related to labor, accessibility, and time constraints. Large venues frequently include seating regions that are difficult to access, elevated balcony sections, or architectural features that limit measurement coverage. In such cases, engineers may be required to approximate acoustic performance in unmeasured areas, potentially resulting in uneven system optimization or increased tuning time. These operational limitations motivate the development of automated measurement approaches capable of collecting spatially dense acoustic data in a more efficient and repeatable manner than traditional workflows.

### Specifications

**System Capabilities**
- The system shall autonomously navigate a defined measurement region within an indoor or outdoor performance venue.
- The system shall carry a lightweight microphone system capable of capturing acoustically meaningful data for comparative analysis of frequency response, timing, and sound pressure level trends.
- The system shall transmit collected acoustic data to a ground-station computer for real-time or post-measurement analysis.
- The system shall support synchronized excitation signals generated by the venue loudspeaker system for transfer-function-based measurements.
- The system shall capture acoustic data in discrete measurement windows at defined spatial locations.
- The system shall associate each measurement with corresponding position and timing metadata for spatial analysis.
- The system shall include onboard processing hardware capable of managing sensor acquisition, signal conditioning, and communication with the ground station.
  
**Modularity and Expandability**

- The system shall allow replacement or modification of sensing modules and processing components.
- The system may support additional environmental sensors, such as temperature or humidity, for future acoustic-environment modeling.
- The system shall provide accessible data interfaces to support integration with external analysis platforms.
  
**Physical Reliability**

- The system shall maintain stable operation within typical indoor venue airflow conditions.
- The system shall include protective structures to safeguard sensing and electronic components during operation and transport.
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


## Comparative Analysis of Potential Solutions

In this section, various potential solutions are hypothesized, design considerations are discussed, and factors influencing the selection of a solution are outlined. The chosen solution is then identified with justifications for its selection.


## High-Level Solution

&nbsp; &nbsp; &nbsp; &nbsp; The proposed solution is an autonomous aerial acoustic measurement system designed to improve the efficiency, consistency, and spatial resolution of sound system analysis in performance venues. The system integrates a multirotor drone platform with acoustic sensing hardware, onboard signal conditioning, and wireless communication to automate the collection of spatial acoustic data. This approach addresses the limitations of traditional manual measurement workflows by enabling repeatable, high-density sampling across large and complex environments.

&nbsp; &nbsp; &nbsp; &nbsp; The system operates by executing a predefined set of measurement waypoints distributed throughout the venue. At each location, the drone stabilizes and initiates a discrete measurement window during which acoustic data generated by the loudspeaker system is captured. The collected audio is transmitted to a ground-station computer, where it is recorded and associated with positional and timing metadata. This data is then analyzed using industry-standard tools to evaluate system performance, including relative frequency response, timing behavior, and spatial consistency.

&nbsp; &nbsp; &nbsp; &nbsp; To satisfy stakeholder requirements, the design prioritizes measurement usefulness, operational efficiency, and safety. Measurement usefulness is achieved through the use of a lightweight electret microphone system, synchronized excitation signals, and front-end signal conditioning techniques that reduce predictable drone-induced noise prior to transmission. While the system does not rely on a laboratory-calibrated measurement microphone, it is designed to produce acoustically meaningful data that supports comparative analysis across spatial locations, ensuring that automation improves workflow efficiency without eliminating practical measurement value.

&nbsp; &nbsp; &nbsp; &nbsp; Operational efficiency is improved through autonomous navigation and repeatable measurement sequences. By eliminating the need for manual microphone repositioning, the system reduces the time required to collect measurements across multiple locations while enabling increased spatial sampling density. This allows for more comprehensive acoustic characterization within the limited time constraints typical of live-event production environments.

&nbsp; &nbsp; &nbsp; &nbsp; Safety and regulatory compliance are addressed through controlled flight behavior, reduced operating speeds, and the inclusion of fail-safe mechanisms such as emergency shutdown and controlled landing procedures. The system is intended to operate within controlled indoor environments while minimizing risk to personnel and venue infrastructure.

&nbsp; &nbsp; &nbsp; &nbsp; The solution is decomposed into five primary subsystems: the drone frame, external components, internal components, control and autonomy software, and acoustic signal processing. Each subsystem performs a distinct function and is designed with clearly defined interfaces to ensure reliable integration. In particular, the flight control subsystem manages navigation and stabilization, the acoustic subsystem performs signal conditioning and measurement coordination, and the ground-station system performs data recording, metadata association, and acoustic analysis. This modular architecture supports parallel development, simplifies troubleshooting, and allows for future system expansion.

&nbsp; &nbsp; &nbsp; &nbsp; Risks associated with the system, including positional inaccuracies, communication delays, and acoustic measurement contamination from drone noise, are mitigated through design strategies such as waypoint validation, discrete measurement windows, synchronized data acquisition, and targeted signal conditioning. These measures ensure that the system maintains reliable performance under realistic operating conditions.

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

&nbsp; &nbsp; &nbsp; &nbsp; The acoustic signal processing subsystem is responsible for acquiring, conditioning, and coordinating the capture of audio signals collected by a lightweight microphone system mounted on the drone. The subsystem performs front-end signal conditioning and supports synchronized measurement collection for evaluating sound system performance across a venue.

&nbsp; &nbsp; &nbsp; &nbsp; Due to payload, power, and safety constraints, the subsystem utilizes a compact electret microphone paired with a wireless transmission system rather than a traditional phantom-powered measurement microphone. This design prioritizes integration with the aerial platform while maintaining sufficient signal fidelity for comparative acoustic analysis.

&nbsp; &nbsp; &nbsp; &nbsp; The subsystem applies simple digital signal processing techniques, such as filtering and signal conditioning, to reduce predictable drone-induced noise prior to wireless transmission. Acoustic measurements are captured as discrete time-windowed recordings at each waypoint and are processed and analyzed at the ground station using external software tools.

**Design Justification**

&nbsp; &nbsp; &nbsp; &nbsp; The selection of a lightweight electret microphone and wireless transmission system represents a design tradeoff between measurement accuracy and system feasibility. Traditional acoustic measurement systems rely on calibrated condenser microphones requiring phantom power; however, these systems introduce significant weight, power consumption, and integration complexity, making them impractical for use on a small aerial platform.

&nbsp; &nbsp; &nbsp; &nbsp; The proposed design instead utilizes a compact electret microphone paired with a Shure wireless transmission system to enable real-time audio transfer from the drone to the ground station. This approach significantly reduces payload weight, simplifies power requirements, and improves overall system reliability during flight operations.

&nbsp; &nbsp; &nbsp; &nbsp; While this configuration does not provide laboratory-grade measurement accuracy, it enables sufficient signal fidelity for comparative acoustic analysis, including spatial variations in sound pressure level, timing differences, and general frequency response trends. These metrics are sufficient to demonstrate the feasibility and effectiveness of autonomous acoustic measurement.

&nbsp; &nbsp; &nbsp; &nbsp; Additionally, performing front-end signal conditioning on the Teensy prior to wireless transmission allows the system to reduce predictable drone-induced noise, such as low-frequency rotor and vibration artifacts. This improves the usability of the transmitted signal without requiring complex or computationally intensive onboard processing.

&nbsp; &nbsp; &nbsp; &nbsp; The decision to perform detailed acoustic analysis at the ground station rather than onboard the drone further reduces system complexity and computational load. By leveraging external software tools such as Smaart, the system maintains compatibility with existing industry workflows while avoiding the need for high-performance embedded processing hardware.

&nbsp; &nbsp; &nbsp; &nbsp; Overall, the selected design balances performance, weight, power consumption, and implementation complexity, resulting in a practical and achievable solution.

**Subsystem Objectives**

The acoustic signal processing subsystem shall:
- acquire audio using a lightweight electret microphone suitable for airborne operation
- apply front-end digital filtering to reduce low-frequency vibration and drone noise
- support wireless transmission of conditioned audio to the ground station
- coordinate discrete measurement windows based on waypoint readiness signals
- enable audio capture suitable for relative spatial comparison across measurement locations
- interface with the control subsystem to synchronize measurement timing

**External Components Interface**
| Interface                  | Signal Type         |         Direction | Protocol / Format                  | Data                     |
| -------------------------- | ------------------- | ----------------: | ---------------------------------- | ------------------------ |
| Electret microphone        | Analog audio        | Input (to Teensy) | Mic-level analog                   | Acoustic pressure signal |
| Teensy → Shure transmitter | Analog audio        |            Output | Conditioned mic-level / line-level | Filtered audio signal    |
| Wireless transmitter       | RF wireless         |            Output | Shure wireless system              | Audio signal             |
| Wireless receiver          | Analog audio output | Input (to laptop) | Line-level analog                  | Received audio signal    |

**Internal Components Interface (Teensy)**
| Interface        | Signal Type            | Direction | Protocol           | Data               |
| ---------------- | ---------------------- | --------: | ------------------ | ------------------ |
| Microphone input | Analog / digital audio |     Input | ADC / audio shield | Raw audio samples  |
| Audio output     | Analog audio           |    Output | DAC / audio output | Filtered audio     |
| Timing reference | Digital                |     Input | System clock       | Measurement timing |

**Control & Autonomy Interface**
| Interface             | Signal Type | Direction | Protocol                | Data                          |
| --------------------- | ----------- | --------: | ----------------------- | ----------------------------- |
| Waypoint ready signal | Digital     |     Input | UART / MAVLink / serial | Waypoint ID + stable flag     |
| Measurement status    | Digital     |    Output | UART / serial           | Measurement active / complete |

**Ground Station Interface**
| Interface      | Signal Type  |         Direction | Protocol          | Data                     |
| -------------- | ------------ | ----------------: | ----------------- | ------------------------ |
| Audio stream   | Analog audio | Input (to laptop) | Audio interface   | Measurement audio        |
| Telemetry data | Digital      | Input (to laptop) | Serial / wireless | Position + waypoint data |
| Data logging   | Digital      |            Output | File system       | Audio + metadata         |

**Detailed Operation**

&nbsp; &nbsp; &nbsp; &nbsp; At each waypoint, the flight control subsystem determines when the drone has reached the desired position and achieved sufficient stability. A waypoint-ready signal is then transmitted to the acoustic subsystem or directly to the ground station.

&nbsp; &nbsp; &nbsp; &nbsp; The Teensy-based subsystem continuously conditions the microphone signal using simple digital filtering techniques. When a measurement trigger condition is met, the system initiates a discrete measurement window, during which the filtered audio signal is transmitted through the wireless system to the ground station.

&nbsp; &nbsp; &nbsp; &nbsp; The ground station records a short-duration audio snippet corresponding to the measurement window. Simultaneously, position and waypoint metadata from the flight controller are logged and associated with the recorded audio. This allows each measurement to be mapped to a specific spatial location within the venue.

&nbsp; &nbsp; &nbsp; &nbsp; The process repeats for each waypoint, enabling efficient spatial sampling of the acoustic environment.

**Functional Flowchart**

**Performance Specifications**

The acoustic signal processing subsystem shall satisfy the following:
- Frequency analysis range: 100 Hz to 10 kHz (practical usable band)
- Relative frequency response agreement within ±3 dB compared to reference measurement
- Time alignment estimation within ±2 ms
- Sampling rate ≥ 44.1 kHz
- Measurement duration: 1–3 seconds per waypoint
- Drone noise level at least 10 dB below measured signal during valid measurements
- Repeatability within ±3 dB across identical measurement positions

The subsystem is subject to the following constraints:
- Microphone system is not a laboratory-calibrated measurement microphone
- Wireless transmission may introduce:
  - bandwidth limitations
  - latency
  - dynamic range compression
- Drone-generated noise and airflow may affect measurements
- Limited onboard processing capability (Teensy)
- Power and payload constraints of aerial platform
  
### Detailed Shall Statements

**Functional Requirements**

- The subsystem shall acquire audio using a lightweight electret microphone integrated with the drone platform.
- The subsystem shall apply simple digital filtering to reduce predictable drone-induced noise prior to transmission.
- The subsystem shall support discrete measurement windows triggered by waypoint readiness conditions.
- The subsystem shall transmit conditioned audio to a ground-based system for recording and analysis.
- The subsystem shall coordinate measurement timing with the control and autonomy subsystem.
  
**Signal Integrity Requirements**

- The subsystem shall preserve sufficient signal fidelity to enable comparative acoustic analysis across measurement locations.
- The subsystem shall reduce low-frequency vibration and rotor noise through filtering techniques.
- The subsystem shall not introduce excessive distortion that prevents meaningful comparison between measurement positions.
  
**Interface Requirements**

- The subsystem shall accept waypoint readiness signals from the flight controller via a serial communication interface.
- The subsystem shall output conditioned audio to the wireless transmission system.
- The subsystem shall communicate measurement status signals to the control subsystem or ground station.
  
**Reliability Requirements**

- The subsystem shall support repeated measurement cycles without manual reset.
- The subsystem shall operate continuously during multi-point measurement sequences.
- The subsystem shall maintain stable operation within the electrical and thermal limits of the drone platform.
  
**Validation Requirements**

- The subsystem shall produce measurements that allow relative comparison of acoustic behavior across spatial positions.
- The subsystem shall demonstrate reasonable agreement with traditional measurement methods for timing and level trends.
- The subsystem shall support side-by-side comparison testing with reference measurement equipment.
  
**Major Data Elements**

The subsystem shall send and receive the following data as applicable:

Received Data:
- waypoint identifier
- position coordinates
- stability flag
- measurement trigger condition
  
Sent Data:
- filtered audio signal (via wireless link)
- measurement status (start/complete)
- associated metadata handled at ground station

## Ethical, Professional, and Standards Considerations

In the project proposal, each team must evaluate the broader impacts of the project on culture, society, the environment, public health, public safety, and the economy. Additionally, teams must consider relevant standards organizations that will inform the design process. A comprehensive discussion should be included on how these considerations have influenced the design. This includes detailing constraints, specifications, and practices implemented as a result, and how these address the identified considerations.


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

