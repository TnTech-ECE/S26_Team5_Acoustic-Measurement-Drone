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



The proposed autonomous acoustic-measurement drone system extends beyond a technical solution for venue sound optimization. Its implementation carries broader global, economic, environmental, and societal implications that must be carefully evaluated. As engineers, the project team bears responsibility for ensuring that innovation is pursued in a manner that prioritizes safety, equity, transparency, sustainability, and regulatory compliance.

### Global and Industry Impacts

The live-event industry is a global enterprise encompassing concerts, conferences, houses of worship, sporting venues, and cultural institutions. A system capable of rapidly collecting spatially dense acoustic data may significantly improve sound-system commissioning workflows worldwide. By enabling more comprehensive acoustic characterization, the technology may contribute to improved listener experiences across diverse cultural and geographic contexts. The importance of accurate spatial measurement in achieving consistent system performance is well documented in professional sound engineering literature [1][2].

Automated acoustic mapping may improve system verification by allowing more measurement locations to be sampled than is typically practical with manual microphone placement methods [2][4]. However, automation shall not replace professional judgment. The system shall function as a decision-support tool that complements the expertise of trained system engineers.

### Economic Implications

The system may reduce labor hours associated with manual microphone relocation and repeated measurement passes. Improved spatial coverage may decrease troubleshooting time during system commissioning, thereby lowering operational costs for production companies and venue operators. Traditional workflows described in established references emphasize manual processes that are labor intensive [1][2].

Automation may influence workforce dynamics. Therefore, the system shall be positioned as a tool that enhances human capability rather than replaces skilled technicians. Engineers and technicians will remain essential for interpretation, artistic voicing decisions, and final system optimization [4].

### Environmental Considerations

The environmental footprint of the system shall be evaluated throughout its lifecycle. Considerations include battery production, energy consumption, material sourcing, and electronic waste disposal.

To mitigate environmental impact:

The system shall prioritize energy-efficient components and optimized flight algorithms.

Rechargeable battery systems with extended cycle life shall be selected.

Modular construction shall allow component replacement instead of full-system disposal.

Responsible e-waste recycling procedures shall be followed.

Acoustic engineering references emphasize responsible SPL management and system efficiency, which indirectly supports reduced environmental and community noise impact [1][3].

### Regulatory Compliance and Operational Governance

Regulatory compliance is a fundamental engineering requirement. The system shall adhere to all applicable aviation, wireless communication, electrical safety, and occupational safety regulations.

#### FAA Compliance

If operated within the United States, the system shall comply with Federal Aviation Administration (FAA) regulations. For outdoor or partially open venues:

The system shall comply with 14 CFR Part 107 – Small Unmanned Aircraft Systems, including pilot certification, aircraft registration, and operational limitations [5].

The system shall comply with Remote Identification (Remote ID) requirements when applicable [5].

Airspace authorization shall be obtained when operating near controlled airspace.

The system shall avoid flight over uninvolved persons unless authorized under FAA operational categories.

Even when operating indoors, the project shall voluntarily align with FAA safety principles to maintain best-practice standards.

Indoor Venue Safety and OSHA Compliance

For indoor deployment:

Operation shall comply with Occupational Safety and Health Administration (OSHA) safety guidelines [6].

The drone shall not operate above occupied seating areas during public events.

Deployment shall occur only during controlled commissioning periods.

Venue management approval shall be obtained prior to operation.

#### FCC and RF Compliance

Wireless communication modules shall comply with FCC Part 15 regulations governing unlicensed radio frequency devices [7].

Transmitters shall operate within approved frequency bands and power limits.

The system shall avoid interference with wireless microphones, in-ear monitoring systems, and venue communication infrastructure.

RF coordination shall be conducted in spectrum-dense environments.

Battery and Electrical Safety

Lithium battery systems shall comply with recognized electrical safety standards:

Certified battery packs shall be used.

Integrated battery management systems (BMS) shall prevent overcharge, deep discharge, and thermal runaway.

