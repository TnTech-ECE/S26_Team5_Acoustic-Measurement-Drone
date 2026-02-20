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

&nbsp; &nbsp; &nbsp; &nbsp;Several research efforts have explored the use of drones for acoustic measurement, noise analysis, and sound localization in different environments. Our team has surveyed existing academic solutions that most closely align with our objective of using a drone-based platform to capture acoustic data while mitigating drone self-noise. Two systems in particular help our design approach: **Urban Traffic Noise Analysis Using a UAV-Based Array of Microphones** and **An Acoustic Source Localization Method Using a Drone-Mounted Phased Microphone Array**.

---

### Urban Traffic Noise Analysis Using a UAV-Based Array of Microphones

&nbsp; &nbsp; &nbsp; &nbsp;This system presents a UAV-based platform designed to measure and map urban traffic noise in three dimensions. The researchers mounted a multi-microphone array on a drone and used onboard digital signal processing to reduce rotor noise and extract useful environmental sound data. The system records sound pressure levels (SPL) while tagging each measurement with altitude and position data, enabling the construction of 3D noise maps of urban environments. A major focus of the work is mitigating UAV self-noise using adaptive filtering and reference microphones oriented toward the rotors.

&nbsp; &nbsp; &nbsp; &nbsp;The system uses a large MEMS microphone array and performs real-time digital signal processing on an FPGA, including PDM-to-PCM conversion, FIR filtering, and adaptive least-mean-squares (LMS) noise cancellation. Reference microphones are used to model rotor noise, which is then subtracted from the measurement microphones. This architecture demonstrates that UAV-based acoustic measurement is feasible when supported by sufficient hardware and processing capability.

**Pros**

- **Demonstrated feasibility of UAV-based acoustic measurement:** Confirms that drones can be used to collect meaningful environmental sound data.  
- **Explicit handling of drone self-noise:** Uses reference microphones and adaptive LMS filtering to suppress rotor noise.  
- **3D noise mapping capability:** Combines audio data with altitude and positional metadata to generate spatial noise maps.  
- **High-performance DSP implementation:** FPGA-based processing enables parallel filtering and real-time operation.  

**Cons**

- **High system complexity:** Requires a large microphone array, FPGA development, and significant DSP expertise.  
- **Array-based measurement focus:** Emphasizes spatial noise mapping rather than single-point measurements.  
- **Post-processing dependence:** While adaptive filtering is used, the system is not optimized for real-time audio analysis.  
- **Limited portability of design:** The size of the array reduce viability in constrained environments such as concert venues.

**Gaps**

- **No focus on soundcheck measurements:** SPL accuracy suitable for professional audio tools is not addressed.  
- **No microphone separation strategy:** Drone noise mitigation relies primarily on DSP rather than physical separation of microphones.  
- **Limited relevance to indoor venues:** The system is primarily demonstrated in outdoor urban environments.  

**Takeaways**

- **Drone acoustic measurement is viable:** This work validates the core concept of airborne sound measurement.  
- **Drone self-noise must be addressed:** Adaptive filtering and reference sensors are necessary but not sufficient alone.  
- **Metadata is critical:** Position and altitude tracking are important for meaningful spatial interpretation of acoustic data.  
- **Simplification is needed:** For our project, a smaller number of microphones and a physically isolated measurement mic may provide better results.

---

### An Acoustic Source Localization Method Using a Drone-Mounted Phased Microphone Array

&nbsp; &nbsp; &nbsp; &nbsp;This research presents a drone-mounted phased microphone array designed to localize acoustic sources on the ground. The system uses a 32-channel MEMS microphone array arranged in a circular geometry beneath the drone. By exploiting phase differences between microphones, the system estimates the direction of arrival (DOA) of sound using delay-and-sum beamforming. The estimated DOA is then fused with drone navigation data (position and altitude) to compute the location of the sound source.

&nbsp; &nbsp; &nbsp; &nbsp;To mitigate drone self-noise, the authors use spectral subtraction across multiple frequency bands, modeling rotor noise separately from the signal of interest. This approach preserves phase information, which is critical for accurate localization. The system was tested using impulsive sound sources and was able to estimate sound directions with an average error of about 9–10 degrees at distances of roughly 150 meters.

**Pros**

- **High spatial awareness:** Phased array enables estimation of sound direction rather than simple amplitude measurement.  
- **Useful DSP techniques:** Uses spectral subtraction and delay-and-sum beamforming, both well-understood and implementable techniques.  
- **Navigation-audio fusion:** Demonstrates how IMU and position data must be integrated with acoustic processing.  
- **Quantified performance metrics:** Provides real-world error measurements for DOA estimation.  

**Cons**

- **Large microphone array requirement:** 32 microphones significantly increase system weight, power, and complexity.  
- **Focus on localization, not measurement accuracy:** The system prioritizes source direction over calibrated SPL values.  
- **Post-measurement processing:** Not designed for live audio analysis or real-time sound system measurements.  
- **Limited applicability indoors:** Accuracy depends on clear measurement paths and sufficient distance from reflective surfaces.

**Gaps**

- **No support for professional audio software:** Does not integrate with sound analysis tools used in live production.  
- **No mechanical noise isolation:** Microphones are rigidly mounted to the drone, increasing susceptibility to vibration and airflow noise.  
- **Unnecessary for point measurements:** The array of microphones may be unnecessary when only a single measurement point is required.  

**Takeaways**

- **Array processing improves spatial insight:** Directional information can be extracted using phase-based methods.  
- **Drone orientation matters:** Accurate acoustic interpretation requires fusing audio data with altitude and position information.  
- **Noise suppression must preserve phase:** Spectral subtraction is better for aggressive filtering when phase information is needed.  
- **Simpler strategies:** For soundcheck applications, a single calibrated microphone with physical isolation may outperform large arrays in accuracy and simplicity.

---

&nbsp; &nbsp; &nbsp; &nbsp;Together, these two systems demonstrate that drone-based acoustic sensing is technically feasible but highlight the trade-offs between complexity, accuracy, and applicability. While both approaches rely heavily on microphone arrays and advanced DSP to mitigate drone noise, our project seeks to simplify the architecture by prioritizing **physical microphone isolation**, **stable indoor flight**, and **compatibility with professional sound analysis tools**. The insights gained from these studies directly inform our design constraints, confirming the need for deliberate self-noise mitigation, precise spatial metadata, and a balance between system sophistication and real-world usability.


## Measures of Success

Define how the project’s success will be measured. This involves explaining the experiments and methodologies to verify that the system meets its specifications and constraints.


## Resources

Each project proposal must include a comprehensive description of the necessary resources.

### Budget

Provide a budget proposal with justifications for expenses such as software, equipment, components, testing machinery, and prototyping costs. This should be an estimate, not a detailed bill of materials.

### Personel

Identify the skills present in the team and compare them to those required to complete the project. Address any skill gaps with a plan to acquire the necessary knowledge.

Besides the team, also state who you choose to be you supervisor and why.

State who your instrucotr is and what role you expect them to play in the project.

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
