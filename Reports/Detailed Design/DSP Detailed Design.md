# Detailed Design

## Function of the Subsystem

&nbsp; &nbsp; &nbsp; &nbsp; The Digital Signal Processing (DSP) Subsystem is responsible for acquiring acoustic measurement data, preparing the signal for digital processing, and transmitting the processed signal to the Front-of-House (FOH) analysis system. This subsystem serves as the primary interface between the physical acoustic environment and the system’s digital processing and analysis components.

&nbsp; &nbsp; &nbsp; &nbsp; At the input stage, the subsystem utilizes a beyerdynamic MM 1 measurement microphone to capture sound pressure variations within the environment. The microphone signal is routed through an Xvive P1 phantom power supply, which provides the required excitation voltage for the condenser microphone while maintaining a balanced XLR signal output. The use of phantom-powered condenser microphones and balanced audio transmission follows established professional audio practices for minimizing noise and preserving signal integrity over cable runs [1], [2].

&nbsp; &nbsp; &nbsp; &nbsp; Following phantom power, the balanced analog signal is conditioned through a THAT1510 low-noise microphone preamplifier stage. This stage performs differential amplification, converts the balanced input signal to a single-ended output, and applies controlled gain based on an external gain-setting resistor. The THAT1510 is specifically designed for high-performance audio applications, offering low noise, high common-mode rejection ratio (CMRR), and precise gain control, making it suitable for measurement-grade signal acquisition [5]. Additional input protection and filtering circuitry ensures signal integrity by mitigating noise, rejecting common-mode interference, and protecting the preamplifier from transient conditions.

&nbsp; &nbsp; &nbsp; &nbsp; The conditioned analog signal is then delivered to a Teensy 4.1 microcontroller equipped with an audio shield, where onboard digital signal processing (DSP) is performed. The audio shield utilizes an integrated audio codec to perform analog-to-digital conversion and supports real-time audio processing at standard sampling rates, enabling the system to prepare signals for measurement and transmission [7], [8].

&nbsp; &nbsp; &nbsp; &nbsp; After DSP, the subsystem converts the processed single-ended signal back into a balanced line-level output using a THAT1646 balanced line driver. This conversion is necessary to maintain compatibility with professional audio interfaces and to preserve signal integrity during transmission, as balanced line-level signaling provides improved noise rejection in electrically noisy environments [2], [6].

&nbsp; &nbsp; &nbsp; &nbsp; Finally, the balanced signal is transmitted via a Shure SLXD3 plug-on transmitter and received by a Shure SLXD4 receiver at FOH. The received signal is then analyzed using industry-standard measurement software such as SMAART, which enables transfer function and impulse response analysis for sound system optimization [11].

&nbsp; &nbsp; &nbsp; &nbsp; Overall, this subsystem enables accurate, repeatable, and spatially distributed acoustic measurements by providing a complete signal path from sound acquisition to wireless transmission, while maintaining professional audio standards and ensuring compatibility with established measurement workflows.

## Specifications and Constraints

&nbsp; &nbsp; &nbsp; &nbsp; The DSP subsystem serves as the link between acoustic signal capture and system analysis, managing the signal path from the measurement microphone through onboard processing and finally to wireless transmission. Its design is driven by the need to preserve signal integrity while operating within the physical and electrical constraints of a drone-based platform. The following specifications and constraints define both the performance expectations and the practical limitations that shape the subsystem.


## Performance Specifications

### Microphone and Input Interface  
&nbsp; &nbsp; &nbsp; &nbsp; The subsystem is designed to accept a balanced XLR input from a phantom-powered condenser microphone, specifically the beyerdynamic MM 1, with phantom power supplied externally via the Xvive P1. This approach allows the system to leverage a measurement-grade microphone while maintaining a clean and noise-resistant signal path. The use of a balanced interface at the input stage is critical, as it minimizes susceptibility to electromagnetic interference and ensures compatibility with professional audio equipment.

### Analog Front-End  
&nbsp; &nbsp; &nbsp; &nbsp; After acquisition, the signal is conditioned using a THAT1510 microphone preamplifier. This stage converts the balanced input into a single-ended signal suitable for digital processing while also providing adjustable gain through an external resistor. The selection of this component is driven by its low-noise performance and strong common-mode rejection, both of which are essential for preserving measurement accuracy.

