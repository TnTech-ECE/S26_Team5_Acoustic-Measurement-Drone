

# Frame Subsystem Detailed Design

### Function of the Subsystem

&nbsp; &nbsp; &nbsp; &nbsp; The frame subsystem provides the primary mechanical structure for the autonomous acoustic measurement drone. Its purpose is to support the propulsion system, power system, flight controller, sensing hardware, onboard processing hardware, and wiring while maintaining the geometry required for stable multirotor flight.

&nbsp; &nbsp; &nbsp; &nbsp; The frame also supports the acoustic measurement mission by providing a stable mounting platform for sensors and electronics. Since the drone is intended to collect acoustic data, the frame must minimize unnecessary vibration, maintain component alignment, and provide enough mounting space for the battery, flight controller, sensors, and wiring.

---

### Specifications and Constraints

The frame subsystem shall satisfy the following dimensional specifications:

| Parameter | Value |
|---|---:|
| Frame configuration | H-frame |
| Overall frame size | 14.142 in × 14.142 in |
| Motor-to-motor diagonal | 20.000 in |
| Center body plate area | Approximately 14.000 in × 4.000 in |
| Arm length | Approximately 5.898 in |
| Top-to-bottom spacing | 2.250 in |
| Primary material | Carbon fiber or carbon-fiber-reinforced material |
| Secondary/custom mount material | PETG or carbon-fiber-reinforced nylon |
| Landing gear material | TPU 95A (Shore hardness 95A thermoplastic polyurethane) |
| Target frame mass | Approximately 500 g |
| Supported total aircraft mass | Approximately 2.2–2.3 kg |

#### Constraints

- The frame shall be lightweight to reduce propulsion power demand and improve flight time.
- The frame shall be rigid enough to prevent excessive flexing during takeoff, hover, maneuvering, and landing.
- The frame shall provide sufficient mounting space for the battery, flight controller, acoustic sensing hardware, and wiring.
- The frame shall maintain motor alignment to support stable thrust generation.
- The frame shall include sufficient clearance for propeller rotation.
- The frame shall support safe handling, transport, takeoff, and landing.
- The frame shall include protective spacing between the ground and underside components through the 2.25 in top-to-bottom spacing.
- The landing gear shall be 3D-printed from TPU 95A to provide flexible, impact-absorbing contact points during touchdown.
- The frame shall support safe operation by reducing the risk of loose components, sharp edges, and structural failure.

---

### Overview of Proposed Solution

&nbsp; &nbsp; &nbsp; &nbsp; The proposed frame design is an H-frame structure with four arms extending from a long central body plate. The selected layout has an overall square footprint of approximately 14.142 in × 14.142 in and a motor-to-motor diagonal of 20.000 in. This geometry provides a compact but stable layout for the selected propulsion system.

&nbsp; &nbsp; &nbsp; &nbsp; The central body plate is approximately 14.000 in long and 4.000 in wide. This provides enough mounting area for the battery, flight controller, onboard electronics, and sensor hardware while keeping the design narrow enough to reduce unnecessary weight. Each arm is approximately 5.898 in long from the central body transition to the motor mount region.

&nbsp; &nbsp; &nbsp; &nbsp; The design uses a stacked frame arrangement with a top-to-bottom height of approximately 2.250 in. This vertical spacing provides room for hardware mounting, wiring clearance, and limited impact protection during landing.

&nbsp; &nbsp; &nbsp; &nbsp; The landing gear consists of four 3D-printed feet fabricated from TPU 95A filament. TPU 95A (Shore hardness 95A) was selected because it provides a balance of structural firmness and elasticity that is well-suited for absorbing landing impact loads. Unlike rigid materials, TPU 95A compresses slightly on touchdown and rebounds to its original shape, reducing shock transfer to the frame and electronics above. The 5.931 in printed landing gear legs extend below the frame body to ensure all underside components clear the ground during normal landing.

---

### Interface with Other Subsystems

#### Interface with Power and Propulsion Subsystem

| Interface Item | Description |
|---|---|
| Interface type | Mechanical support and electrical routing |
| Connected components | Motors, ESCs, propellers, battery |
| Signal/power type | DC power wiring and motor control wiring routed through or along the frame |
| Direction | Frame provides mechanical output/support to propulsion components; propulsion loads are transferred back into the frame |
| Data/protocol | No communication protocol is used by the frame itself |

