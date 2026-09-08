# Drone Controller Detailed Design

This document explains the design of the operator controller subsystem for a custom acoustics measurement drone. The goal is to make the following parts of the controller design clear:

* How the controller subsystem integrates within the broader drone solution
* The constraints and specifications relevant to the controller subsystem
* The rationale behind each crucial design decision
* The intended architecture and operational role of the controller subsystem


## Function of the Subsystem

The controller subsystem is the main interface for the custom acoustics measurement drone. Its role is to allow the operator to view basic system information, issue pilot inputs, select supervisory operating modes, and command the broader system in a controlled and deliberate manner [1][3][16]. In the overall system, the controller is not the airborne flight computer; instead, it is the ground side operator interface that gathers user input through joysticks, buttons, switches, and a graphical display, then communicates operator intent and supervisory information to the drone avionics and autonomy subsystems.

For the acoustics measurement mission, the controller supports the field operator by combining four functions into one device:

1. **Input collection:** It captures joystick commands, button states, menu selections, and supervisory mode changes.
2. **Feedback presentation:** It displays system and mission information on the selected Waveshare 5 inch HDMI capacitive touchscreen [3].
3. **Mission supervision:** It provides a dedicated physical M.A.S. (Manual/Autonomous Selector) so that mode selection remains an explicit operator action rather than a hidden software only function [7].
4. **Drone communication:** It uses the selected Holybro SiK Telemetry Radio V3 915 MHz kit to provide the controller to drone MAVLink/serial communications path used for telemetry and supervisory command exchange [16].

The controller works as a dedicated operator console tailored to the acoustics measurement drone rather than as a generic consumer game controller. Its design emphasizes readability, portability, self contained battery operation, explicit supervisory controls, and a dedicated wireless communications interface.


## Specifications and Constraints

The controller subsystem has electrical, mechanical, human factors, communications, and project integration constraints.

### Electrical and Interface Constraints

* The core computing platform is a Raspberry Pi Zero 2 WH supplied as part of the selected Vemico development kit. The Zero 2 W platform provides 5 V DC power input, mini HDMI video output, USB OTG connectivity, and a 40 pin GPIO header suitable for SPI and digital input expansion [1][2].
* The selected display is the Waveshare 5inch HDMI LCD (H), an 800×480 capacitive touch HDMI display. Touch data is carried over USB [3].
* Because the Raspberry Pi Zero 2 W does not provide general purpose analog input channels, the analog joystick axes must be interfaced through the selected MCP3008 external SPI ADC [1][10].
* The subsystem uses a perfboard based helper board rather than a custom fabricated controller PCB in the current design phase. The selected helper board is a PATIKIL 2 in × 2 in double sided perfboard, which constrains wiring density, component placement, and connector routing [4].
* The selected portable power source is the Nitecore POCKET 5, a 5,000 mAh USB C power bank rated for 15 W total output. The controller therefore uses a USB based internal power architecture rather than a custom lithium cell charging and protection circuit [5].
* The Pi power path uses the selected Adafruit 3879 USB C to Micro B cable, while the display power path uses the selected Adafruit 4056 panel mount USB C socket to Micro B cable [12][14].
* The selected controller to drone communications hardware is the Holybro SiK Telemetry Radio V3 kit, 915 MHz, 100 mW. The kit contains two radio modules and antennas and is used as the MAVLink/serial link between the handheld controller and the aircraft [16].
* The controller uses a 64 GB SanDisk Ultra microSD card for the Raspberry Pi operating system, controller software, and local storage [19].
* The display touch interface and SiK radio both require USB data connectivity. The selected **Zero4U 4 Port USB Hub without Pogo Pins (Adafruit Product 4115)** expands the Raspberry Pi Zero 2 WH USB OTG connection so both devices can operate concurrently. The Zero4U is a USB 2.0/1.1 compatible 5 V hub and may be bus powered or self powered [27].

### Mechanical and Packaging Constraints

* The controller must fit within a custom enclosure sized around the 5 inch display, two Adafruit analog thumb joysticks, momentary buttons, bumper buttons, supervisory switches, the Raspberry Pi assembly, the Zero4U USB hub, the telemetry radio, and the portable battery [2][3][5][9][16][27].
* The mechanical arrangement must accommodate the selected Pi power cable, HDMI video cable, display power cable, display touch data cable, Zero4U USB hub, 20 cm Micro USB to Mini USB OTG hub upstream cable, telemetry radio data cable, internal soldered wiring, and active cooling assembly without interfering with the operator grip or control layout [11][12][13][14][15][17][18].
* The selected 2 in × 2 in PATIKIL perfboard requires the helper board to function primarily as a compact ADC and signal routing board rather than a large all in one interconnect backplane [4].
* The enclosure must provide adequate placement and clearance for the Holybro telemetry radio module and its antenna so that the antenna is not unnecessarily shielded by internal electronics or the operator's hands [16].
* The controller enclosure remains a custom 3D printed component purchased and accounted for through the Frame subsystem.

### Human Factors Constraints

* The controller must present an intuitive layout for two hand operation.
* Distinct functions such as menu navigation, selection, supervisory mode switching, and power switching must remain physically distinguishable to reduce operator error.
* The design includes a dedicated E Switch SPDT M.A.S. (Manual/Autonomous Selector) rather than burying that state change in a software only menu, improving mode awareness during field use [7].
* The E Switch rocker power switch shall remain physically separate from the M.A.S. (Manual/Autonomous Selector) so that powering the controller and changing supervisory mode cannot be confused [8].

### Standards, Ethics, and Cost Constraints

* The subsystem must be documented in a manner appropriate for engineering review, with traceable references, explicit subsystem boundaries, and clear integration descriptions.
* The design should prioritize operator awareness, deliberate mode transitions, maintainable wiring practices, and clear radio link status to reduce avoidable integration and operation errors.
* The project should use accessible commercially available prototyping hardware where reasonable. The final controller BOM therefore combines the selected Zero 2 WH development kit, PATIKIL perfboard, Nitecore power bank, tactile switches, commercially available joysticks, Holybro telemetry radio kit, Zero4U USB hub, and standard interconnect cables rather than requiring a custom PCB or custom battery management assembly [2][4][5][6][9][16].
* The final material selection and prices are defined by the project BOM and should be treated as the purchasing baseline for this controller revision.

These constraints may be revised by the team if additional testing, enclosure changes, USB integration results, or system level flight testing require modification.

The released controller revision also follows the software state definitions, communications interface, performance requirements, pin assignment, and verification plan documented later in this report. Changes to those items shall be revision controlled so that software, wiring, and verification records remain consistent.


## Overview of Proposed Solution

The proposed solution is a custom handheld drone controller built around a Raspberry Pi Zero 2 WH development kit with a dedicated perfboard helper board that organizes analog inputs, digital inputs, and local signal routing [2][4].

The controller is built around the Raspberry Pi Zero 2 WH, chosen because the Zero 2 W platform combines low mass, low volume, graphical display support, USB and HDMI interfaces, and a large GPIO header in a compact package [1][2]. The selected Vemico kit also includes a mini HDMI adapter and USB OTG cable, reducing the number of separate adapter components required for initial integration [2]. The Pi interfaces to the selected Waveshare 5inch HDMI LCD (H) through the included mini HDMI adapter and the selected 1.5 ft HDMI to HDMI cable [3][13].