### Frequency Response  
&nbsp; &nbsp; &nbsp; &nbsp; The subsystem is designed to capture and process signals within a frequency range of 100 Hz to 10 kHz. This range focuses on the most relevant portion of the spectrum for live sound system tuning and intelligibility analysis, ensuring that the collected data aligns with practical measurement needs.

### Signal Accuracy  
&nbsp; &nbsp; &nbsp; &nbsp; To ensure meaningful measurement results, the subsystem must maintain a relative frequency response within ±3 dB when compared to a trusted reference system. This requirement ensures that any data analyzed in SMAART reflects the true behavior of the system under test.

### Digital Signal Processing  
&nbsp; &nbsp; &nbsp; &nbsp; Digital signal processing is performed using a Teensy 4.1 paired with an audio shield, operating at a minimum sampling rate of 44.1 kHz. This configuration provides sufficient resolution for real-time processing while remaining compact enough for integration into the drone platform.

### Output Interface  
&nbsp; &nbsp; &nbsp; &nbsp; Following processing, the signal is converted back to a balanced line-level output using a THAT1646 line driver. This allows the subsystem to maintain professional audio signal standards before transmitting the signal wirelessly through a Shure SLXD3 plug-on transmitter, preserving signal integrity up to the point of transmission.

### Latency  
&nbsp; &nbsp; &nbsp; &nbsp; To support accurate time-domain measurements, the subsystem must maintain a total end-to-end latency of 10 ms or less. This ensures that measurements such as impulse response and system alignment remain valid and are not degraded by excessive delay.

## System Constraints

### Balanced Signal Integrity  
&nbsp; &nbsp; &nbsp; &nbsp; The subsystem must maintain a balanced signal path from the microphone through the transmitter input. This constraint is essential for minimizing noise pickup and preserving signal quality in electrically noisy environments.

### Voltage and ADC Protection  
&nbsp; &nbsp; &nbsp; &nbsp; The output of the THAT1510 must remain within the allowable input range of the Teensy audio shield. This ensures that the ADC is protected from overvoltage conditions and prevents signal clipping that could compromise measurement accuracy.

### Drone Payload  
&nbsp; &nbsp; &nbsp; &nbsp; Because the subsystem is mounted on a drone, weight and size must be minimized. Excess payload directly impacts flight time, stability, and maneuverability, making efficient physical design a critical requirement.

### Power Consumption  
&nbsp; &nbsp; &nbsp; &nbsp; The subsystem must operate within the limited onboard power budget of the drone. Efficient component selection and power management are necessary to ensure reliable operation without reducing overall system endurance.

### Noise Environment  
&nbsp; &nbsp; &nbsp; &nbsp; The subsystem must function effectively in the presence of propeller and motor noise. Both acoustic and electrical noise sources must be considered, as they can degrade the quality of captured measurement data.

### Electromagnetic Interference (EMI)  
&nbsp; &nbsp; &nbsp; &nbsp; The design must minimize susceptibility to EMI from motors, ESCs, and wireless systems. Proper grounding, shielding, and PCB layout practices are required to prevent noise from coupling into the signal path.

### Standards-Based Design  
&nbsp; &nbsp; &nbsp; &nbsp; The subsystem is expected to follow professional audio standards, particularly those defined by the Audio Engineering Society. Adhering to these standards ensures compatibility with industry equipment and aligns the design with best practices.

### Safety and Ethical Considerations  
&nbsp; &nbsp; &nbsp; &nbsp; The system must safely handle phantom power and operating voltages to prevent equipment damage and ensure user safety. These considerations reflect responsible engineering practice and proper system design.

### Economic Constraints  
&nbsp; &nbsp; &nbsp; &nbsp; All components must be commercially available and fit within a reasonable student project budget. This ensures the design remains feasible to build and replicate.

## Overview of Proposed Solution

&nbsp; &nbsp; &nbsp; &nbsp; The proposed solution for the DSP Subsystem is a complete signal chain that captures acoustic data using a measurement-grade microphone, conditions the signal for digital processing, performs onboard DSP, and transmits the processed signal wirelessly to the Front-of-House (FOH) analysis system.

&nbsp; &nbsp; &nbsp; &nbsp; The subsystem begins with the beyerdynamic MM 1 measurement microphone, which provides accurate and consistent acoustic measurements across the required frequency range [3]. The microphone is powered using the Xvive P1 phantom power supply, allowing proper operation of the condenser microphone while maintaining a balanced XLR output [4]. This approach satisfies the requirement for professional audio compatibility and ensures that signal integrity is maintained from the point of acquisition [1].

