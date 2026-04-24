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

The buildable schematic for the Audio Capture, Signal Conditioning, and Transmission Subsystem is divided into three primary stages: the XLR input stage, the THAT1510 microphone preamplifier stage, and the THAT1646 balanced line driver output stage. These stages work together to receive the balanced microphone signal from the Xvive P1 phantom power supply, condition the signal for the Teensy 4.1 Audio Shield, and convert the processed output back into a balanced line-level signal for wireless transmission.

### Stage 1: XLR Input from Xvive P1

The first stage of the schematic receives the balanced microphone signal from the Xvive P1 phantom power supply. The beyerdynamic MM 1 measurement microphone requires phantom power, which is supplied externally by the Xvive P1. This subsystem does not generate phantom power internally, but must safely interface with a phantom-powered signal [4].

The XLR interface follows standard professional audio conventions:

- Pin 2: Hot (+)
- Pin 3: Cold (−)
- Pin 1: Shield / chassis ground

The shield is connected to chassis ground at the XLR connector only. This follows AES grounding practices and helps prevent ground loops and noise coupling into the signal path [1].

Each signal leg includes the following components:

- 47 Ω series resistor (RF/ESD protection)
- 10 µF coupling capacitor (DC blocking)
- 10 kΩ resistor to ground (bias reference)
- 220 pF capacitor to ground (high-frequency filtering)
- 100 Ω series resistor (input isolation)

The coupling capacitors block DC from the phantom power supply while allowing the AC audio signal to pass. The 10 kΩ resistors provide a DC return path and establish the input reference after the coupling stage. The 220 pF capacitors attenuate high-frequency noise, and the series resistors improve stability and protect the preamplifier inputs.

The differential signals are routed as follows:

- Hot (+) → THAT1510 Pin 3 (+IN)
- Cold (−) → THAT1510 Pin 2 (−IN)

This preserves the balanced signal structure and enables high common-mode noise rejection [5].

![Stage 1](Reports/DetailedDesign/Stage1Project.png)

---

### Stage 2: THAT1510 Microphone Preamplifier

The second stage uses the THAT1510 microphone preamplifier to convert the balanced microphone signal into a single-ended signal suitable for the Teensy Audio Shield. The THAT1510 is designed for low-noise audio applications and provides excellent common-mode rejection and gain control [5].

#### THAT1510 Pin Configuration

| Pin | Function | Connection |
|-----|----------|------------|
| 1 | RG1 | Gain resistor |
| 2 | −IN | Cold input (from XLR pin 3) |
| 3 | +IN | Hot input (from XLR pin 2) |
| 4 | V− | −15 V supply |
| 5 | REF | Analog ground |
| 6 | OUT | Audio output |
| 7 | V+ | +15 V supply |
| 8 | RG2 | Gain resistor |

A 10 kΩ resistor is placed between RG1 and RG2 (pins 1 and 8). This sets the gain to approximately +6 dB, which is appropriate for interfacing with the Teensy line-level input. The gain can be adjusted if testing shows the signal is too low or too high.

The power supply connections are:

- Pin 7 (V+) → +15 V
- Pin 4 (V−) → −15 V

Local decoupling capacitors (100 nF and 10 µF) are placed near the supply pins to reduce noise and stabilize operation. The REF pin (Pin 5) is tied to analog ground (AGND), which establishes the correct reference level for the output signal.

The audio output is taken exclusively from Pin 6. This output is conditioned before being sent to the Teensy Audio Shield:

- 1 kΩ series resistor → output isolation
- coupling capacitor → blocks DC
- 10 kΩ resistor to ground → establishes output reference
- optional attenuation network → prevents ADC clipping

The signal is then routed into the Teensy 4.1 Audio Shield line input, where analog-to-digital conversion and DSP processing occur [7], [8].

![Stage 2](Reports/Detailed Design/Stage2Project.png)

---

### Stage 3: THAT1646 Balanced Line Driver Output

After processing by the Teensy, the signal is single-ended and must be converted back to a balanced format for transmission. This stage uses the THAT1646 balanced line driver to perform this conversion [6].

The single-ended output from the Teensy Audio Shield is routed into the THAT1646 input through a resistor and optional filtering capacitor. The device generates two complementary outputs:

- OUT+ (non-inverting)
- OUT− (inverting)

Each output is passed through a 100 Ω resistor before reaching the XLR connector. These resistors improve stability and help isolate the driver from cable loading.

#### XLR Output Configuration

| Pin | Function |
|-----|----------|
| 1 | Shield / Ground |
| 2 | Hot (+) |
| 3 | Cold (−) |

The balanced output is connected to the Shure SLXD3 plug-on transmitter, which transmits the signal wirelessly to the SLXD4 receiver at FOH [9], [10].

Balanced transmission reduces susceptibility to electromagnetic interference and maintains signal integrity prior to wireless conversion [6].

![Stage 3](Reports/Detailed Design/Stage3Project.png)

---

### Schematic Design Summary

The complete signal path is:

beyerdynamic MM 1 → Xvive P1 → THAT1510 → Teensy 4.1 Audio Shield → THAT1646 → Shure SLXD3 → Shure SLXD4 → SMAART

This design ensures:

- Proper handling of phantom-powered signals [4]
- Low-noise differential amplification [5]
- Reliable ADC and DSP processing [7], [8]
- Balanced output for professional audio systems [6]
- Compatibility with SMAART measurement workflows [11]

The schematic provides a fully buildable and testable implementation that meets subsystem requirements while maintaining signal integrity and minimizing noise.

---

## References

[1] AES48-2005, Audio Engineering Society  
[2] Davis & Patronis, *Sound System Engineering*  
[3] beyerdynamic MM 1 Datasheet  
[4] Xvive P1 Documentation  
[5] THAT1510 Datasheet  
[6] THAT1646 Datasheet  
[7] PJRC Teensy 4.1 Documentation  
[8] PJRC Audio Shield Documentation  
[9] Shure SLXD3 Documentation  
[10] Shure SLXD4 Documentation  
[11] Rational Acoustics SMAART