To read joystick positions, the design uses an external analog to digital conversion stage on the PATIKIL helper board. The helper board contains the Microchip MCP3008 ADC, local decoupling, and signal routing for the analog joystick axes [4][10]. The two selected Adafruit Model 512 joystick modules each provide two analog axes and a select button function; the select button function is intentionally reserved for future implementation to reduce the first revision input map [9].

Digital inputs use the selected TWTADE 6 × 6 mm 2 pin tactile switch assortment for the face buttons, D pad directions, menu/select buttons, and bumper buttons [6]. A panel mounted E Switch RA1113112R rocker switch is used as the main power switch, while an E Switch 100SP1T1B4M2QE SPDT toggle switch is used as the M.A.S. (Manual/Autonomous Selector) [7][8].

The controller power system uses a commercial USB C battery bank. The selected Nitecore POCKET 5 provides 5,000 mAh capacity and a 15 W total output rating, allowing the controller to remain portable while avoiding a custom lithium cell charging/protection design [5]. The Pi is powered through the selected Adafruit 3879 USB C to Micro B cable, and the display power path uses the Adafruit 4056 panel mount USB C socket to Micro B cable [12][14]. This arrangement also supports enclosure level access to the display power connection.

Because the POCKET 5 does not provide the Raspberry Pi with direct state of charge telemetry, the controller application shall display an **estimated battery percentage**. The estimate shall be derived from prototype characterized controller power consumption, elapsed operating time, and known operating states such as display brightness and radio activity. Multiple representative discharge tests shall be used to calibrate the estimate. The user interface shall label the value as an estimate so it is not confused with direct fuel gauge telemetry.

The display uses the selected HDMI cable for video and the selected CVILUX USB touch cable for touch data [13][15]. The Raspberry Pi USB OTG data connection feeds the selected Zero4U 4 port USB hub. The display touch data connection and the controller side Holybro SiK radio are connected as separate downstream USB devices through this hub, allowing Linux to enumerate and manage them independently [27].

Communication with the aircraft uses the Holybro SiK Telemetry Radio V3 kit operating at 915 MHz and 100 mW. One radio module is integrated with the handheld controller and the other is installed on the aircraft. The controller side radio is connected to the Raspberry Pi using the selected Adafruit 3610 Micro USB to Micro USB cable, while the aircraft side radio provides the corresponding MAVLink/serial interface to the drone avionics [16][18].

Cooling is handled by the selected TECKEEN aluminum heatsink and cooling fan assembly mounted to the Raspberry Pi Zero 2 W, supplemented by enclosure level airflow and mechanical accommodation in the CAD model [17]. Local software and operating system storage are provided by the selected SanDisk 64 GB Ultra microSD card [19].

Overall, the proposed solution fulfills the subsystem requirements by combining:

* local analog input acquisition through the MCP3008,
* local digital input acquisition through physical switches,
* a 5 inch capacitive touch operator display,
* portable USB C battery operation,
* an explicit M.A.S. (Manual/Autonomous Selector),
* a dedicated 915 MHz SiK telemetry/MAVLink link to the aircraft,
* active thermal management,
* and removable microSD based software storage,

within a compact custom handheld controller tailored to the acoustics measurement drone mission.


## Interface with Other Subsystems

The controller connects to the rest of the system through electrical, informational, wireless, and human command pathways.

### Interface with the Operator

**Inputs to the controller from the operator:**
* Two joystick axes per Adafruit Model 512 joystick module, for four analog axes total in the current revision [9]
 * Right joystick left/right: lateral X axis command
 * Right joystick up/down: forward/backward Y axis command
 * Left joystick up/down: vertical Z axis / altitude command
 * Left joystick left/right: yaw / turning command
* Four face buttons using the selected TWTADE tactile switches [6]
* Four directional pad buttons using the selected TWTADE tactile switches [6]
* Two bumper buttons using the selected TWTADE tactile switches [6]
* One select button [6]
* One menu button [6]
* One E Switch RA1113112R main power switch [8]
* One E Switch 100SP1T1B4M2QE M.A.S. (Manual/Autonomous Selector) [7]
* Capacitive touch display interaction through the Waveshare display [3]

**Outputs from the controller to the operator:**
* Visual system, mission, and mode feedback on the 5 inch Waveshare display [3]
* Controller software status and warning information on the screen
* Power state information provided through the selected power architecture and software/UI implementation as supported by final integration
* Wireless link and telemetry status derived from the Holybro SiK communication path [16]

### Interface with the Drone Avionics and Mission Subsystems

At the system level, the controller provides operator intent and supervisory mode state to the broader drone architecture. The selected Holybro SiK Telemetry Radio V3 kit supplies the 915 MHz wireless MAVLink/serial communications path between the handheld controller and the aircraft [16].

Outbound controller data may include:

* operator axis commands,
* discrete operator button states,
* menu and mode selection commands,
* explicit manual/autonomous supervisory mode state,
* mission start, pause, resume, return, land, or abort requests as supported by the final flight software,
* and controller side acknowledgments or configuration messages.

Inbound information returned to the controller for presentation may include:

* mission state,
* aircraft status,
* operator prompts,
* payload or survey progress,
* battery and health information,
* warning or fault messages,
* and communications link state.

The aircraft side SiK radio interfaces with the Pixhawk 6C Mini and its compatible ArduPilot flight control software. The final message definitions, MAVLink handling, serial port configuration, and fail safe behavior are defined by the flight controller and software integration work described in the Internal Components Detailed Design [16][20][21].


### Software Behavior and Communication Interface

The controller software reads the operator controls, sends manual or supervisory commands, receives drone telemetry, updates the display, and keeps track of the current operating mode. The controller does not stabilize the aircraft itself. That job stays with the Pixhawk 6C Mini and ArduPilot.

#### Controller Software States

The controller software shall implement, at minimum, the following logical states:

* **BOOT:** Raspberry Pi operating system and controller application are starting.
* **SELF TEST:** Local input devices, ADC communication, display/touch interface, storage, and telemetry radio availability are checked.
* **DISCONNECTED:** The controller application is running but a valid aircraft MAVLink connection has not been established.
* **CONNECTED / STANDBY:** A valid aircraft link exists and telemetry is being received, but no active manual control command stream is being sent.
* **AUTONOMOUS SUPERVISION:** The aircraft is executing its autonomous mission while the controller monitors mission state and accepts supervisory requests.
* **MANUAL OVERRIDE:** The operator has selected manual authority and the controller sends manual control input messages at the required update rate.
* **FAULT / LINK LOSS:** The controller has detected a local hardware fault, stale telemetry, or lost aircraft communication and shall clearly notify the operator.

A transition into **MANUAL OVERRIDE** shall take priority over normal autonomous supervision commands. A transition back to autonomous operation shall require both a valid operator request and confirmation from the aircraft side software that autonomous operation can be safely resumed. The controller shall not automatically restore autonomous operation after a communications interruption, software restart, or ambiguous selector switch state.

The physical **M.A.S.** is the authoritative local request for Manual versus Autonomous operation. Moving the M.A.S. to Manual shall immediately request suspension of autonomous mission authority and transition the controller toward manual override. Moving the M.A.S. to Autonomous shall request autonomous operation or resumption only after the aircraft side software confirms that the transition is permitted.

