# NiMH Battery Charger Project

## Introduction
The NiMH Battery Charger project is designed to provide a simple yet effective solution for trickle charging NiMH batteries. Trickle charging is a safe and reliable method where batteries are charged at a constant low current, allowing excess energy to dissipate as heat without damaging the cells. This project is not only functional but also serves as a showcase for good PCB layout practices and modular design.

## Design Philosophy
The design prioritizes simplicity, modularity. By focusing on essential components and allowing selective population of parts, the charger can be tailored to specific needs. The modular approach enables tracking up to 15 cells across three boards, with only the main board containing a microcontroller to reduce costs.

## Schematic Design Process
The schematic design was approached systematically, ensuring each section contributes to the overall functionality and reliability of the charger.

![Schematic Diagram](images/schematic.png)

### 1. Microcontroller Integration
The heart of the design is the ESP-WROOM-32E microcontroller, chosen for its versatility, cost-effectiveness, and extensive open-source support. Key considerations included:
- **Pin Protection**: ESDS diodes were added to safeguard the microcontroller pins against electrostatic discharge.
- **Power Supply**: A regulated 3.3V supply was designed to ensure stable operation.
- **Communication**: UART functionality was incorporated for firmware flashing and debugging.
- **IoT and Web Server Support**: The ESP32 was selected for its ability to host web servers, making it ideal for IoT applications. Its open-source ecosystem simplifies development and integration of web-based monitoring and control features.

### 2. Power Conversion
Reliable power conversion is critical for the charger's operation. The design includes:
- **Voltage Regulators**: AMS1117 was selected for their proven performance. Two of them have been used to split the current load, party done due to them being in my personal stock from other projects. 
- **Filtering**: Capacitors and resistors were strategically placed to filter noise and stabilize the power supply.

### 3. USB-C Power Input
To ensure compatibility with modern power sources, a USB-C receptacle was integrated. Design considerations included:
- **Shield Pad Limiter**: Added for grounding and protection.
- **Filtering Capacitors**: Used to smooth the input power and reduce noise.

### 4. Current Limiting and Charging
The core functionality of the charger lies in its ability to safely charge batteries. This was achieved through:
- **CC Resistors**: These resistors set the constant current for trickle charging, ensuring the charging rate is slow enough to prevent overheating.
- **Current Limit Resistors**: Additional resistors were included to cap the maximum current supplied to the batteries.

### 5. Battery Monitoring and Expansion
To enhance functionality and scalability, the design includes:
- **Remote Pins**: Connectors on either side of the PCB allow additional boards to be added, enabling tracking of up to 15 cells where each of the 2 additional PCBs add support for 5 cells.
- **Sense Resistors**: Used to measure the voltage across each cell, providing data for monitoring and ensuring safe operation.

### 6. Selective Component Population
Acknowledging the project's focus on trickle charging, the schematic allows for selective population of components. For basic functionality, only the voltage regulator, USB port, CC resistors, and current limit resistors need to be populated. This flexibility reduces costs and simplifies assembly.

### Known Caveats
While the modularity feature adds scalability to the design, an alternative approach could have been considered to enhance functionality. Instead of using remote pins for expansion, the ADC channels of the ESP32 could have been utilized to differentially measure the voltage across the current-sense resistors. This would allow:
- **Current Flow Measurement**: By measuring the current flow, the charger could detect the characteristic dip in current that occurs when a NiMH battery is fully charged.
- **Improved Monitoring**: Unlike Li-ion batteries, which transition from constant current (CC) to constant voltage (CV) charging, NiMH batteries do not cease drawing current when fully charged. Monitoring the current flow provides a more accurate indication of charge completion.

Instead, the current design relies on voltage measurements. When no battery is connected, the voltage at the ADC input is nearly 3V. When a battery is plugged in, the voltage drops to a lower value due to the resistor at the battery's positive terminal. If the battery voltage exceeds a certain threshold, it is determined to be nearly fully charged. At this point, a timer can be started to wait for the battery to reach full charge. 

Experimental testing will be required to study the behavior of various battery cells. Multiple brands and capacities of cells are lined up for these tests. Additionally, incorporating transistors or power-disable capable voltage regulators could have been helpful to measure the raw voltage directly at the battery terminal, further improving accuracy.