The frame provides mounting holes and arm geometry for the motors. The arms maintain motor spacing and alignment so that thrust is distributed symmetrically around the aircraft. The center body provides mounting area for the battery and supports routing from the battery to the ESCs.

#### Interface with Flight Control Subsystem

| Interface Item | Description |
|---|---|
| Interface type | Mechanical mounting and vibration control |
| Connected components | Flight controller, IMU, wiring |
| Signal type | Digital control and sensor signals routed through mounted electronics |
| Direction | Frame supports the flight controller; vibration from the frame may affect IMU readings |
| Data/protocol | No direct frame protocol; flight controller may use PWM, UART, I2C, or CAN depending on connected devices |

The frame provides a central mounting location for the flight controller. The flight controller should be mounted near the center of the frame to reduce vibration and improve estimation of aircraft motion.

#### Interface with Sensing and Acoustic Measurement Subsystem

| Interface Item | Description |
|---|---|
| Interface type | Mechanical support and vibration isolation |
| Connected components | Microphone, acoustic sensor mounts, navigation sensors |
| Signal type | Analog audio and digital sensor data carried by sensing hardware |
| Direction | Frame provides mechanical support; sensing subsystem receives stability benefit from frame rigidity |
| Data/protocol | No direct frame protocol; sensors may use analog audio, USB, I2C, UART, or other subsystem-specific interfaces |

The frame supports the acoustic sensor hardware and must minimize vibration transfer to the microphone. Sensor mounts may use PETG or carbon-fiber-reinforced nylon brackets with damping material if needed.

#### Interface with Landing and Protection Features

| Interface Item | Description |
|---|---|
| Interface type | Mechanical support |
| Connected components | TPU 95A landing feet/legs, dampers, protective mounts |
| Signal type | None |
| Direction | Frame transfers landing loads from TPU 95A feet/legs into the main body |
| Data/protocol | None |

The 2.25 in top-to-bottom spacing, combined with the 5.931 in TPU 95A landing gear legs, allows the frame to maintain adequate ground clearance. The TPU 95A material provides inherent flexibility and impact absorption at each landing contact point, protecting the battery, electronics, and sensor hardware from hard ground contact and shock loads during touchdown.

---

### 3D Model of Custom Mechanical Components

&nbsp; &nbsp; &nbsp; &nbsp; The frame was modeled as an H-frame with a long central body plate and four angled arms. The design includes motor mounting regions at the end of each arm, fastener holes along the center plate, and internal spacing for stacked components.

The frame model includes the following measured features:

- Overall square footprint: **14.142 in × 14.142 in**
- Motor-to-motor diagonal: **20.000 in**
- Center body plate: **approximately 14.000 in × 4.000 in**
- Arm length: **approximately 5.898 in**
- Top-to-bottom spacing: **2.250 in**
- Landing gear leg length: **5.931 in (3D-printed TPU 95A)**



<img width="794" height="603" alt="image-2" src="https://github.com/user-attachments/assets/0dacb83c-9464-4a18-9efc-e67c4a340902" />



**Figure 1.** Top view of the H-frame drone body showing the overall frame layout and motor locations.












<img width="620" height="560" alt="image-1" src="https://github.com/user-attachments/assets/a4cc70d7-02d7-4501-ba16-070bb188eda6" />



**Figure 2.** Diagonal motor-to-motor distance of 20.000 in.













<img width="848" height="511" alt="image-5" src="https://github.com/user-attachments/assets/2e089493-0f8e-4f5b-a9f7-5beb9b06d530" />



**Figure 3.** Side view of the frame.








<img width="573" height="377" alt="image-3" src="https://github.com/user-attachments/assets/b7a6ba25-a88d-48ff-a831-e2ccd9426ab6" />



**Figure 4.** Side view of the frame showing the 2.250 in top-to-bottom spacing.







<img width="1005" height="705" alt="image-4" src="https://github.com/user-attachments/assets/afef5d07-1a8a-4ada-8110-4c84ecaf701d" />



**Figure 5.** TPU 95A landing gear legs, 5.931 in in length.








---

# Buildable Mechanical Diagram — H-Frame Drone Subsystem

## Frame Dimensions