If the established SiK link is lost, the aircraft side Pixhawk/ArduPilot configuration shall use an onboard link loss failsafe that first commands a brief position hold and then transitions to a controlled landing. The initial hold duration shall be treated as a configurable prototype test parameter rather than a permanently fixed constant.

#### Controller Side Software Services

The controller application shall be implemented primarily in **C++**. A Qt based interface may be used for the touchscreen UI, while MAVSDK C++ and/or direct MAVLink C/C++ message handling may be used for aircraft communications where appropriate.

The software implementation shall contain the following logical services, whether implemented as separate processes, threads, or software modules:

1. **Input service:** Samples joystick channels through the MCP3008, debounces digital controls, reads the M.A.S. (Manual/Autonomous Selector), applies calibration and configurable deadzones, and generates normalized operator inputs using the released stick to axis mapping.
2. **Communication service:** Opens and maintains the USB serial connection to the Holybro SiK telemetry radio, encodes outbound MAVLink messages, validates inbound MAVLink messages, and tracks connection health.
3. **State manager:** Determines the current controller mode and prevents conflicting manual/autonomous commands.
4. **User interface service:** Updates mission, telemetry, warning, link, and controller state information on the Waveshare display.
5. **Logging service:** Records controller startup, mode changes, link state changes, software faults, and operator supervisory events to the microSD card.
6. **Configuration service:** Stores joystick calibration, deadband values, software version, MAVLink system/component identifiers, and other nonvolatile controller settings.
7. **Battery estimation service:** Computes and displays an estimated controller state of charge using prototype characterized power consumption, elapsed operating time, and calibrated operating state assumptions.

The controller application shall automatically start after the Raspberry Pi completes booting. If the application terminates unexpectedly, the system shall record the fault when possible and shall not assume that autonomous control has been safely restored.

#### MAVLink and SiK Radio Interface

The controller to aircraft communications interface shall use the selected Holybro SiK Telemetry Radio V3 pair operating in the 915 MHz band. The controller side radio shall connect to the Raspberry Pi as a USB serial device, and the aircraft side radio shall connect to a telemetry/UART interface on the Pixhawk 6C Mini. The radio provides a transparent bidirectional serial link intended for MAVLink communication [16][23].

The baseline serial interface data rate shall be **57.6 kbps**, which is the Holybro SiK Radio V3 default. The baseline over air data rate shall be **64 kbps**, also the documented default for the selected radio family [23]. Any change to these radio parameters shall be documented in the integration configuration record so that the two radios remain identically configured.

The controller shall use **MAVLink 2** framing unless project integration testing identifies a compatibility requirement that prevents its use. The controller shall use a fixed, documented ground station system ID and component ID so that the Pixhawk can distinguish authorized controller messages from other MAVLink sources [24][25].

**Primary inbound aircraft data shall include, when available:**

* MAVLink heartbeat and vehicle mode/state
* aircraft battery and system health status
* mission/waypoint progress
* position or local position status needed for operator awareness
* flight controller warning and status text
* rangefinder, optical flow, or localization health information when exposed by the aircraft telemetry configuration
* RC/manual input echo or control state information when useful for verification

**Primary outbound controller data shall include:**

* controller heartbeat / ground station presence
* manual joystick control data while manual override is active
* supervisory mission start request
* return to launch / return request
* land request
* mode or autonomy transition requests defined by the aircraft side mission software
* acknowledgments or configuration messages required by the final controller application

For manual control transport, the build shall use the same selected SiK radio pair used for telemetry and supervisory communication. The baseline software design shall use the MAVLink **MANUAL_CONTROL** message or another ArduPilot supported MAVLink pilot input mechanism selected during integration. ArduPilot documents MANUAL_CONTROL as a normalized pilot input interface and also warns that pilot input over telemetry can be affected by link latency and available bandwidth [24]. The single SiK architecture is therefore a deliberate design choice for this build, and its manual control latency and responsiveness shall be extensively characterized during prototype testing before the manual override function is considered qualified.

Mission level commands shall use standard ArduPilot/MAVLink command mechanisms where supported. Return to launch and land functions shall use supported MAVLink/ArduPilot commands rather than custom packet formats [26]. Any project specific autonomy resume behavior that cannot be represented by a standard command shall be documented as an application level interface between the controller software and the onboard mission software.

The controller shall reject malformed MAVLink messages and shall ignore messages that do not match the expected aircraft system identity once the vehicle connection has been established.

### Performance Requirements

The following performance requirements define measurable controller responsiveness and communications behavior. Values are subsystem requirements for verification and may be tightened after prototype testing.

| Requirement ID | Performance Requirement | Acceptance Target |
|---|---|---|
| PERF-01 | Controller application startup | Main UI shall be available within **30 s** of controller power-up under the final software image. |
| PERF-02 | Joystick sampling | All four active analog joystick axes shall be sampled at **50 Hz minimum** while the controller application is active. |
| PERF-03 | Digital-input response | A valid button or supervisory-switch change shall be recognized by the controller software within **20 ms** after debounce processing. |
| PERF-04 | Manual-control message rate | While manual override is active, pilot-control messages shall be transmitted at **20 Hz minimum**. |
| PERF-05 | Local UI response | The display shall reflect a local mode-selector or kill/manual-override state change within **100 ms**. |
| PERF-06 | High-rate telemetry presentation | Flight mode, attitude/position summary, and link state shall update on the UI at **5 Hz minimum** when the aircraft supplies those data at that rate. |
| PERF-07 | Low-rate telemetry presentation | Battery, mission progress, and health summaries shall update on the UI at **2 Hz minimum** when those data are available. |
| PERF-08 | Stale-link indication | The controller shall present a visible stale-link warning within **2 s** of receiving no valid MAVLink traffic from an established aircraft connection. |
| PERF-09 | Lost-link indication | The controller shall declare the aircraft link disconnected within **5 s** of receiving no valid MAVLink traffic from an established connection. |
| PERF-10 | Manual-control end-to-end latency | In bench/SITL validation, median operator-input-to-aircraft-command latency shall be **≤150 ms**, and the 95th-percentile latency shall be **≤250 ms** under the final telemetry configuration. |
| PERF-11 | Manual-control qualification gate | The single-SiK manual-control architecture shall pass PERF-10 consistently during repeated prototype tests before manual override is considered qualified. If it does not, the design shall be re-evaluated before aircraft-level use. |
| PERF-12 | Power stability | The controller shall operate at maximum display brightness with the radio active, cooling fan running, and all input devices connected without Raspberry Pi undervoltage resets or peripheral disconnects. |
| PERF-13 | Continuous operation | The final assembled controller shall complete a **60 min** bench endurance run without software crash, thermal shutdown, or loss of USB peripherals. |
| PERF-14 | Input calibration stability | After calibration, each centered joystick axis shall remain within the configured neutral deadband for at least **60 s** without operator input. Final deadzone values shall be established through repeated prototype testing. |
| PERF-15 | USB concurrent operation | The Zero4U hub shall support simultaneous touchscreen input and SiK serial communication for at least **60 min** without repeated USB enumeration errors or unintended disconnects. |
| PERF-16 | Battery estimate update | The displayed estimated controller battery percentage shall update at least every **5 s** and shall be calibrated using representative full-discharge prototype tests. |
| PERF-17 | Link-loss response | A confirmed SiK link loss shall initiate the configured aircraft-side brief-hold-then-controlled-land failsafe without requiring a new command from the ground controller. |