Thermal monitoring shall be implemented.

### Acoustic Exposure Compliance

Acoustic excitation signals shall remain within safe exposure limits defined by OSHA occupational noise standards [6].

Personnel shall use hearing protection when required.

Measurement durations shall be controlled to prevent unsafe cumulative exposure.

Responsible sound-pressure management aligns with established acoustic engineering practices [1][3].

### Safety and Public Welfare

Protecting public safety is the highest ethical obligation of engineers. The system shall incorporate:

Redundant emergency shutdown mechanisms.

Controlled landing protocols.

Geofencing and firmware-enforced altitude limits.

Pre-flight risk assessment documentation.

Safety constraints shall override performance objectives. If safety risks are identified, deployment shall be paused until corrective measures are implemented.

Privacy and Data Responsibility

Although the system primarily collects acoustic data, onboard navigation sensors may capture incidental information. Therefore:

Data collection shall be limited strictly to acoustic and positional parameters necessary for analysis.

Visual recording shall be minimized or disabled unless required.

Wireless transmissions shall be encrypted.

Data retention shall comply with institutional and contractual policies.

Transfer-function measurement systems are intended to evaluate system performance, not personal information [4]. The proposed system shall maintain this narrow technical scope.

### Professional and Ethical Responsibility

Each team member shall uphold professional engineering ethics by:

Prioritizing safety, transparency, and reliability.

Accurately reporting performance data without exaggeration.

Clearly documenting system limitations.

Avoiding unsafe testing practices.

Sound-system engineering literature emphasizes disciplined measurement and verification [1][4]. The same rigor shall be applied to the aerial measurement platform.

### Societal and Accessibility Considerations

Improved sound consistency may enhance accessibility for individuals with hearing impairments by reducing spatial inconsistencies and excessive reverberation. More uniform coverage may also reduce the need for excessive volume levels. The relationship between spatial acoustic variation and listener perception is well established [2][3].

By striving for equitable acoustic performance throughout all seating areas—not only premium sections—the project supports fairness and inclusivity in audience experience.


The broader implications of this project extend beyond technical innovation. The autonomous acoustic-measurement system has the potential to improve industry workflows, enhance audience experience, and reduce commissioning time. However, these benefits must be balanced with strict adherence to safety, environmental responsibility, privacy protection, workforce considerations, and regulatory compliance.

By aligning system design with established principles in sound-system engineering [1]–[4] and federal regulatory standards [5]–[7], the project advances technological capability while upholding the paramount obligation of engineers: to protect public welfare and serve society responsibly.

[1] D. Davis and E. Patronis, Sound System Engineering, 4th ed. New York, NY, USA: Focal Press, 2013.

[2] G. Ballou, Ed., Handbook for Sound Engineers, 5th ed. New York, NY, USA: Focal Press, 2015.

[3] F. A. Everest and K. Pohlmann, Master Handbook of Acoustics, 6th ed. New York, NY, USA: McGraw-Hill, 2015.

[4] M. Lawrence, Between the Lines: Concepts in Sound System Design and Alignment. Petaluma, CA, USA: Rational Acoustics, 2016.

[5] Federal Aviation Administration, 14 CFR Part 107 – Small Unmanned Aircraft Systems, U.S. Department of Transportation, Washington, DC, USA.

[6] Occupational Safety and Health Administration, Occupational Noise Exposure Standard (29 CFR 1910.95), U.S. Department of Labor, Washington, DC, USA.

[7] Federal Communications Commission, Title 47 CFR Part 15 – Radio Frequency Devices, Washington, DC, USA.

## References

All sources used in the project proposal that are not common knowledge must be cited. Multiple references are required.


## Statement of Contributions

Each team member must contribute meaningfully to the project proposal. In this section, each team member is required to document their individual contributions to the report. One team member may not record another member's contributions on their behalf. By submitting, the team certifies that each member's statement of contributions is accurate.
