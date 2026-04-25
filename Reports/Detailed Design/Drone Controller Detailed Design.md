# Detailed Design

This document delineates the objectives of the operator controller subsystem for a custom acoustics measurement drone. Upon reviewing this design, the reader should have a clear understanding of:

- How the controller subsystem integrates within the broader drone solution
- The constraints and specifications relevant to the controller subsystem
- The rationale behind each crucial design decision
- The intended architecture and operational role of the controller subsystem


## General Requirements for the Document

The document includes:

- Explanation of the subsystem’s integration within the overall solution
- Detailed specifications and constraints specific to the subsystem
- Synopsis of the suggested solution
- Interfaces to other subsystems
- 3D models of customized mechanical elements
- A buildable diagram placeholder
- A Printed Circuit Board (PCB) design layout placeholder
- An operational flowchart placeholder
- A high-level component list for the subsystem
- Analysis of crucial design decisions



## Function of the Subsystem

The controller subsystem serves as the primary interface for the custom acoustics measurement drone. Its role is to allow the operator to view basic system information, issue pilot inputs, select supervisory operating modes, and command the broader system in a controlled and deliberate manner [1][2]. Within the overall solution, the controller is not the airborne flight computer; instead, it is the ground-side operator interface that gathers user input through joysticks, buttons, switches, and a graphical display, then presents those inputs to the larger drone system architecture for execution by the onboard avionics and autonomy subsystems.

For the acoustics measurement mission, the controller supports the field operator by combining three functions into one device:

1. **Input collection:** It captures joystick and switch commands, menu selections, and supervisory mode changes.
2. **Feedback presentation:** It displays information on a compact 5-inch HDMI capacitive touchscreen selected for compatibility with the Raspberry Pi Zero 2 W platform [2][3].
3. **Mission supervision:** It provides a dedicated physical mode selector to distinguish between manual operator control and autonomous mission behavior at the system level, allowing the team to treat mode management as a deliberate human-factor design feature rather than a hidden software-only function.

The controller therefore acts as a dedicated operator console tailored to the acoustics measurement drone rather than as a generic consumer game controller. Its design emphasizes readability, portability, low mass, self-contained battery operation, and direct access to supervisory functions.


## Specifications and Constraints

The controller subsystem is governed by electrical, mechanical, human-factors, and project-integration constraints.

### Electrical and Interface Constraints

- The core computing platform is the Raspberry Pi Zero 2 W, which provides a compact form factor, 5 V DC power input, mini HDMI video output, USB OTG connectivity, and a 40-pin GPIO header suitable for SPI and digital input expansion [1].
- The selected display is an HDMI touchscreen with 800×480 hardware resolution and single-touch capability, described as driver-free on Raspberry Pi platforms by the vendor [2].
- Because the Raspberry Pi Zero 2 W does not provide general-purpose analog input channels, analog controls must be interfaced through an external ADC rather than directly through the Pi GPIO header [1].
- The subsystem uses a perfboard-based helper board approach rather than a custom fabricated controller PCB in the current design phase. The chosen board is a 2 in × 2 in prototyping perfboard, which imposes a significant spatial constraint on wiring density, component placement, and connector routing [4].
- The selected portable power source is a 5,000 mAh, 15 W USB battery bank with integrated LCD status display and built-in consumer protection features, which constrains the controller to USB-based internal power distribution rather than a custom battery-management circuit [5].

### Mechanical and Packaging Constraints

- The controller must fit within a custom enclosure sized around a 5-inch display, dual joystick inputs, momentary buttons, bumper buttons, and supervisory switches.
- The mechanical arrangement must accommodate short internal USB power cables, a mini-HDMI to HDMI display interconnect, internal soldered wiring, and an active cooler assembly without interfering with the operator grip or control layout [1][2].
- The small perfboard footprint requires that the helper board function primarily as a compact signal-conditioning and distribution board rather than a large all-in-one interconnect backplane [4].

### Human-Factors Constraints

- The controller must present an intuitive layout for two-hand operation.
- Distinct functions such as menu navigation, selection, supervisory mode switching, and power switching must remain physically distinguishable to reduce operator error.
- The design includes a dedicated manual/autonomous selector switch rather than burying that state change in a software-only menu, improving mode awareness during field use.