&nbsp; &nbsp; &nbsp; &nbsp; The balanced analog signal is then routed into the THAT1510 microphone preamplifier stage. This stage converts the balanced signal into a single-ended signal while providing controlled gain and high common-mode noise rejection [5]. Input conditioning components, including coupling capacitors, bias resistors, and filtering capacitors, ensure that phantom power is blocked, noise is reduced, and the signal is properly referenced. This directly satisfies the subsystem requirements for signal accuracy, noise immunity, and compatibility with downstream electronics.

&nbsp; &nbsp; &nbsp; &nbsp; After analog conditioning, the signal is delivered to the Teensy 4.1 microcontroller with the Audio Shield, where analog-to-digital conversion and digital signal processing are performed [7], [8]. This stage enables real-time processing of the acoustic signal, including noise reduction and preparation for measurement analysis. The selected sampling rate of at least 44.1 kHz satisfies the subsystem specification for digital processing performance while maintaining compatibility with standard audio systems.

&nbsp; &nbsp; &nbsp; &nbsp; Following DSP, the signal is converted back into an analog format and routed into the THAT1646 balanced line driver. This stage restores the signal to a balanced line-level output, ensuring high noise immunity and compatibility with professional audio transmission systems [6]. The use of balanced signaling at this stage is critical for maintaining signal integrity prior to wireless transmission, especially in electrically noisy environments such as those present on a drone platform.

&nbsp; &nbsp; &nbsp; &nbsp; The balanced output is then transmitted using the Shure SLXD3 wireless transmitter and received at FOH using the Shure SLXD4 receiver [9], [10]. This wireless link eliminates the need for physical cabling and allows the drone to move freely throughout the measurement environment. The received signal is analyzed using SMAART software, enabling transfer function and impulse response measurements for system optimization [11].

This proposed solution fulfills the subsystem specifications and constraints in several key ways:

- **Signal Integrity:** Maintained through balanced transmission, proper grounding, and high-CMRR amplification [1], [5]  
- **Measurement Accuracy:** Achieved through the use of a calibrated measurement microphone and controlled gain stages [3], [5]  
- **Noise Mitigation:** Addressed through filtering, shielding, and balanced signaling across the entire signal chain  
- **Latency Requirements:** Met through real-time DSP processing and low-latency wireless transmission [11]  
- **Power and Size Constraints:** Satisfied through the use of compact, low-power components suitable for drone integration  
- **Standards Compliance:** Maintained through adherence to professional audio interface and grounding practices [1]  

&nbsp; &nbsp; &nbsp; &nbsp; Overall, the design provides a practical, buildable, and high-performance solution that integrates seamlessly with both the drone platform and professional audio measurement workflows.

## Interface with Other Subsystems

&nbsp; &nbsp; &nbsp; &nbsp; The Audio Capture, Signal Conditioning, and Transmission Subsystem has minimal direct interaction with other onboard subsystems. Its primary interfaces consist of electrical power input and mechanical integration with the drone platform. The subsystem operates largely independently in terms of signal processing and transmission.

---

### Interface with Power Subsystem

**Inputs:**
- +15 V supply rail  
- −15 V supply rail  
- Ground (AGND)

**Outputs:**
- None

**Description:**  
&nbsp; &nbsp; &nbsp; &nbsp; The subsystem requires regulated power from the drone’s power distribution system to operate the THAT1510 microphone preamplifier, THAT1646 line driver, and supporting analog circuitry. The Teensy 4.1 microcontroller and Audio Shield are also powered through this interface, either directly or through onboard regulation.

&nbsp; &nbsp; &nbsp; &nbsp; This interface is critical for proper subsystem operation, as stable and low-noise power rails are required to maintain signal integrity and prevent noise from coupling into the analog signal path. Decoupling capacitors and proper grounding techniques are used to minimize the impact of power supply noise.

---

### Interface with Mechanical Subsystem (Drone Frame)

**Inputs:**
- Physical mounting structure  
- Mechanical support  

**Outputs:**
- None