## PCB Design
The PCB design is a showcase of both practical engineering and creative expression. It is a 2-layer board, with copper tracks running horizontally on the front and vertically on the back. This orthogonal layout is a common and effective strategy in digital and complex circuits, as it allows for clean routing of buses and makes it easy to use vias to jump between layers when a bend is needed. Tracks are routed as buses, with multiple nets grouped together for similar functions, such as the expansion port runs.

The power planes are intentionally split: there is a ground plane, a 3.3V digital plane for the microcontroller, and a separate 3.3V power plane for supplying the batteries. While this might seem less ideal for noise, in this slow-signal, non-critical application, it is perfectly acceptable. The cost savings from using a 2-layer PCB instead of a 4-layer one are substantial—nearly fourfold—and for a battery charger, a 4-layer board would be overkill. This demonstrates that resource constraints did not compromise the methodology or quality of the layout.

### Copper Layers
- **Front Copper**: Routed horizontally, forming the basis for the orthogonal bus structure.
- **Back Copper**: Routed vertically, complementing the front and enabling efficient use of vias for layer transitions.
- **Power Planes**: As described above, the split planes are a deliberate, practical choice for this application.

### Silkscreen

#### Front Silkscreen
The front silkscreen is simple but meaningful. The only decorations are:
- Outlines around the LEDs, showing positive and negative sides, replacing the usual dot or line indicators.
- A plus icon to indicate the positive side of the battery holder.
- Central branding: "5+" (the 5 for the number of cells the board can hold, and the "+" for expandability), a right-to-repair logo (a movement I genuinely support and encourage others to as well), and a battery reuse icon.
- A mask cutout for the Ni atomic number, which appears shiny and provides strong contrast against the rest of the board and silkscreen.
- A turtle icon on the far right, representing the slow, steady nature of trickle charging.

#### Back Silkscreen
The back silkscreen is heavily decorated and, in my opinion, beautiful:
- The top is all functional: my name is on the left, below that an outline and text indicating the 7-segment display is on the opposite side.
- There is a playful Shin Chan reference (I love the show, especially the adult-oriented Funimation English dub).
- The top middle has a spot to mark whether the board is a slave or master (if the lack of ESP wasn't obvious enough for a slave), and below that, space for a serial number.
- The right side provides information about the typical charging current.
- The USB CC pins are outlined, with a white bar to show ground.
- Below the functional area, the current-limiting resistors are depicted as cars speeding down a two-lane road, complete with road outlines, markings, a zebra crosswalk, and "speed trails" behind the resistors.
- At the bottom edge, a cityscape is featured, with the voltage regulator forming the top of one of the buildings.
- The PCB is dotted to give a starry night effect, a motif I have used in previous projects.

Yes, the silkscreen is excessive, but I am very proud of it. It is a testament to the creativity and personality that human designers bring to hardware—something AI will never take away.

### Mask Layer
The front mask layer is also used creatively. There are cutouts, such as for the Ni atomic number, which allow the copper to shine through and provide a striking visual contrast. This use of the mask for graphics adds another layer of visual interest to the board.

### PCB Images
- **Front Copper**: ![Front Copper](images/front_copper.png)
- **Back Copper**: ![Back Copper](images/back_copper.png)
- **Front Silkscreen**: ![Front Silkscreen](images/front_silkscreen.png)
- **Back Silkscreen**: ![Back Silkscreen](images/back_silkscreen.png)
- **Front Mask**: ![Front Mask](images/front_mask.png)

## Design Highlights
- **Modularity**: The ability to expand the system to track more cells makes the design versatile and cost-effective.
- **Safety**: Protection diodes and current limiting resistors ensure safe operation.
- **Efficiency**: The use of proven components like AMS1117 and LM317 guarantees reliable performance.
- **Simplicity**: The design is straightforward, making it accessible for hobbyists and professionals alike.

## Next Steps
This README will be expanded to include detailed chapters on:
1. **PCB Design**: Covering layout considerations, routing strategies, and manufacturing guidelines.
2. **Programming**: Explaining firmware development, microcontroller configuration, and data monitoring.

By documenting the design process in detail, this project aims to serve as a valuable resource for anyone interested in electronics design and battery charging solutions.