### Standards, Ethics, and Socio-Economic Constraints

- The subsystem must be documented in a manner appropriate for engineering review, with traceable references, explicit subsystem boundaries, and clear integration descriptions.
- The design should prioritize operator awareness, deliberate mode transitions, and maintainable wiring practices to reduce avoidable integration errors in a student-built system.
- The project must remain cost-conscious and use accessible prototyping hardware where reasonable, which is reflected in the selection of a Raspberry Pi Zero 2 W, perfboard prototyping, tactile switch assortments, and a consumer USB battery bank [1][4][5][6][7][8].

These constraints may be revised by the team if additional testing, enclosure changes, or system integration results require modification.


## Overview of Proposed Solution

The proposed solution is a custom handheld drone controller built around a Raspberry Pi Zero 2 W with a dedicated helper board that organizes analog inputs, digital inputs, and local power/signal distribution.

At the center of the subsystem is the Raspberry Pi Zero 2 W, chosen because it combines low mass, low volume, graphical display support, USB and HDMI interfaces, and a large GPIO header in a compact package [1]. The Pi interfaces to a 5-inch HDMI capacitive touchscreen selected specifically because it uses HDMI rather than DSI and is therefore better aligned with the Zero 2 W’s native mini HDMI video output [1][2].

Because the controller must read joystick positions, the design incorporates an external analog-to-digital conversion stage on a perfboard helper board. The helper board contains the MCP3008 ADC, local decoupling, and signal routing for the analog joystick axes. The analog trigger inputs and joystick pushbuttons have been intentionally deferred to a future implementation in order to simplify the first complete controller revision.

Digital inputs are provided through momentary tactile switches used for face buttons, D-pad directions, menu/select buttons, and bumper buttons. The selected tactile switch assortment provides a low-cost, compact switch family suitable for this type of distributed operator-input layout [7]. A panel-mounted rocker switch is used as the main power switch, while a panel-mounted SPDT toggle switch is used as the manual/autonomous supervisory mode selector [8][9].

The power architecture is intentionally simple. A USB battery bank supplies 5 V power internally through short USB cables to the Pi and display. This avoids the need for a custom charging board, custom lithium battery protection circuitry, or onboard battery state estimation. The selected power bank already includes an LCD status display and consumer protection features, allowing live battery status to be assessed at the power source itself [5].

Thermal management is provided by an active cooler assembly mounted on the Raspberry Pi Zero 2 W, supplemented by enclosure-level airflow and mechanical accommodation in the CAD model. This approach is consistent with the Raspberry Pi Zero 2 W product guidance that the board should be used in a ventilated environment [1].

Overall, the proposed solution fulfills the subsystem requirements by combining:

- local analog input acquisition,
- local digital input acquisition,
- a compact human-readable display,
- portable USB-powered operation,
- and a physically explicit supervisory mode selector,

within a compact handheld controller tailored to the acoustics measurement drone mission.


## Interface with Other Subsystems

The controller subsystem interfaces with the broader solution through electrical, informational, and human-command pathways.

### Interface with the Operator

**Inputs to the controller from the operator:**
- Two joystick axes per joystick module (four analog axes total in the current revision) [10]
- Four face buttons [7]
- Four directional pad buttons [7]
- Two bumper buttons [7]
- One select button [7]
- One menu button [7]
- One main power switch [9]
- One manual/autonomous supervisory mode selector [8]
- Single-touch display interaction [2]

**Outputs from the controller to the operator:**
- Visual system feedback on the 5-inch display [2]
- Battery status indication at the battery bank LCD [5]
- Software-defined mode/status information on the screen

### Interface with the Drone Avionics and Mission Subsystems

At the system level, the controller provides operator intent and supervisory mode state to the broader drone architecture. In conceptual subsystem terms, the outbound controller data includes:

- operator axis commands,
- discrete operator button states,
- menu and mode-selection commands,
- and explicit supervisory mode selection state.

The inbound information returned to the controller for presentation may include:

- mission state,
- aircraft status,
- operator prompts,
- payload or survey progress,
- and health or warning messages.