| Feature | Dimension |
|---|---|
| Overall width | 14.142 in |
| Overall height | 14.142 in |
| Diagonal motor-to-motor distance | 20.000 in |
| Center body plate length | 14.000 in |
| Center body plate width | 4.000 in |
| Approximate arm length | 5.898 in |
| Vertical spacing | 2.250 in |
| Landing gear leg length | 5.931 in |
| Landing gear material | TPU 95A (Shore hardness 95A) |

## Assembly Notes

- Mount the four motors at the motor mounting holes located at the end of each arm.
- Mount the flight controller near the geometric center of the frame.
- Mount the battery along the center body plate to maintain center-of-mass balance.
- Route battery and ESC wiring along the frame body and arms.
- Mount acoustic sensing hardware on an isolated bracket to reduce vibration transfer.
- Attach four TPU 95A landing gear legs at the designated mounting points on the underside of the frame. Verify that each leg is flush and secured before flight.
- Verify that all propeller zones are clear before operation.
- Inspect all screws, standoffs, mounts, and TPU 95A landing gear for cracks or deformation before flight.

---

## Functional Operation

During operation, the frame acts as a passive structural subsystem. It does not process data or communicate electronically, but it directly affects the performance of the aircraft by supporting all components, maintaining alignment, and transferring loads.

During takeoff and flight, motor thrust loads are transferred from each motor mount through the arms and into the center body. The center body supports the battery and electronics near the aircraft center of mass. During landing, impact loads are transferred from the TPU 95A landing gear contact points through the legs and into the main frame structure. The flexibility of the TPU 95A material absorbs a portion of the shock before it reaches the rigid frame, reducing stress on the electronics and sensor hardware.

## Functional Flowchart


<img width="336" height="473" alt="image" src="https://github.com/user-attachments/assets/818fb9c8-39dd-4627-a50b-3092b22db3de" />

---

## Bill of Materials