These targets separate **local controller response** from **wireless manual control response**. The buttons and display should react quickly, while the SiK link still needs to prove that it is responsive enough for manual control during prototype testing.


### Internal Electrical Interfaces

**Raspberry Pi Zero 2 WH ↔ Zero4U USB Hub ↔ Display / SiK Radio**
* The Pi USB OTG host connection feeds the Zero4U hub upstream Mini USB interface through the selected **20 cm Micro USB male to Mini USB 5 pin male OTG cable** [1][27][28].
* The display touch controller is one downstream USB device on the hub and uses the selected CVILUX USB touch cable [3][15][27].
* The Holybro SiK ground radio is a separate downstream USB serial device on the same hub [16][18][27].
* Linux shall enumerate the touchscreen input device and SiK USB serial device independently; the C++ application shall identify the SiK serial interface by persistent device identity rather than assuming a fixed `/dev/ttyUSB0` number.
* Video output remains independent of the USB hub: it originates from the Pi mini HDMI interface, passes through the Vemico mini HDMI adapter, and reaches the display through the selected HDMI cable [1][2][13].
* Display power remains supplied through the selected Adafruit 4056 panel mount USB C socket to Micro B cable [5][14].

**Raspberry Pi Zero 2 WH ↔ Helper Board**
* 3.3 V distribution provides the ADC and low voltage logic reference [1][10].
* Ground reference is shared across the Pi, helper board, and control devices.
* The Pi communicates with the MCP3008 using SPI [10].
* GPIO lines are used for digital buttons and supervisory switches.

**Helper Board ↔ Controls**
* Analog voltage signals from the two Adafruit joystick modules are routed to MCP3008 ADC channels [9][10].
* Digital tactile switch states are routed as GPIO inputs using the final pull up/pull down and debouncing implementation [6].
* The supervisory toggle and power switch states are routed according to the final enclosure and wiring implementation [7][8].

**Zero4U USB Hub ↔ Holybro SiK Telemetry Radio**
* The controller side radio is connected as a downstream USB serial device through the Zero4U hub [16][27].
* The selected Adafruit 3610 cable remains assigned to the radio side Micro USB data connection [18].
* The wireless signal is the single 915 MHz SiK link used for MAVLink telemetry, supervisory commands, and intended manual control messages [16][23].
* Simultaneous radio communication and touchscreen touch input shall be verified under the USB concurrency requirements.

**Nitecore POCKET 5 ↔ Controller Electronics**
* The power bank supplies the controller's portable USB C power source [5].
* The Pi receives power through the Adafruit 3879 USB C to Micro B cable [12].
* The display receives power through the selected Adafruit 4056 panel mount USB C to Micro B path [14].
* Power distribution and cable routing shall be verified against the power bank's 15 W total output limitation before final enclosure integration [5].
* The Zero4U hub may be operated in bus powered or self powered mode; the final controller power arrangement shall be selected during bench integration so the combined hub/peripheral load remains within the Nitecore power budget [5][27].



### Pin Assignment and Detailed Electrical Behavior

The following pin map is the baseline controller wiring assignment for the Raspberry Pi Zero 2 WH and MCP3008 helper board. The assignment reserves I2C and primary UART pins for future expansion and avoids sharing the SPI0 pins used by the ADC.

#### Raspberry Pi GPIO Assignment

| Function | BCM GPIO | Physical Pin | Direction | Electrical Behavior |
|---|---:|---:|---|---|
| MCP3008 MOSI / DIN | GPIO10 | 19 | Output | SPI0 MOSI, 3.3 V logic |
| MCP3008 MISO / DOUT | GPIO9 | 21 | Input | SPI0 MISO, 3.3 V logic |
| MCP3008 SCLK | GPIO11 | 23 | Output | SPI0 clock, 3.3 V logic |
| MCP3008 CS/SHDN | GPIO8 | 24 | Output | SPI0 CE0, active-low chip select |
| Face Button 1 | GPIO4 | 7 | Input | Active-low; internal pull-up enabled |
| Face Button 2 | GPIO17 | 11 | Input | Active-low; internal pull-up enabled |
| Face Button 3 | GPIO27 | 13 | Input | Active-low; internal pull-up enabled |
| Face Button 4 | GPIO22 | 15 | Input | Active-low; internal pull-up enabled |
| D-Pad Up | GPIO23 | 16 | Input | Active-low; internal pull-up enabled |
| D-Pad Down | GPIO24 | 18 | Input | Active-low; internal pull-up enabled |
| D-Pad Left | GPIO25 | 22 | Input | Active-low; internal pull-up enabled |
| D-Pad Right | GPIO5 | 29 | Input | Active-low; internal pull-up enabled |
| Left Bumper | GPIO6 | 31 | Input | Active-low; internal pull-up enabled |
| Right Bumper | GPIO12 | 32 | Input | Active-low; internal pull-up enabled |
| Select Button | GPIO13 | 33 | Input | Active-low; internal pull-up enabled |
| Menu Button | GPIO19 | 35 | Input | Active-low; internal pull-up enabled |
| Manual selector contact | GPIO16 | 36 | Input | Active-low; one throw of SPDT selector |
| Autonomous selector contact | GPIO26 | 37 | Input | Active-low; opposite throw of SPDT selector |
| Future joystick-click input 1 | GPIO20 | 38 | Input | Reserved; active-low when implemented |
| Future joystick-click input 2 | GPIO21 | 40 | Input | Reserved; active-low when implemented |
| I2C SDA | GPIO2 | 3 | Bidirectional | Reserved for future controller expansion |
| I2C SCL | GPIO3 | 5 | Output/Bidirectional | Reserved for future controller expansion |
| UART TX | GPIO14 | 8 | Output | Reserved; SiK radio uses USB in baseline design |
| UART RX | GPIO15 | 10 | Input | Reserved; SiK radio uses USB in baseline design |

All GPIO connected momentary switches shall connect the assigned GPIO input to the controller ground when pressed. Software shall enable an internal pull up and shall interpret a logic low state as an asserted control. A nominal **20 ms software debounce interval** shall be applied to momentary buttons.

The manual/autonomous SPDT switch shall use its common terminal as ground and its two switched terminals as separate active low inputs. This provides two independent state signals instead of relying on one GPIO voltage level. The controller software shall interpret:
* Manual contact low / Autonomous contact high = **MANUAL requested**
* Manual contact high / Autonomous contact low = **AUTONOMOUS requested**
* Both high or both low = **invalid/ambiguous selector state**

An invalid or ambiguous selector state shall never request autonomous operation. The controller shall present a warning and remain in, or request, the safer non autonomous supervisory state until a valid selector condition is restored.

The RA1113112R main power switch is a **power path device**, not a GPIO input. It shall not be included in the software input map unless a separate auxiliary sensing circuit is intentionally added in a later revision.

#### MCP3008 Analog Channel Assignment

