## Frame Subsystem Detailed Design

### Function of the Subsystem

&nbsp; &nbsp; &nbsp; &nbsp; The frame subsystem provides the primary mechanical structure for the autonomous acoustic measurement drone. It supports the propulsion system, battery, flight controller, sensing hardware, onboard processing hardware, wiring, and landing gear while maintaining the geometry required for stable multirotor flight.

&nbsp; &nbsp; &nbsp; &nbsp; The frame also directly affects acoustic measurement performance. Structural flexing, motor-induced vibration, and poor component placement can introduce unwanted motion and noise into the sensing system. Therefore, the frame design must provide sufficient stiffness, maintain motor alignment, minimize vibration transmission to sensitive components, and remain lightweight enough to satisfy the aircraft power and endurance requirements.

&nbsp; &nbsp; &nbsp; &nbsp; The frame will be manufactured using fused-filament fabrication. PETG-CF will be used for prototype frames because of its lower cost and ease of iteration. The final frame will be printed using carbon-fiber-reinforced PA12 nylon (CF-PA12). The landing gear will be printed separately using TPU 95A.

---

### Specifications and Constraints

#### Design Specifications

| Parameter | Requirement |
|---|---:|
| Frame configuration | H-frame |
| Overall frame size | 14.142 in × 14.142 in |
| Motor-to-motor diagonal | 20.000 in |
| Center body area | Approximately 14.000 in × 4.000 in |
| Arm length | Approximately 5.898 in |
| Top-to-bottom spacing | 2.250 in |
| Prototype frame material | PETG-CF |
| Final frame material | CF-PA12 |
| Landing gear material | TPU 95A |
| Landing gear length | Approximately 5.931 in |
| Maximum aircraft design mass | 2.30 kg |
| Target frame-subsystem mass | ≤ 500 g |
| Static motor-mount deflection | ≤ 2.0 mm under 15 N vertical test load |
| Minimum structural factor of safety | 2.0 under the defined design load case |
| Vibration-isolation target | ≥ 50% reduction in RMS acceleration at the isolated sensor mount relative to the rigid frame mount |
| Landing gear clearance | No underside component shall contact the ground during a normal level landing |

#### Constraints

- The frame shall remain below the allocated mass budget to preserve battery endurance.
- The frame shall maintain motor alignment under expected thrust loading.
- The frame shall provide sufficient propeller clearance for the selected propulsion configuration.
- The frame shall provide mounting space for the battery, flight controller, sensing hardware, processing hardware, and wiring.
- The final frame shall be manufacturable using CF-PA12 on an FDM printer capable of printing abrasive engineering filament.
- The prototype frame shall be manufacturable using PETG-CF.
- The landing gear shall use TPU 95A to provide flexible ground contact and impact absorption.
- The frame shall include removable vibration-isolated mounting provisions for acoustic sensing hardware.
- The frame shall be inspected before flight for cracks, delamination, loose fasteners, or landing gear damage.
- The frame shall satisfy the quantitative validation criteria defined in the verification section.

---

### Material Selection

#### Prototype Material — PETG-CF

PETG-CF is selected for the prototype frame because it allows the team to verify dimensional fit, mounting-hole locations, subsystem packaging, center-of-mass placement, and assembly procedures at significantly lower material cost than the final CF-PA12 frame.

SUNLU currently lists PETG-CF at approximately $19.99 per 1 kg spool [1].

Prototype testing will be used primarily for:
- dimensional verification
- component fit checks
- wiring-path verification
- landing-gear integration
- low-risk ground testing
- refinement of print orientation and support strategy

The PETG-CF prototype shall not be used as evidence that the final frame meets the structural requirements unless it separately passes the structural validation tests.

#### Final Material — CF-PA12

The final frame will use carbon-fiber-reinforced PA12 nylon. MatterHackers NylonX consists of approximately 80% PA12 nylon and 20% chopped carbon fiber [2].

The material is selected because it provides:
- increased stiffness compared with standard unfilled thermoplastics
- good strength-to-weight performance
- improved dimensional stability
- reduced warping compared with conventional nylon
- greater toughness than brittle prototype materials

MatterHackers lists NylonX at approximately $63 per 0.5 kg spool [2].

The manufacturer's published material data will be used for preliminary structural analysis; however, the final printed frame shall be physically tested because FDM mechanical properties depend on print orientation, layer bonding, infill, wall count, moisture condition, and processing temperature.

---

### Overview of Proposed Solution

