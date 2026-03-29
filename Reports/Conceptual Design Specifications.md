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

### **Hypothesized Solutions**

&nbsp; &nbsp; &nbsp; &nbsp;The literature suggests three primary solution paths for implementing acoustic measurement on a drone platform:

#### **1. Large Microphone Array with Beamforming**
- Uses many synchronized microphones
- Applies beamforming to determine sound direction and intensity
- Utilizes spectral subtraction or adaptive filtering to reduce drone noise

**Source basis:**  
- Urban UAV noise study (multi-mic array + FPGA + LMS filtering)  
- Drone-mounted phased array localization study (32-mic array + beamforming)

---

#### **2. Adaptive Noise Cancellation with Reference Microphones**
- Uses additional microphones positioned near motors/propellers
- Captures drone noise separately
- Applies adaptive filtering to remove it from the main signal

**Source basis:**  
- Urban traffic UAV study (reference microphones + Least Mean Square filtering)  
- Localization study (multi-band spectral subtraction)

---

#### **3. Single Measurement Microphone with Physical Isolation**
- Uses a single calibrated microphone for accurate SPL measurement
- Relies on physical separation below the drone instead of heavy DSP
- Minimizes noise at the source rather than removing it digitally

**Source basis:**
- Neither paper uses this approach directly, but both reveal limitations of onboard arrays and heavy DSP
- Motivated by the need for accurate point measurements, not just detection or mapping

---

## **Design Considerations**

&nbsp; &nbsp; &nbsp; &nbsp;Several key factors influence the selection of the final solution:

### **1. Drone Self-Noise**
- Both sources show that propeller and motor noise overlap with the desired signal
- Noise is broadband and difficult to remove using simple filters
- DSP-based solutions reduce noise but do not eliminate it completely

---

### **2. Measurement Accuracy vs. Complexity**
- Microphone arrays provide spatial awareness but require:
  - A complicated synchronizing process
  - high processing power
  - large hardware space
- For soundcheck applications, accurate point measurements are more valuable than directional estimation

---

### **3. System Weight and Power Consumption**
- Large arrays and FPGA systems increase:
  - weight
  - power draw
  - cost
- This reduces flight time and system practicality

---

### **4. Stability and Repeatability**
- Accurate acoustic measurements require:
  - stable hover
  - minimal vibration
- Sensor fusion (optical flow, IMU) improves repeatability

---

## **Selected Solution**

&nbsp; &nbsp; &nbsp; &nbsp;Based on the above considerations, the selected solution is a **hybrid approach combining physical noise mitigation with simplified signal processing**.

### **Chosen Architecture**
- **Single calibrated measurement microphone**, mounted below the drone via a tether or suspension system  
- **Standard drone propulsion system** optimized for stable hover   
- **Stable flight control using Pixhawk + optical flow sensor**

---

## **Justification for Selection**

### **1. Improved Measurement Accuracy**
- Physical separation reduces drone noise at the source
- Avoids distortion introduced by aggressive filtering
- Enables more accurate SPL measurements for tools like Smaart

---

### **2. Reduced System Complexity**
- Eliminates need for large microphone arrays and FPGA processing
- Reduces synchronization and calibration challenges
- Simplifies system integration

---

### **3. Lower Weight and Power Requirements**
- Fewer sensors and processing components
- Longer flight time
- Increased payload margin for microphone stabilization

---

### **4. Alignment with Project Goals**
- The project focuses on **soundcheck and acoustic measurement**, not source localization
- A single high-quality measurement point is more valuable than directional mapping
- The design prioritizes **accuracy, simplicity, and practicality**



## High-Level Solution

This section presents a comprehensive, high-level solution aimed at efficiently fulfilling all specified requirements and constraints. The solution is designed to maximize stakeholder goal attainment, adhere to established constraints, minimize risks, and optimize resource utilization. Please elaborate on how your design accomplishes these objectives.


### Hardware Block Diagram