**Description:**  
&nbsp; &nbsp; &nbsp; &nbsp; The subsystem is mounted directly to the drone frame and relies on the mechanical subsystem for structural support. The design must ensure secure mounting to prevent movement or vibration that could introduce noise into the microphone signal or damage components.

&nbsp; &nbsp; &nbsp; &nbsp; Additionally, the subsystem must be compact and lightweight to meet the payload limitations of the drone. Proper placement is also important to reduce exposure to propeller wash and mechanical vibrations.

---

### External Signal Interface

**Inputs:**
- None (from other onboard subsystems)

**Outputs:**
- Balanced audio signal to wireless transmitter  
- Wireless transmission to FOH system  

**Description:**  
&nbsp; &nbsp; &nbsp; &nbsp; Although not interfacing with other onboard subsystems, the design outputs a balanced audio signal to the Shure SLXD3 wireless transmitter [9]. This signal is transmitted to the FOH system and analyzed using SMAART software [11]. This represents the primary functional output of the subsystem, but it exists outside of the drone’s internal subsystem architecture.

### Interface Summary

The subsystem interfaces are intentionally minimal and consist of:

- Electrical interface with the power subsystem  
- Mechanical interface with the drone frame  
- External signal output to the FOH measurement system  

&nbsp; &nbsp; &nbsp; &nbsp; This simplified interface structure allows the subsystem to operate independently while still integrating effectively within the overall system design.
## Buildable Schematic

&nbsp; &nbsp; &nbsp; &nbsp; The buildable schematic for the DSP Subsystem is divided into three primary stages: the XLR input stage, the THAT1510 microphone preamplifier stage, and the THAT1646 balanced line driver output stage. These stages work together to receive the balanced microphone signal from the Xvive P1 phantom power supply, condition the signal for the Teensy 4.1 Audio Shield, and convert the processed output back into a balanced line-level signal for wireless transmission.

### Stage 1: XLR Input from Xvive P1

&nbsp; &nbsp; &nbsp; &nbsp; The first stage of the schematic receives the balanced microphone signal from the Xvive P1 phantom power supply. The beyerdynamic MM 1 measurement microphone requires phantom power, which is supplied externally by the Xvive P1. This subsystem does not generate phantom power internally, but must safely interface with a phantom-powered signal [4].

The XLR interface follows standard professional audio conventions:

- Pin 2: Hot (+)
- Pin 3: Cold (−)
- Pin 1: Shield / chassis ground

&nbsp; &nbsp; &nbsp; &nbsp; The shield is connected to chassis ground at the XLR connector only. This follows AES grounding practices and helps prevent ground loops and noise coupling into the signal path [1].

Each signal leg includes the following components:

- 47 Ω series resistor (RF/ESD protection)
- 10 µF coupling capacitor (DC blocking)
- 10 kΩ resistor to ground (bias reference)
- 220 pF capacitor to ground (high-frequency filtering)
- 100 Ω series resistor (input isolation)

&nbsp; &nbsp; &nbsp; &nbsp; The coupling capacitors block DC from the phantom power supply while allowing the AC audio signal to pass. The 10 kΩ resistors provide a DC return path and establish the input reference after the coupling stage. The 220 pF capacitors attenuate high-frequency noise, and the series resistors improve stability and protect the preamplifier inputs.

The differential signals are routed as follows:

- Hot (+) → THAT1510 Pin 3 (+IN)
- Cold (−) → THAT1510 Pin 2 (−IN)

&nbsp; &nbsp; &nbsp; &nbsp; This preserves the balanced signal structure and enables high common-mode noise rejection [5].