&nbsp; &nbsp; &nbsp; &nbsp; The selected frame is a custom 3D-printed H-frame with a 14.142 in × 14.142 in overall footprint and a 20.000 in motor-to-motor diagonal. The central body is approximately 14.000 in × 4.000 in, providing a long mounting region for the battery, flight controller, onboard processing hardware, and sensing electronics.

&nbsp; &nbsp; &nbsp; &nbsp; Four approximately 5.898 in arms extend from the center body to the motor mounting regions. The relatively short arm geometry was selected to reduce unsupported bending length while maintaining the required motor spacing.

&nbsp; &nbsp; &nbsp; &nbsp; The final frame will be printed in CF-PA12. Fillets will be incorporated at arm-to-body transitions to reduce local stress concentrations. Motor mounting regions will use increased wall thickness and local material reinforcement. The print orientation shall be selected so that the principal in-plane motor loads are carried primarily within continuous printed roads rather than relying only on inter-layer tensile strength.

&nbsp; &nbsp; &nbsp; &nbsp; The frame uses a 2.250 in stacked vertical arrangement for component clearance. Four TPU 95A landing legs approximately 5.931 in long provide ground clearance and absorb a portion of landing impact energy.

---

### Structural Design and Rigidity Analysis

#### Design Load Case

The maximum design aircraft mass is:

\[
m = 2.30 \text{ kg}
\]

The corresponding weight is:

\[
W = mg
\]

\[
W = (2.30)(9.81) = 22.56 \text{ N}
\]

During steady hover, the average thrust per motor is approximately:

\[
T_{hover} = \frac{22.56}{4} = 5.64 \text{ N}
\]

For structural validation, the frame will be evaluated using a conservative load greater than normal hover thrust. A **15 N vertical load** will be applied at each motor mounting region during individual arm testing. This corresponds to approximately 2.66 times the nominal hover thrust per motor.

#### Finite Element Analysis

A static FEA shall be performed on the final CAD geometry using CF-PA12 material properties.

The analysis shall include:
- fixed or constrained center-body mounting region
- 15 N vertical load applied at the selected motor mount
- identical evaluation of all four arms
- evaluation of maximum displacement
- evaluation of maximum von Mises stress
- identification of stress concentrations at arm-to-body transitions and fastener holes

Initial CF-PA12 material properties may be taken from manufacturer technical data. MatterHackers reports a tensile modulus of approximately 6000 MPa and tensile strength of approximately 100 MPa for NylonX [3].

Because the frame is additively manufactured and anisotropic, these manufacturer values shall not be treated as guaranteed finished-part properties. The FEA shall therefore be followed by physical testing.

#### Structural Acceptance Criteria

The frame passes the rigidity requirement if:

1. Maximum measured vertical motor-mount deflection is **≤ 2.0 mm under a 15 N static load**.
2. No visible cracking, delamination, permanent bending, or fastener-hole damage occurs.
3. Permanent deformation after load removal is **≤ 0.5 mm**.
4. FEA predicts a **minimum factor of safety of 2.0** for the selected design load.
5. The frame survives **three consecutive 15 N loading cycles on each motor arm** without failure or increasing permanent deformation.

If any arm fails these criteria, the geometry shall be revised by adjusting wall thickness, local reinforcement, fillet radius, print orientation, or material distribution.

---

### Frame Mass Budget

The 500 g target is derived from the total aircraft mass requirement rather than being an arbitrary target.

The current aircraft target is approximately:

\[
m_{aircraft,max} = 2300 \text{ g}
\]

The existing propulsion and non-frame electronics allocation is approximately:

\[
m_{non-frame} = 1682.4 \text{ g}
\]

This leaves:

\[
m_{remaining} = 2300 - 1682.4
\]

\[
m_{remaining} = 617.6 \text{ g}
\]

A frame-subsystem target of 500 g therefore leaves approximately:

\[
617.6 - 500 = 117.6 \text{ g}
\]

for final wiring, connectors, adhesives, small mounting hardware, and integration uncertainty.

#### Frame Subsystem Mass Budget

| Component | Target Mass |
|---|---:|
| CF-PA12 primary printed frame | ≤ 380 g |
| Frame fasteners and standoffs | ≤ 45 g |
| TPU 95A landing gear | ≤ 35 g |
| Sensor/flight-controller isolation mounts | ≤ 20 g |
| Cable-retention/frame accessories | ≤ 10 g |
| Design contingency | ≤ 10 g |
| **Total Frame Subsystem Target** | **≤ 500 g** |

