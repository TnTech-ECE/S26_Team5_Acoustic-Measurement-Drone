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


## Comparative Analysis of Potential Solutions

In this section, various potential solutions are hypothesized, design considerations are discussed, and factors influencing the selection of a solution are outlined. The chosen solution is then identified with justifications for its selection.


## High-Level Solution

&nbsp; &nbsp; &nbsp; &nbsp; The proposed solution is an autonomous aerial acoustic measurement system designed to improve the efficiency, accuracy, and spatial resolution of sound system analysis in performance venues. The system integrates a multirotor drone platform with acoustic sensing hardware, onboard processing, and wireless communication to automate the collection of spatial acoustic data. This approach addresses the limitations of traditional manual measurement workflows by enabling repeatable, high-density sampling across large and complex environments.

&nbsp; &nbsp; &nbsp; &nbsp; The system operates by executing a predefined set of measurement waypoints distributed throughout the venue. At each location, the drone stabilizes and collects acoustic data generated by the loudspeaker system. These measurements are synchronized with positional data and processed using digital signal processing techniques to produce key acoustic metrics, including frequency response, impulse response, and time alignment. The resulting dataset provides a detailed spatial representation of system performance, enabling more accurate and efficient system tuning.

&nbsp; &nbsp; &nbsp; &nbsp; To satisfy stakeholder requirements, the design prioritizes measurement accuracy, operational efficiency, and safety. Measurement accuracy is achieved through the use of a electret microphone, synchronized excitation signals, and validated signal processing methods. The system is designed to produce acoustic measurements comparable to current industry-standard workflows, ensuring that automation does not compromise data quality. Additionally, design considerations such as vibration isolation and noise-aware signal processing are implemented to minimize contamination from drone-generated noise.

&nbsp; &nbsp; &nbsp; &nbsp; Operational efficiency is improved through autonomous navigation and repeatable measurement sequences. By eliminating the need for manual microphone repositioning, the system reduces the time required to collect measurements across multiple locations while enabling increased spatial sampling density. This allows for more comprehensive acoustic characterization within the limited time windows typical of live-event production environments.

&nbsp; &nbsp; &nbsp; &nbsp; Safety and regulatory compliance are addressed through controlled flight behavior, reduced operating speeds, and the inclusion of fail-safe mechanisms such as emergency shutdown and controlled landing procedures. The system is designed to operate within controlled indoor environments while minimizing risk to personnel and venue infrastructure.

&nbsp; &nbsp; &nbsp; &nbsp; The solution is decomposed into five primary subsystems: the drone frame, external components, internal components, control and autonomy software, and acoustic signal processing. Each subsystem performs a distinct function and is designed with clearly defined interfaces to ensure reliable integration. This modular architecture supports parallel development, simplifies troubleshooting, and allows for future system expansion.

&nbsp; &nbsp; &nbsp; &nbsp; Risks associated with the system, including positional inaccuracies, communication delays, and acoustic measurement contamination from drone noise, are mitigated through design strategies such as waypoint validation, synchronized data acquisition, and signal processing techniques for noise reduction. These measures ensure that the system maintains reliable performance under realistic operating conditions.

&nbsp; &nbsp; &nbsp; &nbsp; Finally, the design optimizes resource utilization by leveraging commercially available hardware, open-source flight control systems, and established acoustic measurement techniques. This approach reduces development complexity and cost while maintaining flexibility and scalability.

&nbsp; &nbsp; &nbsp; &nbsp; Overall, the proposed solution provides a practical and technically feasible method for autonomous acoustic measurement, enabling improved data collection, reduced labor requirements, and enhanced sound system optimization in real-world venue environments.


### Hardware Block Diagram

Block diagrams are an excellent way to provide an overarching understanding of a system and the relationships among its individual components. Generally, block diagrams draw from visual modeling languages like the Universal Modeling Language (UML). Each block represents a subsystem, and each connection indicates a relationship between the connected blocks. Typically, the relationship in a system diagram denotes an input-output interaction.

In the block diagram, each subsystem should be depicted by a single block. For each block, there should be a brief explanation of its functional expectations and associated constraints. Similarly, each connection should have a concise description of the relationship it represents, including the nature of the connection (such as power, analog signal, serial communication, or wireless communication) and any relevant constraints.

The end result should present a comprehensive view of a well-defined system, delegating all atomic responsibilities necessary to accomplish the project scope to their respective subsystems.