The exact transport and data framing are handled by the broader communications and avionics architecture and are therefore outside the implementation scope of this controller subsystem report.

### Internal Electrical Interfaces

**Raspberry Pi Zero 2 W ↔ Display**
- Video output via mini HDMI from the Pi to the HDMI display [1][2]
- Touch input returned through USB connection to the Pi [1][2]
- Power supplied independently via internal USB power cable [5]

**Raspberry Pi Zero 2 W ↔ Helper Board**
- 3.3 V distribution for ADC and low-voltage logic reference [1]
- Ground reference shared across Pi, helper board, and control devices
- SPI interface between Pi and ADC
- GPIO lines for digital buttons and switches

**Helper Board ↔ Controls**
- Analog joystick voltage signals routed to ADC input channels [10]
- Digital button states routed as active-low inputs with software pull-ups
- Supervisory toggle and power switch states routed according to final enclosure implementation


## 3D Model of Custom Mechanical Components

![ControllerOverview](Images\ControllerOverview.png)

![ControllerFrontView](Images\ControllerFrontview.png)
Spaces in the controller for buttons and joysticks. and 2 spaces beside the screen clearence for future implementation.

![ControllerTopandBackView](Images\ControllerTopandBackView.png)
Spaces for bumper buttons and future implementations for triggers. Spacing for buttons that are attached to the screen, as well as spacing for the power switch (right) and manual control switch (Left) Removable back panel (Blue) for easy internal access.

![ControllerInternalView](Images\ControllerInternalView.png)
Spaces reserved for battery (Yellow), Microcomputer (orange), Cooling System (Blue), and Helper Board (Green).

The CAD package for the controller enclosure demonstrates ergonomic control placement, internal component clearances, cable routing space, airflow vents for the active cooler, and the spatial relationship between the perfboard helper board and operator controls.


## Buildable Schematic

![Controller Wiring Diagram](Images\Controller_Wiring_Diagram.png)

The schematic image shows the Raspberry Pi Zero 2 W header connections, the MCP3008 ADC placement on the Perfboard helper board, decoupling capacitor placement, analog joystick signal routing, digital switch routing, shared ground distribution, and reserved future channels. The already prepared perfboard-style helper-board wiring image should be placed in this section.


## Printed Circuit Board Layout

None Used for this Subsystem


## Flowchart

![Drone Operational flowchart](Images/drone_operational_flowchart_v2-1.png)


## BOM

The current controller subsystem has the following confirmed high-level component set. Detailed institutional purchasing data may be appended by the team in a controlled procurement sheet.

| Item | Component | Manufacturer / Source | Part / Model | Qty. | Price | Notes |
|---|---|---|---|---|---|---|
| U1 | Single-board computer | Raspberry Pi | Raspberry Pi Zero 2 W | 1 | $15 | Core controller computer [1] |
| DS1 | Display | Waveshare / PiShop | 5inch HDMI LCD (H), 800×480 capacitive | 1 | $53 | HDMI touchscreen display [2] |
| BRD1 | Helper board | SchmalzTech | 2" × 2" Perfboard, ST-PERF-2-2 | 1 | $4.25 | Perfboard for ADC and signal routing [4] |
| PWR1 | Portable power source | Energizer / Best Buy | UE5035C, 5,000 mAh, 15 W USB-C power bank | 1 | $15 | Internal USB-powered system source [5] |
| SW1-SW12 | Momentary tactile switches | TWTADE / Amazon | 260-piece tactile switch assortment | 1 | $14 | Used for face buttons, D-pad, menu/select, bumpers [7] |
| SW13 | Supervisory mode selector | E-Switch / DigiKey | 100SP1T1B4M2QE | 1 | $2.66 | SPDT toggle switch used for manual/autonomous supervisory selection [8] |
| SW14 | Main power switch | E-Switch / DigiKey | RA1113112R | 1 | $0.65 | SPST rocker main power switch [9] |
| J1-J2 | Analog joystick modules | Tinkersphere | PS4 Thumb Joystick with Click Button | 2 | $8 | Analog joystick modules; click feature reserved for future implementation [10] |
| U2 | ADC | Microchip | MCP3008 | 1 | $3.12 | External ADC for joystick acquisition |
| C1 | Decoupling capacitor | Generic | 0.1 µF ceramic capacitor | 1 | Already Owned | Local ADC supply decoupling |
| W1 | Hook-up wire | TUOFENG / Walmart | 20 AWG solid wire kit | 1 | $17 | Internal soldered wiring between helper board and controls [11] |
| CBL1 | Video interconnect | Generic | mini-HDMI to HDMI adapter/cable | 1 | $5.06 | Pi-to-display video link [1][2][12] |
| CBL2-CBL3 | Power interconnect | Generic | Short internal USB cables | 2 | $6.14 | Battery bank to Pi and display [13] |
| TH1 | Active cooler | User-selected vendor | Active cooler kit for Raspberry Pi Zero 2 W | 1 | $10 | Thermal management, mechanical locations defined in CAD [14] |
| ENC1 | 3D Printed Enclosure | Custom | Team-designed enclosure | 1 | Purchased in Frame subsystem | Mechanical integration defined by CAD |