The values above are design allocations. The final values shall be replaced with:
1. slicer-predicted material mass before fabrication, and
2. measured component mass using a digital scale after fabrication.

The completed frame subsystem passes the mass requirement only if its measured assembled mass is **≤ 500 g**.

---

### Vibration Mitigation Strategy

Vibration mitigation is required because propulsion vibration can affect the microphone, IMU, and other sensing hardware.

The frame will use the following mitigation methods:

1. **Propeller balancing**
   - All propellers shall be inspected and balanced before vibration testing.

2. **Motor inspection**
   - Motor shafts, bearings, and mounting screws shall be inspected for looseness or abnormal vibration.

3. **Rigid motor mounting**
   - Motors shall be mounted securely to prevent movement between the motor base and frame.

4. **CF-PA12 frame stiffness**
   - The frame geometry shall limit excessive arm bending and resonant motion.

5. **Isolated acoustic sensor mount**
   - The acoustic sensing assembly shall not be rigidly coupled directly to a high-vibration motor arm.
   - TPU, elastomeric grommets, or another compliant isolation interface shall be placed between the microphone/sensor bracket and the primary frame.

6. **Flight-controller isolation**
   - The flight controller/IMU shall use vibration-damping mounting hardware where required.

7. **Component placement**
   - Sensitive sensors shall be mounted near the central body rather than near the motor mounts where practical.

---

### Quantitative Vibration Test

An accelerometer or IMU shall be used to measure vibration at:

- Point A: rigid frame near the sensor mounting location
- Point B: vibration-isolated sensor mount

The aircraft shall be secured to a test fixture and operated at representative motor speeds.

The following data shall be recorded:
- RMS acceleration
- peak acceleration
- dominant vibration frequencies
- motor operating condition

#### Vibration Acceptance Criteria

The vibration-isolation system passes if:

- RMS acceleration measured at the sensor mount is **at least 50% lower** than the rigid frame measurement over the selected test interval, **or**
- the isolation system provides at least **6 dB attenuation** in the primary motor/propeller vibration band.

The sensor mounting system shall also show:
- no loosening of fasteners
- no permanent deformation
- no contact between the isolated sensor assembly and the rigid frame

If the vibration requirement is not met, the team shall modify the isolator stiffness, mounting geometry, sensor placement, or propulsion balancing.

---

### Landing Gear Design

The landing gear consists of four approximately 5.931 in legs printed from TPU 95A.

SUNLU specifies TPU 95A with Shore hardness 95A, density approximately 1.21 g/cm³, tensile strength approximately 31 ± 3 MPa, and high elongation at break [4].

The TPU landing gear is intended to:
- maintain ground clearance
- absorb touchdown impact
- prevent direct rigid-frame contact with the ground
- reduce shock transmission to sensors and electronics
- remain replaceable independently of the primary frame

#### Landing Gear Acceptance Criteria

The landing gear shall pass the following test:

1. Install all four legs on the completed frame.
2. Load the aircraft to its 2.30 kg maximum design mass.
3. Perform five controlled vertical drop tests from a height of **100 mm** onto a flat test surface.
4. Inspect the landing gear and frame after every drop.

The landing system passes if:
- no underside aircraft component touches the ground
- no leg detaches
- no visible tearing or cracking occurs
- no permanent leg deformation greater than **2 mm** remains after the fifth test
- no frame cracking occurs at the landing-gear mounting points

---

### Interface with Other Subsystems

#### Power and Propulsion Interface

| Interface Item | Description |
|---|---|
| Interface type | Mechanical support and electrical-routing support |
| Connected components | Motors, ESCs, battery, propellers |
| Signal/power routed | Battery DC wiring and motor/ESC wiring |
| Mechanical input | Motor thrust, torque, and vibration |
| Mechanical output | Motor alignment and load transfer into central body |
| Communication protocol | None; frame is a passive mechanical subsystem |

#### Flight Control Interface

| Interface Item | Description |
|---|---|
| Interface type | Mechanical mounting and vibration isolation |
| Connected components | Flight controller and IMU |
| Signals carried by mounted system | UART, I2C, PWM and other controller-specific signals |
| Frame input | Mass, mounting, and isolation requirements |
| Frame output | Rigid central mounting location and vibration isolation |
| Direct frame communication | None |

#### Acoustic Sensing Interface

