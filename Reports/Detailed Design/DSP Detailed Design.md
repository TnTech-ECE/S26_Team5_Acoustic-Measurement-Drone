# Detailed Design

## Function of the Subsystem

The Digital Signal Processing (DSP) Subsystem is responsible for acquiring acoustic measurement data, preparing the signal for digital processing, and transmitting the processed signal to the Front-of-House (FOH) analysis system. This subsystem serves as the primary interface between the physical acoustic environment and the system’s digital processing and analysis components.

At the input stage, the subsystem utilizes a beyerdynamic MM 1 measurement microphone to capture sound pressure variations within the environment. The microphone signal is routed through an Xvive P1 phantom power supply, which provides the required excitation voltage for the condenser microphone while maintaining a balanced XLR signal output. The use of phantom-powered condenser microphones and balanced audio transmission follows established professional audio practices for minimizing noise and preserving signal integrity over cable runs [1], [2].

Following phantom power, the balanced analog signal is conditioned through a THAT1510 low-noise microphone preamplifier stage. This stage performs differential amplification, converts the balanced input signal to a single-ended output, and applies controlled gain based on an external gain-setting resistor. The THAT1510 is specifically designed for high-performance audio applications, offering low noise, high common-mode rejection ratio (CMRR), and precise gain control, making it suitable for measurement-grade signal acquisition [5]. Additional input protection and filtering circuitry ensures signal integrity by mitigating noise, rejecting common-mode interference, and protecting the preamplifier from transient conditions.

The conditioned analog signal is then delivered to a Teensy 4.1 microcontroller equipped with an audio shield, where onboard digital signal processing (DSP) is performed. The audio shield utilizes an integrated audio codec to perform analog-to-digital conversion and supports real-time audio processing at standard sampling rates, enabling the system to prepare signals for measurement and transmission [7], [8].

After DSP, the subsystem converts the processed single-ended signal back into a balanced line-level output using a THAT1646 balanced line driver. This conversion is necessary to maintain compatibility with professional audio interfaces and to preserve signal integrity during transmission, as balanced line-level signaling provides improved noise rejection in electrically noisy environments [2], [6].

Finally, the balanced signal is transmitted via a Shure SLXD3 plug-on transmitter and received by a Shure SLXD4 receiver at FOH. The received signal is then analyzed using industry-standard measurement software such as SMAART, which enables transfer function and impulse response analysis for sound system optimization [11].

Overall, this subsystem enables accurate, repeatable, and spatially distributed acoustic measurements by providing a complete signal path from sound acquisition to wireless transmission, while maintaining professional audio standards and ensuring compatibility with established measurement workflows.

---

## Specifications and Constraints

The DSP Subsystem is responsible for acquiring acoustic signals, conditioning them for digital processing, and transmitting them wirelessly for analysis. The following specifications and constraints define the subsystem design, with each requirement justified based on system needs, physical limitations, standards, and practical considerations.

---

# Specifications

---

## Microphone and Input Interface Specification

**Specification:**  
The subsystem shall support a balanced XLR input from a phantom-powered condenser microphone, specifically the beyerdynamic MM 1 measurement microphone, powered externally via the Xvive P1 portable phantom power supply.

**Rationale:**  
The MM 1 provides measurement-grade accuracy and flat frequency response, making it suitable for acoustic analysis. The use of a balanced XLR interface minimizes noise pickup and ensures compatibility with professional audio systems [1], [3], [4].

---

## Analog Front-End Specification

**Specification:**  
The subsystem shall utilize a THAT1510 microphone preamplifier to convert the balanced microphone signal to a single-ended output and provide adjustable gain through an external resistor between RG1 and RG2.

**Rationale:**  
The THAT1510 offers low noise, high common-mode rejection ratio (CMRR), and precise gain control, which are critical for preserving signal integrity in measurement applications [5].

---

## Frequency Response Specification

**Specification:**  
The subsystem shall accurately capture and transmit signals within a frequency range of **100 Hz to 10 kHz**.

**Rationale:**  
This range captures the most critical frequency content for live sound system tuning and intelligibility analysis [2].

---

## Signal Accuracy Specification

**Specification:**  
The subsystem shall maintain a relative frequency response within **±3 dB** compared to a reference measurement system.