### BOM Notes

- The analog trigger subsystem is intentionally deferred to a future implementation and is not included in the current subsystem revision.
- The joystick click-button function is also reserved for future implementation and is not assigned in the present input map.
- | Drone Controller Subsystem | Total Pricing (w/o tax) | $153.88 |


## Analysis

The selected architecture is appropriate for a custom drone operator controller because it balances capability, compactness, integration simplicity, and cost.

The Raspberry Pi Zero 2 W is a defensible controller-compute choice because it provides the required combination of compact dimensions, display support through mini HDMI, USB connectivity, and a flexible GPIO header in a single low-cost module [1]. For this controller, graphical output and configurable GPIO are more important than high computational throughput, making the Zero 2 W a suitable subsystem anchor.

The display selection is similarly justified. The chosen 5-inch Waveshare HDMI touchscreen is explicitly described as supporting Raspberry Pi systems, single-touch operation, and driver-free operation on Raspberry Pi platforms [2]. This reduces software integration burden and avoids the interface mismatch that a DSI-only display would create with the Zero 2 W. The 800×480 resolution is also appropriate for a compact handheld operator interface because it provides enough screen area for status indication, menus, and simple mission feedback while remaining mechanically compatible with a small controller enclosure [2].

The helper-board strategy is also justified. A custom PCB could eventually yield a more polished and compact design, but the use of a 2 in × 2 in perfboard at this stage supports iterative subsystem development and revision without the lead time and re-spin cost of a fabricated PCB [4]. The tradeoff is wiring density. Because the chosen perfboard is small, careful routing and the use of soldered point-to-point wiring are necessary.

The ADC decision is a direct consequence of the Pi platform’s capabilities. Since the Raspberry Pi Zero 2 W does not expose general-purpose analog inputs, an external ADC is necessary for joystick acquisition [1]. The MCP3008 is a standard, compact SPI ADC suitable for a design of this scale. The inclusion of a local 0.1 µF decoupling capacitor is also an appropriate design measure because it improves local supply stability for the ADC during switching activity.

From a control-layout perspective, the current revision is appropriately scoped. By deferring analog triggers and joystick click functions to a later revision, the team reduces early integration complexity while still preserving a controller layout that can be expanded in the future. This is a sound engineering decision because it narrows first-pass wiring complexity, reduces immediate GPIO/ADC assignment pressure, and allows the enclosure and user interface to be validated before adding less essential inputs.

The power architecture is one of the strongest decisions in the design. Instead of creating a custom lithium battery subsystem, the team selected a commercial USB battery bank rated at 5,000 mAh and 15 W with a built-in LCD and protection features [5]. This reduces electrical risk, removes the need for custom cell protection design, and aligns well with the selected Pi and display, both of which can be powered conveniently from 5 V USB paths [1][2][5]. The decision to use short internal USB cables for power distribution is also consistent with this architecture and simplifies serviceability.

The switch strategy is also reasonable for a student-built custom controller. Tactile switches are inexpensive, compact, and readily distributed around a custom front panel [7]. A dedicated SPDT toggle switch for supervisory mode selection improves operator awareness by making system mode physically visible and immediately accessible [8]. A dedicated rocker switch for system power similarly supports straightforward operator use and enclosure-level control [9].