### Operational Flow Chart

Similar to a block diagram, the flow chart aims to specify the system, but from the user's point of view rather than illustrating the arrangement of each subsystem. It outlines the steps a user needs to perform to use the device and the screens/interfaces they will encounter. A diagram should be drawn to represent this process. Each step should be represented in the diagram to visually depict the sequence of actions and corresponding screens/interfaces the user will encounter while using the device.


## Atomic Subsystem Specifications

### Acoustic Signal Processing Subsystem

**Functional Description**

&nbsp; &nbsp; &nbsp; &nbsp; The acoustic signal processing subsystem is responsible for acquiring, conditioning, and processing audio signals collected by a lightweight microphone system mounted on the drone. The subsystem converts raw acoustic data into usable metrics for evaluating sound system performance across a venue.

&nbsp; &nbsp; &nbsp; &nbsp; Due to payload, power, and safety constraints, the subsystem utilizes a compact electret microphone paired with a wireless transmission system rather than a traditional phantom-powered measurement microphone. This design prioritizes integration with the aerial platform while maintaining sufficient signal fidelity for comparative acoustic analysis.

&nbsp; &nbsp; &nbsp; &nbsp; The subsystem processes the received audio to extract key acoustic characteristics such as relative frequency response, time alignment, and signal level trends. These measurements are used to evaluate spatial variations in system performance and support the overall goal of improving acoustic measurement coverage and efficiency.

**Subsystem Objectives**

The acoustic signal processing subsystem shall:
- acquire audio using a lightweight electret microphone suitable for airborne operation
- support wireless transmission of audio data from the drone to the processing system
- process received signals to extract meaningful acoustic metrics
- provide measurements suitable for relative comparison across spatial locations
- minimize the impact of drone-generated noise and wireless signal artifacts
- associate each measurement with corresponding positional and timing data

**External Components Interface**
| Interface                    | Signal Type         |              Direction | Protocol / Format                               | Data                     |
| ---------------------------- | ------------------- | ---------------------: | ----------------------------------------------- | ------------------------ |
| Electret microphone          | Analog audio        | Input (to transmitter) | Mic-level analog                                | Acoustic pressure signal |
| Wireless transmitter (Shure) | RF wireless         |    Output (from drone) | Analog FM / digital wireless (system-dependent) | Audio signal             |
| Wireless receiver            | Analog audio output |  Input (to DSP system) | Line-level analog                               | Received audio signal    |

**Internal Components Interface**
| Interface                          | Signal Type             | Direction | Protocol                     | Data                      |
| ---------------------------------- | ----------------------- | --------: | ---------------------------- | ------------------------- |
| Audio input (receiver → processor) | Analog or digital audio |     Input | ADC / USB audio / line-in    | Audio samples             |
| Data logging                       | Digital                 |    Output | SD / USB / memory            | Recorded measurement data |
| Timing reference                   | Digital                 |     Input | System clock / software sync | Timestamp                 |

**Control & Autonomy Interface**
| Interface            | Signal Type | Direction | Protocol                | Data                      |
| -------------------- | ----------- | --------: | ----------------------- | ------------------------- |
| Measurement trigger  | Digital     |     Input | UART / software message | Start recording           |
| Measurement complete | Digital     |    Output | UART / software message | Done signal               |
| Position data        | Digital     |     Input | MAVLink / serial        | Coordinates + waypoint ID |

**Ground Station Interface**
| Interface     | Signal Type | Direction | Protocol    | Data                      |
| ------------- | ----------- | --------: | ----------- | ------------------------- |
| Data transfer | Digital     |    Output | USB / Wi-Fi | Audio + processed results |
| Configuration | Digital     |     Input | Software UI | Sample rate, duration     |

**Detailed Operation**

&nbsp; &nbsp; &nbsp; &nbsp; At each waypoint, the control subsystem issues a measurement trigger. The acoustic subsystem records the received wireless audio signal corresponding to the loudspeaker excitation signal.

The recorded signal is digitized and processed to extract:
- relative frequency response characteristics
- time delay between measurement positions
- signal level variations across the venue

&nbsp; &nbsp; &nbsp; &nbsp; Filtering may be applied to reduce low-frequency vibration noise and isolate the frequency range of interest. After processing, the data is tagged with positional metadata and stored or transmitted for further analysis.