| Interface Item | Description |
|---|---|
| Interface type | Mechanical mounting and vibration isolation |
| Connected components | Microphone and associated sensing hardware |
| Signals carried by mounted system | Analog/digital audio depending on sensor implementation |
| Frame input | Sensor position and isolation requirements |
| Frame output | Stable, vibration-isolated mounting interface |
| Direct frame communication | None |

#### Landing Gear Interface

| Interface Item | Description |
|---|---|
| Interface type | Mechanical |
| Connected components | Four TPU 95A landing legs |
| Input | Ground reaction and landing impact forces |
| Output | Load transfer into primary frame |
| Communication protocol | None |

---

### 3D Model of Custom Mechanical Components

The detailed CAD model shall document:

- 14.142 in × 14.142 in overall frame footprint
- 20.000 in motor-to-motor diagonal
- approximately 14.000 in × 4.000 in central body
- approximately 5.898 in arm length
- 2.250 in vertical frame spacing
- motor mounting-hole patterns
- flight-controller mounting locations
- battery mounting area
- acoustic sensor mount location
- wiring paths
- TPU landing-gear mounting locations
- approximately 5.931 in landing-gear leg length
- fillets at high-stress arm-to-body transitions

All final CAD dimensions shall be frozen before the final CF-PA12 print.

---

### Manufacturing Requirements

#### PETG-CF Prototype

The prototype shall be printed first to verify:
- dimensions
- fit
- motor mounting
- landing-gear mounting
- electronics placement
- wiring clearance
- propeller clearance

PETG-CF currently costs approximately $19.99 per 1 kg spool [1].

#### CF-PA12 Final Frame

The final frame shall use CF-PA12.

MatterHackers recommends hardened or otherwise abrasion-resistant nozzles for NylonX and specifies that the material is hygroscopic and should be dried before printing [2].

The final print procedure shall document:
- printer model
- nozzle material and diameter
- nozzle temperature
- bed temperature
- layer height
- wall/perimeter count
- infill percentage and pattern
- print orientation
- drying procedure
- post-processing or annealing, if used

These parameters shall be recorded because they directly affect structural repeatability.

---

### Verification and Test Plan

| Test | Method | Pass/Fail Criterion |
|---|---|---|
| Dimensional inspection | Calipers/tape/CAD comparison | Critical dimensions within ±1.0 mm unless otherwise specified |
| Frame mass | Digital scale | Complete frame subsystem ≤ 500 g |
| Arm rigidity | 15 N load at each motor mount | Deflection ≤ 2.0 mm |
| Permanent deformation | Measure after load removal | ≤ 0.5 mm |
| Static durability | Three 15 N load cycles per arm | No cracks/delamination |
| FEA structural margin | Static FEA | Factor of safety ≥ 2.0 |
| Propeller clearance | Physical rotation/measurement | No frame/component interference |
| Vibration isolation | IMU/accelerometer comparison | ≥ 50% RMS reduction or ≥ 6 dB attenuation |
| Landing clearance | Fully assembled aircraft | No underside component contacts ground |
| Landing durability | Five 100 mm drop tests at design mass | No cracking/detachment; ≤2 mm permanent leg deformation |
| Fastener inspection | Visual/manual inspection | No loosening after test sequence |

---

### Bill of Materials

| Item | Description | Manufacturer / Source | Qty | Est. Unit Price | Est. Total |
|---|---|---|---:|---:|---:|
| PETG-CF | Prototype frame material, 1 kg spool | SUNLU | 1 | $19.99 | $19.99 |
| NylonX CF-PA12 | Final structural frame material, 0.5 kg spool | MatterHackers | 2 | $63.00 | $126.00 |
| TPU 95A | Landing gear material, 1 kg spool | SUNLU | 1 | ~$20 | ~$20 |
| Frame fasteners | M2.5/M4/8-32 screws and locknuts | McMaster-Carr | 1 set | TBD from final quantities | TBD |
| Standoffs | Frame/electronics spacing hardware | McMaster-Carr / equivalent | 1 set | TBD | TBD |
| Vibration isolators | TPU/elastomeric sensor and FC mounts | Custom / commercial | As required | TBD | TBD |

**Current material subtotal before final hardware quantities: approximately $166.**

The BOM shall be updated after the final CAD and hardware count are frozen.

---

### Analysis of Crucial Design Decisions

#### H-Frame Geometry

The H-frame was selected because its long central body provides mounting space for the battery, controller, processing hardware, and sensing components while allowing the four motors to remain symmetrically positioned.

#### Arm Length