The joystick selection is appropriate for a prototype custom controller because the chosen modules already provide dual-axis analog output in a compact form factor compatible with Raspberry Pi and Arduino projects [10]. Even though the modules include a click switch, reserving that feature for a later revision avoids overcomplicating the first fully integrated controller version.

The thermal solution is justified by packaging realities. The Raspberry Pi Zero 2 W documentation indicates that the board should be operated in a ventilated environment [1]. Once installed in a controller enclosure near a display and internal wiring, passive convection may not be sufficient under all operating conditions. An active cooler, paired with enclosure airflow accommodation in the CAD model, is therefore a sensible risk-reduction measure.

Overall, the subsystem design should satisfy its intended function as a custom handheld controller for the acoustics measurement drone. The design supports operator input capture, feedback display, supervisory control, portable self-powered operation, and realistic student-team fabrication while keeping several optional features appropriately deferred to future revisions.


## References

[1] Raspberry Pi Ltd., https://www.raspberrypi.com/products/raspberry-pi-zero-2-w/ . Used for Zero 2 W power, interface, and platform characteristics. Available from Raspberry Pi documentation.

[2] PiShop / Waveshare, https://www.pishop.us/product/5inch-hdmi-lcd-h-display-800x480-capacitive/?searchid=0&search_query=HDMI+display . Used for display resolution, interface type, Raspberry Pi compatibility, and touch support.

[3] Raspberry Pi Ltd., https://www.raspberrypi.com/products/raspberry-pi-zero-2-w/ . Used for general platform context.

[4] SchmalzTech, https://www.schmalztech.com/products/2-x-2-perfboard?variant=41104997810353&country=US&currency=USD&utm_medium=product_sync&utm_source=google&utm_content=sag_organic&utm_campaign=sag_organic&gad_source=1&gad_campaignid=17510350240&gclid=CjwKCAjwqazPBhALEiwAOuXqdFzJ6_SEnYtjTMc3QHrWzrQJpOttZcJ4Rt3oivcgZCnfYlquOz-fdhoC5HgQAvD_BwE . Used for helper-board size and prototyping-board selection.

[5] Best Buy / Energizer, https://www.bestbuy.com/product/energizer-max-5000mah-15w-usb-c-3-port-universal-portable-battery-charger-power-bank-w-lcd-screen-for-smartphones-accessories-black/J36C4YXKWS/sku/6589873?utm_source=feed&extStoreId=172&ref=212&loc=21202115495&gclsrc=aw.ds&gad_source=4&gad_campaignid=21202120766&gclid=CjwKCAjwqazPBhALEiwAOuXqdGwC6kUtJMvIarNreMOcdhoHZc4Q-_esTaZBqXpgqFK39k3YCICfkRoC0qIQAvD_BwE . Used for portable power source capacity, wattage, and battery-bank selection.

[6] Microsoft, https://www.microsoft.com/en-us/d/replacement-buttons-for-xbox-wireless-controller/8ZX7Z1R45KKQ/K2P2?OCID=AIDcmm6mu07qw1_seo_omc_goo&source=googleshopping&activetab=pivot:overviewtab . Used earlier in design evaluation to distinguish cosmetic controller parts from electrical switch components.

[7] Amazon / TWTADE, https://www.amazon.com/TWTADE-260pcs-Momentary-SwitchTactile-Assortment/dp/B086BYPH79/ref=sr_1_4?dib=eyJ2IjoiMSJ9.YiK0xw3HhQQ8JrRO5vn0D_uFPQedATflBhdBz3ZTgRzkqrWfi6CaNJvRqPfULngdKuz8K2ebqPnswZUkqunZMTy6Eko7drOGXETXRm9xKBgs_26vB-Zm9bxQhiP_jT9ZyPV21FOyJrdp40sLFtOsXIpnXDqX7K5nSZ3tR7IHv2Y9hkaPGr5gnKrBX15kthd9vkGVRKDhyoNVhkWWjQKly1GVjkh5JjZsGI53pcbuto8.ONEX5RY_JWiE2K0guHDBxj-7mblixrE9xyrCpZDXstI&dib_tag=se&keywords=buttons%2Belectronics&qid=1777090973&sr=8-4&th=1 . Used for selected tactile switch family.

