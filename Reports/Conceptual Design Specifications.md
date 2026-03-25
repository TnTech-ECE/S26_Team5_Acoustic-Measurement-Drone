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

## **External Components and Power Subsystem**

### **Subsystem Description**

The External Components and Power Subsystem shall store electrical energy, distribute power to the necessary hardware, and generate the thrust required for takeoff, hover, maneuvering, and landing. This subsystem includes the battery, power distribution path, electronic speed controller (ESC), motors, propellers, and associated external mounting and wiring interfaces.

For this design, the subsystem is based on a **6S 8000 mAh Li-ion battery** with a nominal voltage of **22.2 V**, providing **177.6 Wh** of energy. The propulsion system consists of **T-Motor Velox V3120 motors** paired with **APC 10x4.5 propellers**, driven by a **Hobbywing XRotor G2 65A 4-in-1 ESC**. The subsystem interfaces with the **Holybro Pixhawk 6C flight controller**, which requires regulated low-voltage input.

---

## **Subsystem Functional Breakdown**

### **Battery**
The battery subsection shall store and supply electrical energy to the system.

**Functions:**
- Provide primary DC power (22.2 V nominal)
- Support required mission duration
- Deliver high current to propulsion system
- Enable safe connection/disconnection

---

### **Power Distribution and Regulation**
This subsection shall distribute power from the battery to propulsion and avionics systems.

**Functions:**
- Route high-current power to ESC
- Provide low-voltage power to Pixhawk 6C and sensors
- Maintain common/ground across all subsystems
- Prevent voltage drops and unsafe current conditions

---

### **ESC**
The ESC subsection shall convert DC battery power into controlled three-phase motor drive signals.

**Functions:**
- Receive digital throttle commands from Pixhawk
- Drive four motors using three-phase outputs
- Support DShot300/600 communication
- Provide voltage, current, and RPM if necessary

---

### **Motors**
The motor section shall convert electrical power into mechanical rotation.

**Functions:**
- Receive three-phase signals from ESC
- Rotate propellers to generate thrust
- Provide lift and control torques
- Operate efficiently under hover conditions

---

### **Propellers**
The propeller subsection shall convert motor rotation into thrust.

**Functions:**
- Generate lift for flight
- Provide control authority for roll, pitch, and yaw
- Operate efficiently at moderate RPM
- Minimize vibration through proper balance

---

### **Sensors**
This subsection shall support externally mounted avionics components.

**Functions:**
- Provide mounting and power for optical flow sensor
- Enable stable hover by improved positioning feedback
- Route sensor wiring safely

---

## **Interfaces**

### **Interface with Flight Controller (Pixhawk 6C)**

- **Signal Types:**  
  - Regulated DC power  
  - Digital motor control 

- **Direction:**  
  - Power: External subsystem to Pixhawk  
  - Control: Pixhawk to ESC   

- **Protocols:**  
  - DShot300/600 (motor control)  
  - Regulated DC power input  

- **Data Sent:**  
  - Motor throttle commands  
  - Arm and disarm signals  

- **Data Received:**    
  - Status feedback  

---

### **Interface with Sensor**

- **Signal Types:** Power and sensor data  
- **Direction:**  
  - Power: External subsystem to sensors  
  - Data: Sensors to Pixhawk  

- **Protocols:**  
  - Sensor-specific interface (handled by avionics subsystem)

- **Data:**  
  - Optical flow motion data  

---

### **Interface with Acoustic Payload**

- **Signal Types:** Mechanical and optional power  
- **Direction:**  
  - External subsystem → payload: support and power  
  - Payload → external subsystem: mass and placement constraints  

---

### **Interface with User**

- **Signal Type:** Physical interaction  
- **Direction:** Bidirectional  

**User Actions:**
- Install and remove battery  
- Inspect motors and propellers  
- Connect and disconnect power  

---

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