| MCP3008 Channel | Assigned Signal | Direction | Expected Electrical Range | Notes |
|---|---|---|---|---|
| CH0 | Left Joystick X | Analog input | 0–3.3 V nominal | Yaw / turning command |
| CH1 | Left Joystick Y | Analog input | 0–3.3 V nominal | Vertical Z-axis / altitude command |
| CH2 | Right Joystick X | Analog input | 0–3.3 V nominal | Lateral X-axis movement command |
| CH3 | Right Joystick Y | Analog input | 0–3.3 V nominal | Forward/backward Y-axis movement command |
| CH4 | Future Analog Trigger 1 | Analog input | Reserved | Not populated in current revision |
| CH5 | Future Analog Trigger 2 | Analog input | Reserved | Not populated in current revision |
| CH6 | Spare Analog Input | Analog input | Reserved | Future expansion |
| CH7 | Spare Analog Input | Analog input | Reserved | Future expansion |

The MCP3008 VDD and VREF pins shall be supplied from the Raspberry Pi **3.3 V** rail so that ADC input voltages remain within the Pi compatible logic and reference range. Joystick modules shall be powered from the same 3.3 V reference unless component level testing demonstrates a different required supply arrangement. No joystick or ADC signal connected to the Pi subsystem shall exceed 3.3 V.

A 0.1 µF ceramic decoupling capacitor shall be installed locally between MCP3008 VDD and ground. Analog ground and digital ground shall share the controller common ground while physical routing should minimize long parallel runs between sensitive analog joystick signals and the telemetry radio/power wiring.

Joystick deadzones shall be configurable software parameters and shall be finalized only after repeated prototype characterization of center drift and operator response. At controller startup, each active joystick axis shall be checked for a plausible ADC reading before manual control output is enabled. Open circuit, short to ground, or full scale stuck inputs shall generate a controller input fault instead of being interpreted as a valid pilot command.



## 3D Model of Custom Mechanical Components