**Rationale:**  
Ensures that the subsystem produces reliable measurement data when analyzed using SMAART [11].

---

## Digital Signal Processing Specification

**Specification:**  
The subsystem shall utilize a Teensy 4.1 with an audio shield to perform real-time digital signal processing at a minimum sampling rate of **44.1 kHz**.

**Rationale:**  
This configuration supports standard audio processing requirements and allows sufficient resolution for acoustic measurements [7], [8].

---

## Output Interface Specification

**Specification:**  
The subsystem shall convert the processed signal back to a balanced line-level output using a THAT1646 line driver and transmit the signal via a Shure SLXD3 plug-on wireless transmitter.

**Rationale:**  
Balanced line-level output ensures compatibility with professional audio equipment and maintains signal integrity prior to wireless transmission [6], [9].

---

## Latency Specification

**Specification:**  
The subsystem shall maintain a total signal latency of **≤ 10 ms** from input to receiver output.

**Rationale:**  
Low latency is required for accurate time-domain measurements such as impulse response and system alignment [11].

---

# Constraints

---

## Balanced Signal Integrity Constraint

**Constraint:**  
The subsystem shall maintain balanced signal transmission from the microphone through the wireless transmitter input.

**Rationale:**  
Balanced signaling reduces susceptibility to electromagnetic interference and preserves signal quality in noisy environments [1].

---

## Voltage and ADC Protection Constraint

**Constraint:**  
The subsystem shall ensure that the output of the THAT1510 does not exceed the input voltage limits of the Teensy Audio Shield.

**Rationale:**  
Exceeding ADC input limits can result in signal clipping or permanent hardware damage [7].

---

## Drone Payload Constraint

**Constraint:**  
The subsystem shall be designed to minimize weight and physical size to ensure compatibility with the drone platform.

**Rationale:**  
Increased payload negatively impacts flight time, maneuverability, and system stability.

---

## Power Consumption Constraint

**Constraint:**  
The subsystem shall operate within the available onboard power budget of the drone system.

**Rationale:**  
Limited battery capacity restricts the allowable power consumption of onboard electronics.

---

## Noise Environment Constraint

**Constraint:**  
The subsystem shall operate effectively in the presence of propeller and motor noise.

**Rationale:**  
Drone operation introduces both acoustic and electrical noise that can degrade measurement accuracy.

---

## Electromagnetic Interference (EMI) Constraint

**Constraint:**  
The subsystem shall minimize susceptibility to EMI from motors, ESCs, and wireless transmission systems.

**Rationale:**  
Improper shielding or grounding can introduce noise and distort measurement signals.

---

## Standards-Based Design Constraint

**Constraint:**  
The subsystem shall adhere to professional audio interface standards as defined by the Audio Engineering Society (AES).

**Rationale:**  
Compliance with established standards ensures interoperability, reliability, and alignment with industry best practices [1].

---

## Safety and Ethical Constraint

**Constraint:**  
The subsystem shall ensure safe handling of phantom power and operating voltages.

**Rationale:**  
Proper electrical design prevents equipment damage and ensures user safety, aligning with ethical engineering practices.

---

## Economic Constraint

**Constraint:**  
The subsystem shall utilize commercially available components within a reasonable student project budget.

**Rationale:**  
Ensures feasibility of construction and accessibility for replication or further development.

---

## Manufacturability Constraint

**Constraint:**  
The subsystem shall be designed such that it can be implemented on a manufacturable PCB and pass standard design rule checks (DRC).

**Rationale:**  
Ensures the design can be fabricated reliably and assembled without errors.

---

![Alt text](Images/Stage1Project.png)


# References

[1] Audio Engineering Society (AES), *AES48-2005: Grounding and EMC Practices for Audio Equipment*

[2] Davis, D., Patronis, E., *Sound System Engineering*, 4th ed., Focal Press, 2013

[3] beyerdynamic MM 1 Datasheet

[4] Xvive P1 Product Documentation

[5] THAT1510 Datasheet, THAT Corporation

[6] THAT1646 Datasheet, THAT Corporation

[7] Teensy 4.1 Documentation, PJRC

[8] Teensy Audio Shield Documentation, PJRC

[9] Shure SLXD3 Product Documentation

[10] Shure SLXD4 Product Documentation

[11] SMAART, Rational Acoustics