[8] DigiKey / E-Switch, https://www.digikey.com/en/products/detail/e-switch/100SP1T1B4M2QE/378824?gclsrc=aw.ds&gad_source=1&gad_campaignid=20504615652&gclid=CjwKCAjwqazPBhALEiwAOuXqdGFH9fdn7cJ6GLsz7rnZBOre3oF-5V8MKM9agUen592bKCrXvL_XpRoCce4QAvD_BwE . Used for supervisory mode selector identification.

[9] DigiKey / E-Switch, https://www.digikey.com/en/products/detail/e-switch/RA1113112R/3778055?gclsrc=aw.ds&gad_source=1&gad_campaignid=20504615652&gclid=CjwKCAjwqazPBhALEiwAOuXqdMDirCPAnjKP-Y7A6ck-4w05lwJiU5TrZuJRBB_EazA6qwOuH_CQAxoCZRcQAvD_BwE . Used for main power switch identification.

[10] Tinkersphere, https://tinkersphere.com/buttons-switches/366-thumb-joystick-with-click-button-arduino-raspberry-pi-compatible.html?srsltid=AfmBOorcEIjWHtifgJ9y1kJfkr_MM8Q-voTfMPU2xZPZqCvT6ZzT9Zxo8iE . Used for selected joystick module family.

[11] Walmart / TUOFENG, https://www.walmart.com/ip/TUOFENG-20-gauge-Solid-Wire-Solid-Wire-Kit-6-different-colored-26-Feet-spools-20-awg-Jumper-wire-Hook-up-Wire-Kit/347557210?wmlspartner=wlpa&selectedSellerId=101194961&selectedOfferId=8C0523B8BC1C381EAC919F578C2CB6A0&conditionGroupCode=1 . Used for internal wiring selection.

[12] MyCableMart / FE-HHM14-01, https://www.mycablemart.com/store/cart.php?m=product_detail&p=11440&ad_source=google_usa&gad_source=1&gad_campaignid=23469041990&gclid=CjwKCAjwqazPBhALEiwAOuXqdNm0xQqDYooqRzyJCp4S1PeSK7v9N3zdKNUi0ee_X5Cr1eYVBIYiNhoC9wQQAvD_BwE . Used to connect the Zero 2 W to the Touch Screen.

[13] Digikey / 3021059-01M, https://www.digikey.com/en/products/detail/qualtek/3021059-01M/7795309?gclsrc=aw.ds&gad_source=1&gad_campaignid=20232005509&gclid=CjwKCAjwqazPBhALEiwAOuXqdHRT8hXXG53vtq9huyp_aWrB9hwn6l8Ia6ooqDOnhOexaHedwiMqkBoCLkAQAvD_BwE . Used to connect the touch screen and Zero 2 W to the power bank.

[14] AliExpress / 3256807816523654, https://www.aliexpress.us/item/3256807816523654.html?src=google&snps=y&src=google&albch=shopping&acnt=708-803-3821&isdl=y&slnk=&plac=&mtctp=&albbt=Google_7_shopping&aff_platform=google&aff_short_key=UneMJZVf&gclsrc=aw.ds&albagn=888888&ds_e_adid=&ds_e_matchtype=&ds_e_device=c&ds_e_network=x&ds_e_product_group_id=&ds_e_product_id=en3256807816523654&ds_e_product_merchant_id=704218984&ds_e_product_country=US&ds_e_product_language=en&ds_e_product_channel=online&ds_e_product_store_id=&ds_url_v=2&albcp=20542171667&albag=&isSmbAutoCall=false&needSmbHouyi=false&gad_source=1&gad_campaignid=18545443176&gclid=CjwKCAjw46HPBhAMEiwASZpLRGc_tOF_30Og6DcfSNr_PqhLfAz0AV2Q3w45cMSuHi44Gbp8oOv4SRoCduIQAvD_BwE&gatewayAdapt=glo2usa#nav-specification . Colling system for Zero 2 W and by extention the controller enclosure.
