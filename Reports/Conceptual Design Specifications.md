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