![Stage 1](https://github.com/TnTech-ECE/S26_Team5_Acoustic-Measurement-Drone/blob/Rough_Draft_Project/Reports/Images/Stage1Project.png)

### Stage 2: THAT1510 Microphone Preamplifier

&nbsp; &nbsp; &nbsp; &nbsp; The second stage uses the THAT1510 microphone preamplifier to convert the balanced microphone signal into a single-ended signal suitable for the Teensy Audio Shield. The THAT1510 is designed for low-noise audio applications and provides excellent common-mode rejection and gain control [5].

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

&nbsp; &nbsp; &nbsp; &nbsp; A 10 kΩ resistor is placed between RG1 and RG2 (pins 1 and 8). This sets the gain to approximately +6 dB, which is appropriate for interfacing with the Teensy line-level input. The gain can be adjusted if testing shows the signal is too low or too high.

The power supply connections are:

- Pin 7 (V+) → +15 V
- Pin 4 (V−) → −15 V

&nbsp; &nbsp; &nbsp; &nbsp; Local decoupling capacitors (100 nF and 10 µF) are placed near the supply pins to reduce noise and stabilize operation. The REF pin (Pin 5) is tied to analog ground (AGND), which establishes the correct reference level for the output signal.

The audio output is taken exclusively from Pin 6. This output is conditioned before being sent to the Teensy Audio Shield:

- 1 kΩ series resistor → output isolation
- coupling capacitor → blocks DC
- 10 kΩ resistor to ground → establishes output reference
- optional attenuation network → prevents ADC clipping

&nbsp; &nbsp; &nbsp; &nbsp; The signal is then routed into the Teensy 4.1 Audio Shield line input, where analog-to-digital conversion and DSP processing occur [7], [8].

![Stage 2](https://github.com/TnTech-ECE/S26_Team5_Acoustic-Measurement-Drone/blob/Rough_Draft_Project/Reports/Images/Stage2Project.png)


### Stage 3: THAT1646 Balanced Line Driver Output

&nbsp; &nbsp; &nbsp; &nbsp; After processing by the Teensy, the signal is single-ended and must be converted back to a balanced format for transmission. This stage uses the THAT1646 balanced line driver to perform this conversion [6].

The single-ended output from the Teensy Audio Shield is routed into the THAT1646 input through a resistor and optional filtering capacitor. The device generates two complementary outputs:

- OUT+ (non-inverting)
- OUT− (inverting)

&nbsp; &nbsp; &nbsp; &nbsp; Each output is passed through a 100 Ω resistor before reaching the XLR connector. These resistors improve stability and help isolate the driver from cable loading.

#### XLR Output Configuration

| Pin | Function |
|-----|----------|
| 1 | Shield / Ground |
| 2 | Hot (+) |
| 3 | Cold (−) |

&nbsp; &nbsp; &nbsp; &nbsp; The balanced output is connected to the Shure SLXD3 plug-on transmitter, which transmits the signal wirelessly to the SLXD4 receiver at FOH [9], [10].

&nbsp; &nbsp; &nbsp; &nbsp; Balanced transmission reduces susceptibility to electromagnetic interference and maintains signal integrity prior to wireless conversion [6].

![Stage 3](https://github.com/TnTech-ECE/S26_Team5_Acoustic-Measurement-Drone/blob/Rough_Draft_Project/Reports/Images/Stage3Project.png)

### Schematic Design Summary

The complete signal path is:

beyerdynamic MM 1 → Xvive P1 → THAT1510 → Teensy 4.1 Audio Shield → THAT1646 → Shure SLXD3 → Shure SLXD4 → SMAART

This design ensures:

- Proper handling of phantom-powered signals [4]
- Low-noise differential amplification [5]
- Reliable ADC and DSP processing [7], [8]
- Balanced output for professional audio systems [6]
- Compatibility with SMAART measurement workflows [11]

&nbsp; &nbsp; &nbsp; &nbsp; Theses schematics provide a fully buildable and testable implementation that meets subsystem requirements while maintaining signal integrity and minimizing noise.

## Flowchart

![Teensy Flow Chart](https://github.com/TnTech-ECE/S26_Team5_Acoustic-Measurement-Drone/blob/Rough_Draft_Project/Reports/Images/DSPFlowChart.png)

## Bill of Materials (BOM)


&nbsp; &nbsp; &nbsp; &nbsp; The following Bill of Materials lists the components required to construct the Audio Capture, Signal Conditioning, and Transmission Subsystem. All components are selected based on availability through Amazon to simplify procurement and ensure accessibility. Major audio equipment, including the measurement microphone, wireless system, and XLR cabling, is provided by the audio program and is not included in the project cost.

---

| Ref Designator | Component Description | Manufacturer | Part Number | Qty | Unit Price | Total Price | Purchase Website |
|---|---|---|---|---:|---:|---:|---|
| U1 | Microphone preamplifier IC | THAT Corporation | THAT1510P08-U | 1 | $10.99 | $10.99 | https://www.amazon.com |
| U2 | Balanced line driver IC | THAT Corporation | THAT1646P08-U | 1 | $10.99 | $10.99 | https://www.amazon.com |
| U3 | Microcontroller | PJRC | Teensy 4.1 | 1 | $35.99 | $35.99 | https://www.amazon.com |
| U4 | Audio processing shield | PJRC | Teensy Audio Shield Rev D | 1 | $15.99 | $15.99 | https://www.amazon.com |
| PB1 | Perf board / prototyping board | Generic | Perf Board | 1 | $7.99 | $7.99 | https://www.amazon.com |
| W1 | 22 AWG solid core wire kit | Generic | Hookup Wire Kit | 1 | $9.99 | $9.99 | https://www.amazon.com |
| TB1 | Breadboard (testing) | Generic | Solderless Breadboard | 1 | $8.99 | $8.99 | https://www.amazon.com |
| R1, R2 | 47 Ω resistors | Generic | 1/4 W resistor | 2 | — | — | Included in kit |
| R3, R4, R8, R9, R10, RG | 10 kΩ resistors | Generic | 1/4 W resistor | 6 | — | — | Included in kit |
| R5, R6, Rout+, Rout− | 100 Ω resistors | Generic | 1/4 W resistor | 4 | — | — | Included in kit |
| R7, Rin | 1 kΩ resistors | Generic | 1/4 W resistor | 2 | — | — | Included in kit |
| C1, C2 | 10 µF capacitors | Generic | Electrolytic Capacitor | 2 | — | — | Included in kit |
| C3, C4 | 220 pF capacitors | Generic | Ceramic Capacitor | 2 | — | — | Included in kit |
| C5, C7, C1(out), C2(out) | 100 nF capacitors | Generic | Ceramic Capacitor | 4 | — | — | Included in kit |
| C6, C8 | 10 µF capacitors | Generic | Electrolytic Capacitor | 2 | — | — | Included in kit |
| C9 | 4.7 µF capacitor | Generic | Electrolytic Capacitor | 1 | — | — | Included in kit |
| Cin | 100 pF capacitor | Generic | Ceramic Capacitor | 1 | — | — | Included in kit |
| D1–D4 | Signal diodes | Generic | 1N4148 | 4 | $6.99 | $6.99 | https://www.amazon.com |
| KIT1 | Assorted resistor kit | Generic | Resistor Kit | 1 | $12.99 | $12.99 | https://www.amazon.com |
| KIT2 | Assorted capacitor kit | Generic | Capacitor Kit | 1 | $13.99 | $13.99 | https://www.amazon.com |

---

### Cost Summary

| Category | Cost |
|---|---:|
| Electronics (ICs + Teensy + Shield) | $73.96 |
| Passive Component Kits | $26.98 |
| Prototyping Materials | $26.97 |
| **Total Subsystem Cost** | **$127.91** |

---

### BOM Notes

- All components are sourced from Amazon to simplify procurement and reduce lead times.
- Passive components are purchased as kits and used to populate the required schematic components.
- The following components are provided by the audio program and are not included in the project cost:
  - beyerdynamic MM 1 measurement microphone  
  - Shure SLXD3 wireless transmitter  
  - Shure SLXD4 wireless receiver  
  - XLR cables and connectors  
- The subsystem is constructed on a perf board rather than a custom PCB.

## Analysis

&nbsp; &nbsp; &nbsp; &nbsp; The DSP Subsystem is designed to meet its intended function by maintaining a clean, professional audio signal path from the measurement microphone to the FOH analysis system. The design accomplishes this by using a measurement microphone, balanced audio connections, low-noise analog conditioning, onboard digital processing, and balanced wireless transmission.

&nbsp; &nbsp; &nbsp; &nbsp; The subsystem begins with the beyerdynamic MM 1 measurement microphone, which is appropriate for acoustic measurement because it is designed for flat and consistent response behavior [3]. Since the MM 1 requires phantom power, the Xvive P1 provides the necessary external phantom power before the signal enters the perf board [4]. This keeps the perf board simpler and safer because it does not need to generate +48 V internally. Instead, the perf board only needs to safely accept the balanced signal from the Xvive P1 and block the DC phantom component before the signal reaches the preamplifier.

&nbsp; &nbsp; &nbsp; &nbsp; The input stage supports this requirement by using coupling capacitors, bias resistors, RF filtering capacitors, and series protection resistors. The coupling capacitors block DC from the phantom-powered line while allowing the AC audio signal to pass. The 10 kΩ resistors provide a stable DC reference after the coupling capacitors, preventing the preamplifier inputs from floating. The small capacitors to ground help reduce high-frequency interference, while the series resistors provide input isolation and protection. This design directly supports the subsystem constraints related to signal integrity, ADC protection, EMI reduction, and safe handling of phantom-powered signals [1], [5].

&nbsp; &nbsp; &nbsp; &nbsp; The THAT1510 preamplifier stage is a strong choice because it is specifically intended for low-noise microphone preamplifier applications [5]. By accepting the hot and cold XLR signals into its differential inputs, the THAT1510 helps reject common-mode noise that may be picked up along the cable or introduced by the surrounding drone electronics. The use of an external gain resistor also allows the gain to be adjusted during testing. Starting with a 10 kΩ gain resistor provides a conservative gain setting, which helps prevent clipping when feeding the Teensy Audio Shield line input.

&nbsp; &nbsp; &nbsp; &nbsp; The Teensy 4.1 and Audio Shield provide the digital processing portion of the subsystem [7], [8]. Since the audio shield supports standard audio sample rates, including 44.1 kHz operation, the subsystem meets the sampling requirement for real-time audio processing. This allows the Teensy to apply filtering or other DSP methods before transmission. Processing the signal onboard is beneficial because it allows noise reduction to occur before the signal reaches the wireless transmitter.

&nbsp; &nbsp; &nbsp; &nbsp; After processing, the signal must be converted back into a balanced format. The THAT1646 line driver accomplishes this by converting the single-ended output from the Teensy Audio Shield into a balanced line-level signal [6]. This is important because the Shure SLXD3 transmitter expects a professional audio input, and balanced signaling improves noise rejection before wireless transmission [9]. The balanced output also supports the customer requirement for professional signal integrity throughout the system.

&nbsp; &nbsp; &nbsp; &nbsp; The wireless transmission stage allows the drone to move freely without requiring a physical audio cable to FOH. The Shure SLXD3 transmitter and SLXD4 receiver provide a practical wireless link for sending the processed signal to the FOH measurement system [9], [10]. At FOH, the received signal can be routed into an audio interface or mixer and analyzed in SMAART for transfer function and impulse response measurements [11].

Overall, the design meets the major subsystem specifications:

- **Frequency response:** The MM 1 microphone, analog front-end, Teensy Audio Shield, and balanced output chain are all intended for audio-band signal handling, supporting the required 100 Hz to 10 kHz measurement range [3], [5], [8].
- **Signal accuracy:** The use of a measurement microphone, balanced signal path, low-noise preamplifier, and controlled gain structure supports the ±3 dB relative accuracy goal.
- **Noise rejection:** Balanced XLR connections, differential amplification, RF filtering, proper grounding, and balanced line output all reduce susceptibility to noise and interference [1], [5], [6].
- **Digital processing:** The Teensy 4.1 with Audio Shield supports real-time DSP operation and standard audio sampling rates [7], [8].
- **Wireless compatibility:** The THAT1646 output stage provides a balanced signal suitable for the SLXD3 transmitter [6], [9].
- **Safety and protection:** The design avoids onboard phantom power generation, blocks DC from the input stage, and includes protection and attenuation features to reduce risk of hardware damage.

&nbsp; &nbsp; &nbsp; &nbsp; The main limitations of the design are related to drone noise, wireless transmission behavior, and gain staging. Drone propellers and motors may introduce acoustic and electrical noise that cannot be completely removed through filtering. Wireless transmission may also introduce latency, bandwidth limitations, or signal coloration. Additionally, the gain through the THAT1510 and the output level into the Teensy must be tested carefully to prevent ADC clipping.

&nbsp; &nbsp; &nbsp; &nbsp; These risks are manageable through testing and adjustment. The gain resistor can be changed if the signal level is too low or too high, the optional attenuation network can be used to protect the Teensy input, and filtering can be adjusted in software after real measurements are collected. Because the subsystem is modular, individual stages can be tested separately before full integration.

&nbsp; &nbsp; &nbsp; &nbsp; Based on this analysis, the proposed design should meet the intended function of acquiring, conditioning, processing, and transmitting acoustic measurement data. The design is technically practical, buildable, and compatible with professional audio measurement workflows.

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
[12] OpenAI, "ChatGPT," OpenAI, San Francisco, CA, USA. [Online]. Available: https://chat.openai.com.