![Alt text](https://github.com/TnTech-ECE/S26_Team5_Acoustic-Measurement-Drone/blob/Jackson's-Branch/Reports/Images/Hardware_Block_Diagram_Ext_Comp.png)

### Operational Flow Chart

Similar to a block diagram, the flow chart aims to specify the system, but from the user's point of view rather than illustrating the arrangement of each subsystem. It outlines the steps a user needs to perform to use the device and the screens/interfaces they will encounter. A diagram should be drawn to represent this process. Each step should be represented in the diagram to visually depict the sequence of actions and corresponding screens/interfaces the user will encounter while using the device.


## Atomic Subsystem Specifications

### Power and Propulsion Subsystem

#### 1. Subsystem Overview
The Power and Propulsion Subsystem is responsible for storing electrical energy, distributing power to propulsion components, and generating thrust for flight. It consists of the battery, electronic speed controllers (ESCs), brushless motors, and propellers. This subsystem directly determines total aircraft weight, thrust capability, and flight endurance.

---

#### 2. Component Functions and Specifications

##### 2.1 Battery
**Component:** iFlight Fullsend 6S 8000 mAh Li-Ion  
- **Voltage:** 22.2 V (nominal)  
- **Capacity:** 8000 mAh  
- **Energy:** 177.6 Wh  
- **Weight:** 840 g  

**Function:**  
The battery serves as the primary energy source for the entire aircraft. It supplies high-voltage DC power to the propulsion system and, through regulation, powers onboard electronics. It is the dominant factor in flight endurance and one of the largest contributors to total mass.

##### 2.2 Electronic Speed Controllers (ESCs)
**Component:** 4 × HobbyWing XRotor 40A 2–6S ESC  
- **Continuous Current:** 40 A  
- **Peak Current:** 60 A  
- **Voltage Range:** 2–6S  
- **Weight:** 32 g each (128 g total)  

**Function:**  
Each ESC regulates power delivered from the battery to a motor. It converts DC voltage into controlled three-phase signals to precisely adjust motor speed. ESCs enable real-time thrust control for stabilization and maneuvering.

##### 2.3 Motors
**Component:** 4 × Tarot 4112 300KV Brushless Motors  
- **KV Rating:** 300 KV  
- **Max Power:** 500 W per motor  
- **Weight:** 152 g each (608 g total)  

**Function:**  
Motors convert electrical energy into rotational motion. They generate torque to spin the propellers and produce lift. Low-KV motors are selected to improve efficiency and support stable, long-duration flight.

##### 2.4 Propellers
**Component:** APC 12×4.5 Multirotor Propellers (×4)  

**Function:**  
Propellers convert motor rotation into thrust. Their diameter and pitch determine lift efficiency, power consumption, and flight stability. A larger diameter, low-pitch configuration is selected to maximize hover efficiency for mapping operations.

---

#### 3. Weight Distribution

##### 3.1 Estimated Non-Propulsion Mass
- **Flight Controller:** 46.8 g  
- **HFlow Sensor:** 15.2 g  
- **LiDAR (RPLIDAR C1):** 110 g  
- **DSP/Teensy:** 50 g  
- **Frame:** 500 g  

**Subtotal:** **722 g**

##### 3.2 Propulsion Subsystem Mass
- **Battery:** 840 g  
- **Motors (×4):** 608 g  
- **ESCs (×4):** 128 g  
- **Propellers (×4):** ~60 g (estimated)  

**Subtotal:** **1636 g**

##### 3.3 Total Estimated Weight
**Total Mass ≈ 722 g + 1636 g = 2358 g (~2.36 kg)**

This estimate does not include wiring, mounting hardware, or additional sensors. A realistic final weight is expected to be:

**2.4–2.5 kg**

---

#### 4. Power Distribution

##### 4.1 System Architecture
Power is distributed from the battery into two primary paths:

**1. Propulsion Path (High Power)**  
- Battery → ESCs → Motors → Propellers  
- Dominates total power consumption  

**2. Avionics Path (Low Power)**  
- Battery → Voltage Regulation → Flight controller, sensors, DSP  

##### 4.2 Propulsion Power Capability
- **Max motor power:** 500 W × 4 = **2000 W total**  
- **ESC capacity:** 40 A per channel  
- **System voltage:** 22.2 V  

**Theoretical max current per motor:**  
500 W / 22.2 V ≈ **22.5 A**

This is within ESC limits, providing operating margin.

##### 4.3 Expected Operating Power
The system will not operate at maximum power continuously. During hover and mapping flight:

- **Typical thrust required per motor:** 550–650 g  
- **Expected operating power:** **400–800 W total system power**

##### 4.4 Battery Capability Check
- **Max continuous current:**  
  8 Ah × 17.5C = **140 A**

This is sufficient to support:
- Estimated hover current (~20–35 A total)
- Moderate maneuvering loads

---

#### 5. Summary
The propulsion subsystem contributes the majority of system mass and nearly all power consumption. The selected 6S battery provides sufficient energy density for extended flight time, while the motor and propeller combination prioritizes efficiency over peak thrust. The ESCs provide adequate current capacity and control authority, supporting stable and responsive operation.

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