**Functional Flowchart**

**Performance Specifications**

The acoustic signal processing subsystem shall satisfy the following performance requirements:
- The subsystem shall support an acoustic analysis bandwidth of at least 20 Hz to 20 kHz.
- The subsystem shall measure frequency response within ±2 dB of a calibrated reference microphone measurement across the specified analysis range.
- The subsystem shall determine time alignment or delay within ±1 ms of the reference measurement process.
- The subsystem shall support a sampling rate sufficient for professional acoustic analysis, with a minimum sampling rate of 44.1 kHz.
- The subsystem shall maintain a signal-to-noise ratio adequate for venue acoustic measurements under expected operating conditions.
- The subsystem shall ensure that drone self-noise remains at least 10 dB below the measured acoustic signal level during valid measurements.
- The subsystem shall support acquisition durations long enough to capture impulse-response and transfer-function information at each waypoint.
- The subsystem shall tag every measurement record with corresponding waypoint and timing metadata.
Constraints

The subsystem is subject to the following constraints:
- onboard processing power may be limited by weight, power, and hardware selection
- rotor noise and airflow may contaminate the microphone signal
- vibration from the airframe may introduce low-frequency measurement artifacts
- wireless bandwidth may not support full-rate real-time streaming of raw audio,
- the subsystem must operate within the overall power budget of the drone platform,
- the subsystem must remain compatible with the project’s safety and timing requirements.

### Detailed Shall Statements

**Functional Requirements**

- The acoustic signal processing subsystem shall acquire audio from at least one calibrated measurement microphone mounted on the drone.
- The subsystem shall digitize the microphone signal using an audio interface or ADC suitable for acoustic measurement applications.
- The subsystem shall process the acquired signal to generate acoustic analysis outputs including frequency response, impulse response, and time-delay information.
- The subsystem shall associate each acoustic measurement with the corresponding waypoint identifier, spatial position, and timestamp.
- The subsystem shall support either onboard processing, post-processing, or a hybrid approach depending on available computational resources.
- The subsystem shall export raw and/or processed acoustic data to another subsystem or operator interface for analysis and validation.
- The subsystem shall provide a measurement-complete indication to the control and autonomy subsystem after data acquisition and processing are complete.

**Signal Integrity Requirements**

- The subsystem shall preserve the amplitude and timing fidelity of the microphone signal sufficiently to support venue acoustic analysis.
- The subsystem shall include filtering, isolation support, or contamination-reduction methods to reduce the impact of rotor noise and mechanical vibration.
- The subsystem shall not introduce frequency-response deviations greater than ±1 dB due to internal signal-conditioning circuitry within the primary analysis band.
- The subsystem shall maintain synchronization between acoustic data and position metadata throughout the measurement sequence.

**Interface Requirements**

- The subsystem shall accept analog microphone input signals from the external components subsystem.
- The subsystem shall accept digital trigger and measurement-control commands from the control and autonomy subsystem.
- The subsystem shall output processed acoustic data and subsystem status to the internal components subsystem and/or ground station interface.
- The subsystem shall communicate digital data using documented protocols supported by the final hardware implementation.
- The subsystem shall support at least one method of data logging for recovery of measurements in the event of wireless communication failure.

**Reliability Requirements**

- The subsystem shall complete repeated measurements at each waypoint without requiring manual reset during normal operation.
- The subsystem shall detect and report invalid or incomplete measurements when signal quality falls below a defined threshold.
- The subsystem shall continue to preserve recorded measurement data in the event of temporary telemetry interruption.
- The subsystem shall operate within the thermal and electrical limits imposed by the drone platform.

**Validation Requirements**

- The subsystem shall produce measurements that are comparable to the current manual venue measurement process using a stationary calibrated microphone.
- The subsystem shall support side-by-side comparison testing against reference measurement equipment.
- The subsystem shall allow measurement quality to be evaluated using metrics such as frequency-response deviation, delay error, and contamination level.

**Major Data Elements**

The subsystem shall send and receive the following data as applicable:

Received Data:
- measurement trigger,
- waypoint identifier,
- position coordinates,altitude,
- timestamp or synchronization signal,
- subsystem configuration parameters.

Sent Data:
- raw microphone samples or buffered audio,
- processed frequency-response data,
- impulse-response data,
- time-alignment / delay values,
- contamination or quality flags,
- subsystem health / ready / complete status.


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