The approximately 5.898 in arm length reduces unsupported length compared with a larger frame while still providing the required motor spacing. The expected benefit is increased stiffness and reduced mass. This claim will be verified using FEA and the defined 15 N static arm-load test rather than assumed.

#### Mass Target

The 500 g frame target is derived from the 2.30 kg aircraft mass limit. With approximately 1682.4 g already allocated to propulsion and non-frame electronics, approximately 617.6 g remains. Limiting the frame subsystem to 500 g preserves approximately 117.6 g of integration margin.

#### CF-PA12 Selection

CF-PA12 was selected as the final frame material because it provides an engineering-grade 3D-printable structure with higher stiffness than conventional unfilled thermoplastics while retaining the manufacturing flexibility required for the custom H-frame geometry.

#### Vibration Control

Vibration will not be controlled by material selection alone. The final system uses a combination of propulsion balancing, rigid motor attachment, central sensor placement, and compliant vibration-isolation mounts. The effectiveness of the isolation will be experimentally verified.

#### Landing Gear

TPU 95A landing gear provides a compliant first contact with the ground. Its effectiveness will be validated through repeatable drop testing rather than assumed from material flexibility alone.

---

### Detailed Shall Statements

#### Structural Requirements

1. The frame subsystem shall support a maximum aircraft mass of 2.30 kg.
2. The frame subsystem shall maintain a factor of safety of at least 2.0 for the defined structural design load.
3. Each motor arm shall deflect no more than 2.0 mm when subjected to a 15 N vertical load at the motor mounting location.
4. Each arm shall exhibit no more than 0.5 mm permanent deformation after the static load is removed.
5. The frame shall survive three consecutive 15 N static load cycles at each motor mount without cracking or delamination.

#### Mass Requirements

1. The complete frame subsystem shall have a measured mass of no more than 500 g.
2. The frame mass shall be verified using a digital scale after final assembly.
3. The slicer-predicted frame mass shall be documented before fabrication.
4. The final mass budget shall include printed frame material, landing gear, fasteners, standoffs, isolators, and mounting hardware.

#### Vibration Requirements

1. The frame subsystem shall include a vibration-isolated mounting interface for the acoustic sensing hardware.
2. The isolation system shall reduce RMS acceleration by at least 50% relative to an equivalent rigid mounting point, or provide at least 6 dB attenuation in the dominant propulsion vibration band.
3. Propellers shall be inspected and balanced prior to final vibration testing.
4. Motor mounts shall be inspected for looseness prior to vibration testing.

#### Landing Requirements

1. The frame shall use four TPU 95A landing legs.
2. The landing gear shall prevent all underside electronics from contacting the ground during normal level landing.
3. The landing system shall survive five 100 mm drop tests at the 2.30 kg design mass without cracking or detachment.
4. Permanent deformation of a landing leg after the drop-test sequence shall not exceed 2 mm.

#### Manufacturing Requirements

1. The prototype frame shall be manufactured using PETG-CF.
2. The final structural frame shall be manufactured using CF-PA12.
3. Final print parameters shall be documented and controlled.
4. CF-PA12 shall be dried and stored according to the material manufacturer's recommendations prior to printing.
5. An abrasion-resistant nozzle shall be used for CF-PA12 printing.

#### Validation Requirements

1. The subsystem shall complete dimensional inspection before propulsion hardware is installed.
2. The subsystem shall complete static structural testing before free-flight testing.
3. The subsystem shall complete vibration testing before final acoustic validation.
4. The subsystem shall complete landing-gear drop testing before autonomous landing testing.
5. Test results shall be recorded with measured values and explicit pass/fail status.

---

### References

[1] SUNLU, “PETG-CF (PETG Carbon Fiber) 3D Printer Filament 1KG,” SUNLU Online Store. [Online]. Available: https://store.sunlu.com/products/petg-cfpetg-carbon-fiber-3d-printer-filament-1kg.

[2] MatterHackers, “NylonX Carbon Fiber PA12 Filament,” MatterHackers. [Online]. Available: https://www.matterhackers.com/store/3d-printer-filament/nylonx-carbon-fiber-nylon-filament-1.75mm.

[3] MatterHackers, “NylonX Technical Data Sheet,” MatterHackers. Tensile modulus approximately 6000 MPa and tensile strength approximately 100 MPa.

[4] SUNLU, “TPU 95A,” SUNLU. Shore hardness 95A, density approximately 1.21 g/cm³, and tensile strength approximately 31 ± 3 MPa.