| Item | Description | Manufacturer / Source | Qty | Est. Unit Price | Est. Total | Purchasing URL |
|---|---|---|---|---|---|---|
| Carbon fiber plate/sheet | Main body plates and arms, approx. 3 mm carbon fiber sheet | COYOUCO / Amazon | 1–2 sheets | $30–$80 | $30–$160 | [Amazon — COYOUCO Carbon Fiber Sheet [1]](https://www.amazon.com/COYOUCO-Carbon-Fiber-Surface-Sheets/dp/B0DLB6F3Z6) |
| M3 aluminum standoffs | Frame spacing and stacked assembly support | ImpulseRC / MyFPVStore | 1 kit | $3.00–$6.00 | $3.00–$12.00 | [MyFPVStore — ImpulseRC M3 Standoff [2]](https://www.myfpvstore.com/extras-and-hardware/impulserc-m3-standoff-pick-your-length/) |
| M2.5 × 10 mm flat head screws | Zinc-plated steel Phillips flat head, M2.5 × 0.45 mm thread, 10 mm long — PN 91420A020 | McMaster-Carr | 4 | ~$0.10–$0.20 ea | ~$0.40–$0.80 | [McMaster-Carr 91420A020 [3]](https://www.mcmaster.com/91420A020/) |
| M2.5 × 12 mm flat head screws | Zinc-plated steel Phillips flat head, M2.5 × 0.45 mm thread, 12 mm long — PN 91420A022 | McMaster-Carr | 2 | ~$0.10–$0.20 ea | ~$0.20–$0.40 | [McMaster-Carr 91420A022 [4]](https://www.mcmaster.com/91420A022/) |
| M2.5 Nylock nuts | M2.5 × 0.45 mm hex nylon-insert locknuts | McMaster-Carr | 2 | ~$0.10–$0.20 ea | ~$0.20–$0.40 | [McMaster-Carr — M2.5 Nylock Nut [5]](https://www.mcmaster.com/products/nylock/thread-size~m2-5/nut-type~hex/locking-type~nylon-insert/) |
| M4 × 20 mm flat head screws | Zinc-plated steel Phillips flat head, M4 × 0.7 mm thread, 20 mm long — PN 91420A228 | McMaster-Carr | 8 | ~$0.15–$0.25 ea | ~$1.20–$2.00 | [McMaster-Carr 91420A228 [6]](https://www.mcmaster.com/91420A228/) |
| M4 Nylock nuts | Medium-strength steel nylon-insert locknut Class 8, zinc plated, M4 × 0.7 mm, 5 mm high — PN 90576A103 | McMaster-Carr | 8 | ~$0.15–$0.25 ea | ~$1.20–$2.00 | [McMaster-Carr 90576A103 [7]](https://www.mcmaster.com/90576A103/) |
| M4 hex standoffs (female threaded) | Corrosion-resistant 18-8 stainless steel female threaded hex standoff — PN 91115A827 | McMaster-Carr | 8 | ~$1.50–$3.00 ea | ~$12.00–$24.00 | [McMaster-Carr 91115A827 [8]](https://www.mcmaster.com/91115A827/) |
| M4 × 20 mm flat head screws (motor mount) | Zinc-plated steel Phillips flat head, M4 × 0.7 mm thread, 20 mm long — PN 91420A228 (same PN as above) | McMaster-Carr | 16 | ~$0.15–$0.25 ea | ~$2.40–$4.00 | [McMaster-Carr 91420A228 [6]](https://www.mcmaster.com/91420A228/) |
| 8-32 × 9/16 in flat head screws | 316 stainless steel hex-drive flat head, 82° countersink, 8-32 thread, 9/16 in long — PN 90585A134 | McMaster-Carr | 20 | ~$0.20–$0.35 ea | ~$4.00–$7.00 | [McMaster-Carr 90585A134 [9]](https://www.mcmaster.com/90585A134/) |
| 8-32 thin Nylock nuts | Low-strength steel nylon-insert locknut, thin-profile, zinc-plated, 8-32 thread — PN 90633A009 | McMaster-Carr | 8 | ~$0.15–$0.25 ea | ~$1.20–$2.00 | [McMaster-Carr 90633A009 [10]](https://www.mcmaster.com/90633A009/) |
| 8-32 × 11/16 in pan head screws | Passivated 18-8 stainless steel pan head Phillips, 8-32 thread, 11/16 in long — PN 91772A523 | McMaster-Carr | 4 | ~$0.20–$0.35 ea | ~$0.80–$1.40 | [McMaster-Carr 91772A523 [11]](https://www.mcmaster.com/91772A523/) |
| PETG-CF filament | Prototype sensor mounts and brackets | SUNLU | 1 kg | ~$19.99 | ~$19.99 | [SUNLU — PETG-CF Filament [12]](https://store.sunlu.com/products/petg-cfpetg-carbon-fiber-3d-printer-filament-1kg) |
| Carbon-fiber nylon filament | Final rigid sensor mounts, if used | MatterHackers NylonX PA12 | 0.5 kg | ~$63.00 | ~$63.00 | [MatterHackers — NylonX PA12 [13]](https://www.matterhackers.com/store/3d-printer-filament/nylonx-carbon-fiber-nylon-filament-1.75mm) |
| TPU 95A filament | 3D-printed landing gear legs (4×), Shore hardness 95A | SUNLU | 1 kg | ~$19.99 | ~$19.99 | [SUNLU — TPU 95A Filament [14]](https://store.sunlu.com/products/moq-3kg-tpu-3d-printer-filament-1kg) |

**Estimated frame subsystem total:**
- Minimum estimated cost: ~$105 (with PETG-CF sensor mounts, TPU 95A landing gear, and all McMaster-Carr fasteners)
- Higher-performance estimate (with NylonX carbon-fiber nylon mounts): ~$320–$350

---

## Analysis of Crucial Design Decisions

### H-Frame Selection
The H-frame was selected because it provides a long central body plate and sufficient mounting room for the battery, flight controller, and acoustic measurement hardware. Compared to a compact X-frame, the H-frame offers better packaging flexibility and easier access to mounted components.

### Size Selection
The measured frame footprint of 14.142 in × 14.142 in with a 20.000 in motor-to-motor diagonal provides enough physical space for the selected 13 in propellers while keeping the frame compact. This helps reduce weight and improves flight efficiency.

### Center Plate Design
The approximately 14 in × 4 in central body plate gives the frame enough length for battery placement and electronics mounting while limiting excess material. The narrow center body supports the goal of reducing weight while maintaining adequate usable mounting area.

### Arm Length
The approximately 5.898 in arm length keeps the motor mounts far enough from the center body to provide propeller clearance while avoiding unnecessary extra material. Shorter arms reduce frame mass and may improve arm stiffness.

### Vertical Spacing
The 2.250 in top-to-bottom spacing provides clearance for electronics, wiring, and landing protection. This spacing also helps protect the lower-mounted components during touchdown.

### Material Selection
Carbon fiber or carbon-fiber-reinforced material is used because it provides high stiffness at low mass. This is important because the aircraft must carry sensors, battery mass, and propulsion hardware while maintaining reasonable flight time.

### Landing Gear Material Selection (TPU 95A)
The landing gear legs are 3D-printed from TPU 95A (thermoplastic polyurethane, Shore hardness 95A). This material was selected because it provides a combination of structural integrity and elastic deformation that is well-suited for absorbing touchdown impact loads. TPU 95A is firmer than softer TPU variants such as 90A, meaning it holds the leg geometry under the weight of the aircraft while still compressing slightly on contact to absorb shock. Its abrasion resistance and rebound characteristics make it durable across repeated landings. Printing the landing gear from TPU 95A eliminates the need for separate rubber feet or damping inserts, simplifying the assembly while meeting the protection requirement for underside components. SUNLU TPU 95A filament is used as the source material at approximately $19.99 per kilogram [14].

---

## Detailed Shall Statements

### Functional Requirements
1. The frame subsystem shall support all propulsion, power, control, sensing, and communication components.
2. The frame subsystem shall maintain a stable H-frame geometry during takeoff, hover, autonomous flight, and landing.
3. The frame subsystem shall provide a central mounting area for the battery, flight controller, and onboard electronics.
4. The frame subsystem shall support the acoustic sensing hardware without interfering with propeller operation.

### Dimensional Requirements
1. The frame subsystem shall have an overall footprint of approximately 14.142 in × 14.142 in.
2. The frame subsystem shall have a motor-to-motor diagonal distance of approximately 20.000 in.
3. The frame subsystem shall include a center body plate area of approximately 14.000 in × 4.000 in.
4. The frame subsystem shall use arms approximately 5.898 in long from the center body transition to the motor mounting region.
5. The frame subsystem shall provide approximately 2.250 in of top-to-bottom spacing.
6. The frame subsystem shall include TPU 95A landing gear legs of approximately 5.931 in in length to provide ground clearance for all underside components.

### Mechanical Requirements
1. The frame subsystem shall maintain sufficient stiffness to prevent excessive arm deflection during flight.
2. The frame subsystem shall support a total aircraft mass of at least 2.3 kg.
3. The frame subsystem shall provide secure mounting holes for four motors.
4. The frame subsystem shall provide mounting points for standoffs, landing supports, and component brackets.
5. The frame subsystem shall provide adequate clearance for propeller rotation.
6. The TPU 95A landing gear shall absorb landing impact loads and prevent direct hard contact between the frame body and the ground.

### Weight Requirements
1. The frame subsystem shall minimize mass to reduce battery power consumption.
2. The frame subsystem shall target a frame mass near 500 g.
3. The frame subsystem shall avoid unnecessary material in non-load-bearing areas.

### Integration Requirements
1. The frame subsystem shall allow the battery to be mounted near the aircraft center of mass.
2. The frame subsystem shall allow the flight controller to be mounted near the geometric center of the aircraft.
3. The frame subsystem shall provide routing paths for motor, ESC, battery, and sensor wiring.
4. The frame subsystem shall support removable or replaceable sensor mounts.
5. The frame subsystem shall allow access to screws, standoffs, and electronics for maintenance.
6. The TPU 95A landing gear legs shall be removable and replaceable without disassembling the main frame structure.

### Safety and Validation Requirements
1. The frame subsystem shall be inspected before each flight for loose screws, cracks, or damaged mounting points.
2. The TPU 95A landing gear legs shall be inspected before each flight for tears, permanent deformation, or detachment.
3. The frame subsystem shall be validated through a static load test before flight.
4. The frame subsystem shall be validated through hover testing to observe vibration, flexing, and stability.
5. The frame subsystem shall prevent mounted components from contacting the ground during normal landing, using the TPU 95A landing gear legs as the primary ground contact point.
6. The frame subsystem shall avoid sharp exposed edges that could injure users during handling.

---

## References

[1] COYOUCO, "Carbon Fiber Sheet 400×500mm, 3K Twill Weave, 3mm Thickness," Amazon.com. [Online]. Available: https://www.amazon.com/COYOUCO-Carbon-Fiber-Surface-Sheets/dp/B0DLB6F3Z6. [Accessed: Apr. 2025].

[2] ImpulseRC, "M3 Standoff (Pick Your Length) — Knurled 7075 Aluminum, 10–35 mm options," MyFPVStore.com. [Online]. Available: https://www.myfpvstore.com/extras-and-hardware/impulserc-m3-standoff-pick-your-length/. [Accessed: Apr. 2025].

[3] McMaster-Carr, "Zinc-Plated Steel Phillips Flat Head Screw, M2.5 × 0.45 mm Thread Size, 10 mm Long — PN 91420A020," McMaster-Carr.com. [Online]. Available: https://www.mcmaster.com/91420A020/. [Accessed: Apr. 2025].

[4] McMaster-Carr, "Zinc-Plated Steel Phillips Flat Head Screw, M2.5 × 0.45 mm Thread Size, 12 mm Long — PN 91420A022," McMaster-Carr.com. [Online]. Available: https://www.mcmaster.com/91420A022/. [Accessed: Apr. 2025].

[5] McMaster-Carr, "M2.5 × 0.45 mm Hex Nylon-Insert Locknut (Nylock), Zinc-Plated," McMaster-Carr.com. [Online]. Available: https://www.mcmaster.com/products/nylock/thread-size~m2-5/nut-type~hex/locking-type~nylon-insert/. [Accessed: Apr. 2025].

[6] McMaster-Carr, "Zinc-Plated Steel Phillips Flat Head Screw, M4 × 0.7 mm Thread Size, 20 mm Long — PN 91420A228," McMaster-Carr.com. [Online]. Available: https://www.mcmaster.com/91420A228/. [Accessed: Apr. 2025].

[7] McMaster-Carr, "Medium-Strength Steel Nylon-Insert Locknut, Class 8, Zinc Plated, M4 × 0.7 mm, 5 mm High — PN 90576A103," McMaster-Carr.com. [Online]. Available: https://www.mcmaster.com/90576A103/. [Accessed: Apr. 2025].

[8] McMaster-Carr, "Corrosion-Resistant 18-8 Stainless Steel Female Threaded Hex Standoff — PN 91115A827," McMaster-Carr.com. [Online]. Available: https://www.mcmaster.com/91115A827/. [Accessed: Apr. 2025].

[9] McMaster-Carr, "316 Stainless Steel Hex-Drive Flat Head Screw, 82° Countersink Angle, 8-32 Thread Size, 9/16 in Long — PN 90585A134," McMaster-Carr.com. [Online]. Available: https://www.mcmaster.com/90585A134/. [Accessed: Apr. 2025].

[10] McMaster-Carr, "Low-Strength Steel Nylon-Insert Locknut, Thin-Profile, Zinc-Plated, 8-32 Thread Size — PN 90633A009," McMaster-Carr.com. [Online]. Available: https://www.mcmaster.com/90633A009/. [Accessed: Apr. 2025].

[11] McMaster-Carr, "Passivated 18-8 Stainless Steel Pan Head Phillips Screw, 8-32 Thread Size, 11/16 in Long — PN 91772A523," McMaster-Carr.com. [Online]. Available: https://www.mcmaster.com/91772A523/. [Accessed: Apr. 2025].

[12] SUNLU, "PETG-CF (PETG Carbon Fiber) 3D Printer Filament, 1.75mm, 1 kg, 10% Carbon Fiber Reinforced," SUNLU Online Store. [Online]. Available: https://store.sunlu.com/products/petg-cfpetg-carbon-fiber-3d-printer-filament-1kg. [Accessed: Apr. 2025].

[13] MatterHackers, "NylonX Carbon Fiber PA12 Filament, 1.75mm, 0.5 kg — 20% Chopped Carbon Fiber, Engineering Grade," MatterHackers.com. [Online]. Available: https://www.matterhackers.com/store/3d-printer-filament/nylonx-carbon-fiber-nylon-filament-1.75mm. [Accessed: Apr. 2025].

[14] SUNLU, "TPU 95A / TPU 90A Flexible 3D Printer Filament, 1.75mm, 1 kg — Shore Hardness 95A, Dimensional Accuracy ±0.02 mm," SUNLU Online Store. [Online]. Available: https://store.sunlu.com/products/moq-3kg-tpu-3d-printer-filament-1kg. [Accessed: Apr. 2025].
