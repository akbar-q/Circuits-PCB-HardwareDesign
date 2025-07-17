# NiMH Battery Charger Project

## Introduction
The NiMH Battery Charger project is designed to provide a simple yet effective solution for trickle charging NiMH batteries. Trickle charging is a safe and reliable method where batteries are charged at a constant low current, allowing excess energy to dissipate as heat without damaging the cells. This project is not only functional but also serves as a showcase for good PCB layout practices and modular design.

## Design Philosophy
The design prioritizes simplicity, modularity. By focusing on essential components and allowing selective population of parts, the charger can be tailored to specific needs. The modular approach enables tracking up to 15 cells across three boards, with only the main board containing a microcontroller to reduce costs.

## Schematic Design Process
The schematic design was approached systematically, ensuring each section contributes to the overall functionality and reliability of the charger.

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