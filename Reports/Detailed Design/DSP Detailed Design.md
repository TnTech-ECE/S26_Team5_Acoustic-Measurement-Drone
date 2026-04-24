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
## Buildable Schematic

The buildable schematic for the DSP Subsystem is divided into three primary stages: the XLR input stage, the THAT1510 microphone preamplifier stage, and the THAT1646 balanced line driver output stage. These stages work together to receive the balanced microphone signal from the Xvive P1 phantom power supply, condition the signal for the Teensy 4.1 Audio Shield, and convert the processed output back into a balanced line-level signal for wireless transmission.

---

### Stage 1: XLR Input from Xvive P1

The first stage of the schematic receives the balanced microphone signal from the Xvive P1 phantom power supply. The beyerdynamic MM 1 measurement microphone requires phantom power, but this subsystem does not generate phantom power directly. Instead, the Xvive P1 provides the required phantom power externally and sends the balanced audio signal into the PCB through an XLR connector [4].

XLR pin 2 carries the hot signal, XLR pin 3 carries the cold signal, and XLR pin 1 is connected to shield/chassis ground. The shield is connected to chassis at the XLR connector only to reduce noise pickup and avoid routing shield current through the analog signal ground.

Each signal leg includes a 47 Ω series resistor, a 10 µF coupling capacitor, a 10 kΩ resistor to ground, a 220 pF capacitor to ground, and a 100 Ω series resistor before entering the THAT1510 preamplifier. The 10 µF capacitors block DC from the externally supplied phantom power while allowing the audio signal to pass. The 10 kΩ resistors provide a DC return path and establish the input reference after the coupling capacitors. The 220 pF capacitors provide high-frequency filtering, while the series resistors help provide input protection, RF isolation, and stability.

The hot signal path is routed to pin 3 of the THAT1510, which is the non-inverting input. The cold signal path is routed to pin 2 of the THAT1510, which is the inverting input. This maintains the balanced input relationship and allows the preamplifier to reject common-mode noise [1], [5].

![Stage 1 Schematic](Reports/Images/Stage1Project.png) 

---

### Stage 2: THAT1510 Microphone Preamplifier

The second stage uses a THAT1510 microphone preamplifier to convert the balanced microphone signal into a single-ended signal that can be sent to the Teensy Audio Shield. The THAT1510 is designed for low-noise microphone preamplifier applications and includes differential inputs, gain-setting pins, a reference pin, and a single-ended output [5].

The corrected THAT1510 pin connections used in this design are:

| Pin | Function | Connection |
|---|---|---|
| 1 | RG1 | One side of gain resistor |
| 2 | -IN | Cold input from XLR pin 3 |
| 3 | +IN | Hot input from XLR pin 2 |
| 4 | V- | -15 V supply rail |
| 5 | REF | Analog ground reference |
| 6 | OUT | Audio output to Teensy line input |
| 7 | V+ | +15 V supply rail |
| 8 | RG2 | Other side of gain resistor |

A 10 kΩ gain-setting resistor is placed between pins 1 and 8. This resistor sets the preamplifier gain to approximately +6 dB, which is a reasonable starting point because the signal is being sent into the Teensy Audio Shield line input rather than a microphone-level input. If testing shows that the signal level is too low, the gain resistor can be adjusted to increase gain. If the signal level is too high, the gain should remain low to avoid clipping the ADC input.

Pin 7 is connected only to the positive supply rail, and pin 4 is connected to the negative supply rail. These rails should be equal in magnitude and opposite in polarity, such as +15 V and -15 V. Local decoupling capacitors are placed near the power pins to reduce noise on the supply rails. Pin 5, the REF pin, is tied to analog ground so that the output signal is referenced properly.

The audio output from the THAT1510 comes only from pin 6. This output is routed through a 1 kΩ series resistor and a coupling capacitor before reaching the Teensy Audio Shield line input. A 10 kΩ resistor to ground provides an output reference, and an optional attenuation network can be used to reduce the signal level if ADC clipping occurs. The Teensy Audio Shield uses the SGTL5000 codec, which supports routing the line input into the ADC for audio processing [7], [8].

![Stage 2 Schematic](Reports/Images/Stage2Project.png) 

---

### Stage 3: THAT1646 Balanced Line Driver Output

After the signal is processed by the Teensy 4.1 and Audio Shield, the output is single-ended and must be converted back into a balanced signal before entering the wireless transmitter. This stage uses a THAT1646 balanced line driver to convert the processed single-ended signal into a balanced, low-impedance line-level output [6].

The single-ended output from the Teensy Audio Shield is routed into the input of the THAT1646 through an input resistor. The THAT1646 then produces two opposite-polarity outputs: a positive balanced output and a negative balanced output. These outputs are routed through 100 Ω output resistors before reaching the XLR output connector.

The balanced output connector follows the standard professional audio XLR pin arrangement:

| XLR Pin | Function |
|---|---|
| 1 | Shield / Ground |
| 2 | Hot / Positive Output |
| 3 | Cold / Negative Output |

This balanced output is then connected to the Shure SLXD3 plug-on transmitter. The balanced connection reduces noise pickup before transmission and maintains compatibility with professional audio equipment. At Front-of-House, the signal is received by the Shure SLXD4 receiver and routed into the audio interface or mixer for analysis in SMAART [9], [10], [11].

![Stage 3 Schematic](Reports/Images/Stage3Project.png)

---

### Schematic Design Summary

The buildable schematic satisfies the subsystem requirements by providing a complete signal path from the measurement microphone to the wireless transmission system. The XLR input stage safely accepts the signal from the externally powered phantom supply while blocking unwanted DC from entering the preamplifier. The THAT1510 stage provides low-noise balanced-to-single-ended conversion and adjustable gain. The Teensy 4.1 Audio Shield performs digital processing, and the THAT1646 output stage restores the signal to a balanced format for the wireless transmitter.

This design also addresses the major constraints of the subsystem. Balanced signaling helps reduce electromagnetic interference, coupling capacitors protect sensitive electronics from DC offsets, local decoupling capacitors improve supply stability, and the output attenuation option helps prevent ADC clipping. Overall, the schematic provides a practical and buildable implementation for acquiring, conditioning, processing, and transmitting acoustic measurement data.


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