![ControllerOverview](https://github.com/TnTech-ECE/S26_Team5_Acoustic-Measurement-Drone/blob/520dd80f169e465b70f5a01bcdc116141ace6367/Reports/Images/ControllerOverview.png)

![ControllerFrontView](https://github.com/TnTech-ECE/S26_Team5_Acoustic-Measurement-Drone/blob/520dd80f169e465b70f5a01bcdc116141ace6367/Reports/Images/ControllerFrontview.png)
Spaces in the controller for buttons and joysticks. and 2 spaces beside the screen clearence for future implementation.

![ControllerTopView2](https://github.com/TnTech-ECE/S26_Team5_Acoustic-Measurement-Drone/blob/Rough_Draft_Project/Reports/Images/ControllerTopView2.png)
Spaces for bumper buttons and future implementations for triggers. Spacing for buttons that are attached to the screen, as well as spacing for the power switch (right) and manual control switch (Left). Removable back panel (Blue) for easy internal access. Space on the top revealing the Transmitter for antenna implementation.

![ControllerInternalView2](https://github.com/TnTech-ECE/S26_Team5_Acoustic-Measurement-Drone/blob/Rough_Draft_Project/Reports/Images/ControllerInternalView2.png)
Spaces reserved for battery (Yellow), Microcomputer (orange), Cooling System (Blue), USB Hub (Black), Radio Transmitter (Gray), and Helper Board (Green).

The CAD package for the controller enclosure demonstrates ergonomic control placement, internal component clearances, cable routing space, airflow vents for the active cooler, and the spatial relationship between the perfboard helper board and operator controls.


## Buildable Schematic

![Controller Wiring Diagram2](https://github.com/TnTech-ECE/S26_Team5_Acoustic-Measurement-Drone/blob/Rough_Draft_Project/Reports/Images/Controller%20Wiring%20Diagram2.png)

The wiring diagram shows the Raspberry Pi Zero 2 W GPIO header connections, the MCP3008 ADC mounted on the perfboard helper board, the 3.3 V and ground rails, and the 0.1 µF decoupling capacitor. It also shows the final analog joystick channel assignments, SPI connections between the Pi and MCP3008, digital wiring for the face buttons, D pad, menu and select buttons, bumpers, and the M.A.S. The diagram also identifies the reserved channels for future analog triggers and joystick click inputs and shows the common ground arrangement used throughout the controller.

![Drone Controller Cable Diagram](https://github.com/TnTech-ECE/S26_Team5_Acoustic-Measurement-Drone/blob/Rough_Draft_Project/Reports/Images/Drone%20Controller%20Cable%20Diagram.png)

The drone controller cabling diagram shows the main power, data, and signal connections between the controller components. It includes the Nitecore POCKET 5 battery, Raspberry Pi Zero 2 WH, Waveshare 5 inch HDMI touchscreen, Zero4U USB hub, Holybro SiK Telemetry Radio V3, MCP3008 perfboard helper board, joysticks, controller buttons, and cooling system. The diagram identifies the specific USB, HDMI, OTG, power, GPIO, SPI, and analog connections between these devices. It also shows the wireless SiK communication link between the controller and the air side radio connected to the Pixhawk 6C Mini.

## Printed Circuit Board Layout

None Used for this Subsystem


## Flowchart

![Controller Flowchart2](https://github.com/TnTech-ECE/S26_Team5_Acoustic-Measurement-Drone/blob/Rough_Draft_Project/Reports/Images/Controller%20Flowchart2.png)



## Verification and Validation Plan

Testing will start with software checks and unpowered wiring checks, then move to powered bench testing and simulation or hardware in the loop testing. Aircraft testing will come later, after the controller passes the bench and simulation checks and the team is ready to follow the project safety plan.

| Test ID | Requirement / Feature Verified | Verification Method | Acceptance Criteria |
|---|---|---|---|
| V-01 | BOM and assembly configuration | Inspection | Installed parts and cable assignments match the released BOM and wiring documentation. |
| V-02 | GPIO wiring | Continuity / inspection with power removed | Every switch reaches only its assigned GPIO and common ground; no unintended shorts are present. |
| V-03 | MCP3008 SPI interface | Bench software test | ADC is detected and all four active axes return stable 10-bit readings. |
| V-04 | Joystick electrical range | Bench measurement / software logging | No active joystick signal exceeds the 0–3.3 V ADC range; full stick travel produces a usable monotonic ADC range. |
| V-05 | Joystick calibration | Software test | Center, minimum, and maximum calibration values are stored and restored after reboot; neutral values meet PERF-14. |
| V-06 | Digital buttons | Software input test | Each button produces only its assigned logical event and meets PERF-03. |
| V-07 | M.A.S. (Manual/Autonomous Selector) | Software input/state test | Manual, autonomous, and invalid selector states are detected correctly; Manual immediately requests autonomy override and invalid state never commands autonomous operation. |
| V-08 | Display and touch | UI functional test | Screen renders correctly and touch events are recognized; local state changes meet PERF-05. |
| V-09 | Zero4U USB concurrency | 60-minute bench integration test | The selected 20 cm OTG upstream cable provides a stable Pi-host-to-Zero4U connection, and display touch plus SiK radio operate simultaneously without repeated enumeration errors, USB disconnect/reconnect events, loss of touch input, or unintended loss of serial communication. |
| V-10 | Controller startup | Timed boot test | Controller reaches usable main UI within PERF-01. |
| V-11 | MAVLink connection | SiK radio bench test | Controller establishes a valid MAVLink session with the target flight-controller/SITL endpoint and correctly identifies the expected system. |
| V-12 | Telemetry reception | Bench/SITL test | Required mode, battery, mission, status, and link data are decoded and displayed at the required update rates when supplied by the source. |
| V-13 | Supervisory commands | SITL/HIL command test | Mission-start, return, land, and supported mode requests are encoded, transmitted, acknowledged, and logged without using undocumented custom packet formats. |
| V-14 | Manual-control mapping | SITL/HIL test | Right-stick X/Y commands lateral/forward-backward motion, left-stick Y commands vertical Z motion, and left-stick X commands yaw, with correct sign and no unintended channel motion. |
| V-15 | Manual-control latency | Timestamped bench/SITL measurement | PERF-10 is met for the final controller software and radio configuration. |
| V-16 | Single-SiK manual-control qualification | Repeated bench/SITL latency testing and design review | The selected single SiK pair consistently satisfies PERF-10 before the manual-override function is considered qualified; otherwise the design is re-evaluated before aircraft-level use. |
| V-17 | Link degradation / loss | Bench/SITL disconnection simulation | Stale and disconnected states are presented within PERF-08 and PERF-09, and confirmed loss invokes the configured aircraft-side brief-hold-then-controlled-land failsafe. |
| V-18 | Application restart/fault | Software fault-injection test | Restart or application failure does not automatically request autonomous operation and the fault is visible/logged after recovery when possible. |
| V-19 | Logging | Software test | Startup, mode changes, link changes, warnings, and software faults are recorded with timestamps on the microSD card. |
| V-20 | Maximum electrical load | Bench power test | Pi, display at maximum brightness, fan, radio, ADC, and controls operate simultaneously without brownout/reset, satisfying PERF-12. |
| V-21 | Endurance / thermal behavior | 60-minute enclosed bench run | No software crash, thermal shutdown, undervoltage event, or loss of required peripherals occurs, satisfying PERF-13. |
| V-22 | Power-cycle recovery | Repeated bench power-cycle test | Controller returns to a known startup state after repeated normal power cycles and retains required calibration/configuration data. |
| V-23 | Battery-percentage estimation | Representative discharge characterization | Estimated percentage is compared against elapsed runtime and full-discharge reference data; model coefficients are adjusted from repeated prototype tests and the UI clearly labels the value as estimated. |
| V-24 | Requirements traceability | Design review | Every controller "shall" requirement has at least one test, inspection, analysis, or demonstration method assigned before final design review. |

The verification record shall document the controller software version, Raspberry Pi image version, ArduPilot version used for simulation/integration, radio settings, test date, pass/fail result, and any deviations. Failed requirements shall be corrected or formally accepted with documented rationale before the controller is considered ready for system level integration.

For safety, early verification of manual control behavior shall be performed using software simulation, hardware in the loop, or propeller disabled bench setups rather than relying on initial free flight testing. Final aircraft level validation is outside the scope of this controller document and shall follow the broader project's approved system test and safety plan.



## BOM

The current controller subsystem has the following confirmed high level component set. 

| Item | Component | Manufacturer / Source | Part / Model | Qty. | Price | Notes |
|---|---|---|---|---|---|---|
| U1 | Single-board computer / development kit | Vemico / Amazon | Raspberry Pi Zero 2 WH dev kit, B0G5PLTL79 | 1 | $120 | Zero 2 WH kit; includes mini-HDMI adapter and USB OTG cable |
| DS1 | Display | Waveshare / Newegg | 5inch HDMI LCD (H), 800×480 capacitive | 1 | $80 | 5 in HDMI touchscreen; touch interface over USB |
| BRD1 | Helper board | PATIKIL / Amazon | 2 in × 2 in double-sided perfboard | 1 | $4.25 | Carries the MCP3008 and signal routing |
| PWR1 | Portable power source | Nitecore / Battery Junction | POCKET 5 | 1 | $40 | 5,000 mAh USB-C power bank, 15 W total output, IPX7; built-in USB-C cable |
| SW1-SW12 | Momentary tactile switches | TWTADE / Amazon | 6 × 6 mm 2-pin tactile pushbutton assortment, B085SWHFMK | 1 | $9 | Used for face buttons, D-pad, menu/select, and bumpers |
| SW13 | Supervisory mode selector | E-Switch / DigiKey | 100SP1T1B4M2QE | 1 | $2.66 | SPDT on-on toggle switch used for manual/autonomous supervisory selection |
| SW14 | Main power switch | E-Switch / DigiKey | RA1113112R | 1 | $0.65 | SPST rocker main power switch |
| J1-J2 | Analog joystick modules | Adafruit / DigiKey | 512 | 2 | $12 | Analog 2-axis thumb joysticks with select buttons; select function reserved for future implementation |
| U2 | ADC | Microchip Technology / DigiKey | MCP3008-I/P | 1 | $3.12 | 8-channel 10-bit SPI ADC for joystick acquisition |
| C1 | Decoupling capacitor | Generic | 0.1 µF ceramic capacitor | 1 | Already Owned | Local ADC supply decoupling |
| W1 | Hook-up wire | Adafruit / DigiKey | 1311 | 1 | $16 | 22 AWG solid-core wire set for internal wiring between helper board and controls |
| CBL1 | Pi power interconnect | Adafruit | 3879 | 1 | $2.95 | USB-C to Micro-B cable, 1 ft; power bank to Pi power input |
| CBL2 | Video interconnect | jojobnj / Amazon | HDMI to HDMI cable, 1.5 ft | 1 | $3.35 | Pi-to-display video connection using the adapter included with the Pi kit |
| CBL3 | Display power interconnect | Adafruit / DigiKey | 4056 | 1 | $5 | Panel-mount USB-C socket to Micro-B plug, 30 cm; display power feed through enclosure wall |
| CBL4 | Display touch-data interconnect | CVILUX USA / DigiKey | DH-20M50057 | 1 | $1.20 | USB 2.0 Type-A male to Micro-B male cable for display touch data |
| RAD1 | Telemetry radio kit | Holybro | SiK Telemetry Radio V3, 915 MHz, 100 mW, part 17013 | 1 | $59 | Two-module radio kit with antennas for MAVLink / serial communication between controller and drone |
| HUB1 | Internal USB hub | UUGear / Adafruit | Zero4U 4-Port USB Hub without Pogo Pins v1.3, Adafruit Product 4115 | 1 | $9.95 | USB 2.0 hub for simultaneous display touch and SiK radio connectivity; 5 V; 65 × 30 × 8.5 mm [27] |
| CBL6 | USB hub upstream data interconnect | Tech Cabin / Newegg | 20 cm Micro USB Male to Mini 5Pin USB Male OTG Converter Cable | 1 | $17.69 | Direct OTG/data connection from Pi Zero 2 W USB host port to Zero4U upstream Mini-USB port [28] |
| TH1 | Active cooler | TECKEEN / Amazon | Aluminum heatsink with cooling fan, B0BLRVVMBK | 1 | $15 | Thermal management for Raspberry Pi Zero 2 W |
| CBL5 | Radio data interconnect | Adafruit | 3610 | 1 | $2 | Micro-USB to Micro-USB cable for Pi-to-telemetry-radio connection |
| SD1 | Storage | SanDisk / Amazon | 64GB Ultra microSD Card, SDSQUJQ-064G-GZ6MA | 1 | $25 | Operating-system and controller-software storage for Raspberry Pi |
| ENC1 | 3D Printed Enclosure | Custom | Team-designed enclosure | 1 | Purchased in Frame subsystem | Mechanical integration defined by CAD |

### BOM Notes

* The analog trigger subsystem is intentionally deferred to a future implementation and is not included in the current subsystem revision.
* The joystick select button function is also reserved for future implementation and is not assigned in the present input map.
* The Zero4U hub upstream interconnect is finalized as a 20 cm Micro USB male to Mini USB 5 pin male OTG cable, providing the direct Pi USB host to hub data connection [28].
* **Drone Controller Subsystem Total Pricing (w/o tax): $428.82**


## Analysis

The selected architecture is appropriate for a custom drone operator controller because it balances capability, compactness, integration simplicity, operator awareness, and compatibility with the project's final material selections.

The Raspberry Pi Zero 2 WH is a practical controller computer choice because the Zero 2 W platform provides the required combination of compact dimensions, mini HDMI display support, USB connectivity, and GPIO expansion [1]. The final BOM purchases the board through a Vemico development kit that also includes a mini HDMI adapter and USB OTG cable, which reduces the number of separate setup accessories that must be sourced during assembly [2]. For this controller, graphical output, serial communications, and configurable GPIO are more important than high computational throughput, making the Zero 2 W platform a suitable subsystem anchor.

The display also fits the design well. The selected Waveshare 5inch HDMI LCD (H) provides the required 800×480 capacitive touch operator interface [3]. HDMI avoids a DSI only dependency, while USB touch input supports direct operator interaction with the controller software. The display is large enough for status indication, menus, telemetry, and simple mission feedback while remaining compatible with a handheld enclosure.

The helper board approach fits the prototype well. A custom PCB could eventually yield a more polished and compact implementation, but the selected PATIKIL 2 in × 2 in double sided perfboard supports iterative subsystem development without the lead time and re spin cost of a fabricated PCB [4]. The tradeoff is wiring density, so careful component placement and point to point wiring remain important.

The ADC is needed because of the Pi platform's capabilities. Since the Raspberry Pi Zero 2 W does not expose general purpose analog inputs, an external ADC is necessary for joystick acquisition [1]. The selected Microchip MCP3008 provides eight 10 bit analog input channels over SPI, which is sufficient for the four active joystick axes while leaving additional channels available for future analog controls [10]. The retained 0.1 µF decoupling capacitor provides local supply decoupling for the ADC.

The selected Adafruit Model 512 joystick modules are appropriate for the first controller revision because each module provides two analog axes and a select button function in a compact breakout board format [9]. The present design uses the analog axes and reserves the joystick select button feature for later expansion, keeping the initial wiring and input map manageable.

The switch setup is a good fit for this custom controller. The selected TWTADE tactile switch assortment supplies compact momentary switches for the face buttons, D pad, menu/select controls, and bumpers [6]. The dedicated E Switch SPDT toggle for manual/autonomous supervisory mode keeps the mode decision physically visible and directly accessible [7], while the E Switch rocker switch provides a distinct main power control [8].

The power architecture has been updated to the final Nitecore POCKET 5 selection. Rather than implementing a custom battery pack and charging circuit, the controller uses a commercial 5,000 mAh USB C power bank with a 15 W total output rating [5]. This reduces battery management design complexity and matches the controller's USB powered Pi and display architecture. The selected Adafruit 3879 and 4056 cables define the Pi and display power paths respectively [12][14]. Because the power bank's total output is finite, final integration testing should verify controller operation under simultaneous Pi, display, radio, cooling fan, and peripheral loading.

Communication with the drone uses the Holybro SiK Telemetry Radio V3 915 MHz, 100 mW kit [16]. The kit provides a dedicated two module wireless link between the handheld controller and the aircraft rather than relying on the Pi's onboard Wi Fi or Bluetooth for flight communications. This is a better fit for a Pixhawk/ArduPilot based drone architecture and provides an explicit MAVLink/serial communications path for telemetry and supervisory commands [16][20][21]. The selected Adafruit 3610 cable is assigned to the controller side Pi to radio connection [18].

The cable connections are now clearly defined. Video is carried by the 1.5 ft HDMI cable through the mini HDMI adapter included with the Vemico kit [2][13]. Display touch data uses the CVILUX USB cable [15], Pi power uses the Adafruit 3879 cable [12], display power uses the Adafruit 4056 panel mount cable [14], and the SiK radio uses the Adafruit 3610 Micro USB cable [18]. This improves build traceability because each major internal interconnect now has a specific BOM item.

The previous USB topology uncertainty is resolved by the selection of the Zero4U 4 Port USB Hub without Pogo Pins (Adafruit Product 4115) together with the selected 20 cm Micro USB to Mini USB OTG upstream cable. The hub provides four USB 2.0 downstream ports from the Pi's single USB OTG host connection, allowing the touchscreen touch interface and Holybro SiK radio to remain distinct USB devices while operating concurrently [27][28]. The hub's 65 × 30 × 8.5 mm footprint and the short 20 cm upstream cable are appropriate for internal enclosure routing, subject to final CAD placement verification.

Cooling is handled by the selected TECKEEN aluminum heatsink and cooling fan assembly [17]. Because the Pi, touchscreen, telemetry radio, and battery are packaged in a compact controller enclosure, active cooling plus enclosure ventilation provides additional margin against heat buildup. The selected SanDisk 64 GB Ultra microSD card provides sufficient local storage for the Raspberry Pi operating system, controller software, configuration files, and reasonable logging needs [19].

The updated design now also defines controller software behavior, the MAVLink/SiK communications interface, measurable responsiveness targets, a released GPIO/ADC pin map, a verification/validation plan, the selected Zero4U USB expansion architecture, the final joystick to axis mapping, M.A.S. behavior, estimated battery percentage method, and the single SiK prototype qualification strategy. These additions make the subsystem more directly buildable and testable because the expected electrical behavior, software states, communication data, and acceptance criteria are no longer left implicit.

Overall, this version of the subsystem design matches the final BOM and the choices made during prototype planning. It supports operator input capture, touchscreen feedback, explicit supervisory control, dedicated 915 MHz communication to the aircraft, portable self powered operation, active cooling, and realistic student team fabrication while keeping optional input features appropriately deferred to future revisions.


## References

[1] Raspberry Pi Ltd., “Raspberry Pi Zero 2 W,” Raspberry Pi. https://www.raspberrypi.com/products/raspberry-pi-zero-2-w/ . Used for Zero 2 W interface, GPIO, HDMI, USB, and platform characteristics.

[2] Vemico / Amazon, “Raspberry Pi Zero 2 WH Development Kit,” Amazon product B0G5PLTL79. https://www.amazon.com/dp/B0G5PLTL79/ . Final BOM source for the Raspberry Pi Zero 2 WH kit, including mini-HDMI adapter and USB OTG cable.

[3] Waveshare / Newegg, “5inch HDMI LCD (H), 800×480 Capacitive Touch,” Newegg item 9SIC89SM5H7915. https://www.newegg.com/p/2TP-005B-02WM3?item=9SIC89SM5H7915 . Final BOM source for the controller display.

[4] PATIKIL / Amazon, “2 in × 2 in Double-Sided Perfboard,” Amazon product B0FFGT1BD8. https://www.amazon.com/PATIKIL-Prototype-Electronic-Solderable-Breadboard/dp/B0FFGT1BD8/ . Final BOM source for the helper-board perfboard.

[5] Nitecore / Battery Junction, “POCKET 5,” 5,000 mAh USB-C power bank. https://www.batteryjunction.com/products/nitecore-pocket-5?variant=48141327597800 . Final BOM source for the controller portable power supply.

[6] TWTADE / Amazon, “6 × 6 mm 2-Pin Tactile Pushbutton Assortment,” Amazon product B085SWHFMK. https://www.amazon.com/TWTADE-260pcs-Momentary-SwitchTactile-Assortment/dp/B085SWHFMK/ . Final BOM source for face, D-pad, menu/select, and bumper switches.

[7] E-Switch / DigiKey, “100SP1T1B4M2QE,” DigiKey part EG2355-ND. https://www.digikey.com/en/products/detail/e-switch/100SP1T1B4M2QE/378824 . Final BOM source for the manual/autonomous supervisory selector.

[8] E-Switch / DigiKey, “RA1113112R,” DigiKey part EG5619-ND. https://www.digikey.com/en/products/detail/e-switch/RA1113112R/3778055 . Final BOM source for the main controller power switch.

[9] Adafruit Industries / DigiKey, “Analog 2-Axis Thumb Joystick with Select Button + Breakout Board,” Model 512, DigiKey part 1528-512-ND. https://www.digikey.com/en/products/detail/adafruit-industries-llc/512/7056915 . Final BOM source for the two joystick modules.

[10] Microchip Technology / DigiKey, “MCP3008-I/P, 8-Channel 10-Bit SPI ADC,” DigiKey part MCP3008-I/P-ND. https://www.digikey.com/en/products/detail/microchip-technology/MCP3008-I-P/319422 . Final BOM source for the external joystick ADC.

[11] Adafruit Industries / DigiKey, “22 AWG Solid-Core Hook-Up Wire Set,” Model 1311, DigiKey part 1528-1743-ND. https://www.digikey.com/en/products/detail/adafruit-industries-llc/1311/6198255 . Final BOM source for internal soldered wiring.

[12] Adafruit Industries, “USB-C to Micro-B Cable, 1 ft,” Model 3879. https://www.adafruit.com/product/3879 . Final BOM source for the power-bank-to-Pi power cable.

[13] jojobnj / Amazon, “HDMI to HDMI Cable, 1.5 ft,” Amazon product B0CYZRZBZ7. https://www.amazon.com/jojobnj-Gold-Plated-Connectors-Aluminum-soundbar/dp/B0CYZRZBZ7/ . Final BOM source for the Pi-to-display video cable.

[14] Adafruit Industries / DigiKey, “Panel-Mount USB-C Socket to Micro-B Plug Cable, 30 cm,” Model 4056, DigiKey part 1528-4056-ND. https://www.digikey.com/en/products/detail/adafruit-industries-llc/4056/9997694 . Final BOM source for the display power interconnect.

[15] CVILUX USA / DigiKey, “DH-20M50057 USB 2.0 Type-A Male to Micro-B Male Cable,” DigiKey part 2987-DH-20M50057-ND. https://www.digikey.com/en/products/detail/cvilux-usa/DH-20M50057/13177527 . Final BOM source for display touch-data communication.

[16] Holybro, “SiK Telemetry Radio V3, 915 MHz, 100 mW,” Part 17013. https://holybro.com/products/sik-telemetry-radio-v3?variant=41562952302781 . Final BOM source for the two-module controller-to-drone MAVLink/serial radio link.

[17] TECKEEN / Amazon, “Aluminum Heatsink with Cooling Fan for Raspberry Pi Zero,” Amazon product B0BLRVVMBK. https://www.amazon.com/TECKEEN-Aluminum-Heatsink-Double-Raspberry/dp/B0BLRVVMBK/ . Final BOM source for Raspberry Pi thermal management.

[18] Adafruit Industries, “Micro USB to Micro USB Cable,” Model 3610. https://www.adafruit.com/product/3610 . Final BOM source for the Raspberry Pi-to-SiK-radio data cable.

[19] SanDisk / Amazon, “64 GB Ultra microSD Card,” Part SDSQUJQ-064G-GZ6MA. https://www.amazon.com/SANDISK-Ultra-microSD-UHS-I-SDSQUJQ-064G-GZ6MA/dp/B0G8KLQ64L/ . Final BOM source for Raspberry Pi operating-system and controller-software storage.

[20] Holybro, “Pixhawk 6C Mini,” Holybro, 2024. https://holybro.com/collections/flight-controllers/products/pixhawk-6c-mini . Used for aircraft-side controller integration context.

[21] ArduPilot Dev Team, “Radio Control Systems,” ArduPilot Copter Documentation. https://ardupilot.org/copter/docs/common-rc-systems.html . Used for flight-control and radio-integration context.

[22] OpenAI, “ChatGPT (GPT-5.6 Sol),” large language model, used for drafting, formatting, and editing assistance, 2026.

[23] Holybro, “SiK Telemetry Radio V3,” Holybro Documentation. https://docs.holybro.com/radio/sik-telemetry-radio-v3 . Used for SiK transparent serial behavior, 915 MHz band, default RF data rate, default serial data rate, electrical interface, and MAVLink framing.

[24] ArduPilot Dev Team, “RC Input (aka Pilot Input),” ArduPilot Developer Documentation. https://ardupilot.org/dev/docs/mavlink-rcinput.html . Used for MAVLink MANUAL_CONTROL behavior and the documented latency/bandwidth considerations of pilot input over telemetry.

[25] ArduPilot Dev Team, “MAVLink Advanced Configuration,” ArduPilot Copter Documentation. https://ardupilot.org/copter/docs/common-mavlink-configuration.html . Used for MAVLink channel identity and telemetry stream-rate configuration context.

[26] ArduPilot Dev Team, “Mission Commands,” ArduPilot Copter Documentation. https://ardupilot.org/copter/docs/common-mavlink-mission-command-messages-mav_cmd.html . Used for supported return-to-launch, land, and other mission-command interface definitions.

[27] UUGear / Adafruit Industries, “Zero4U - 4-Port USB Hub without Pogo Pins - v1.3,” Adafruit Product 4115. https://www.adafruit.com/product/4115 . Final BOM source for the internal USB 2.0 hub; used for 5 V operation, bus/self-power capability, four-port USB expansion, dimensions, and current product pricing.

[28] Tech Cabin / Newegg, “20cm Micro USB Male to Mini 5Pin USB Male OTG Converter Cable, Male to Male Adapter Cord,” https://www.newegg.com/p/181-08GR-00E09 . Final BOM source for the Zero4U upstream USB connection; used for the 20 cm length, Micro-USB male to Mini-USB 5-pin male connectors, OTG/data capability, and current listed price.
