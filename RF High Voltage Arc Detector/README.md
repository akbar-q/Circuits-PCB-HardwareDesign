# High Voltage Arc Detector

**Client Project - Fiverr Delivery**  
**Version:** 1.0  
**Date:** April 2025  
**Designer:** Akbar Q Hardware Design

---

## Overview

The High Voltage Arc Detector is a non-contact RF-based sensing system that detects high-voltage electrical arcs. Using a differential antenna and fast amplification, the system captures high-frequency electromagnetic noise from arcing events and outputs a user-friendly signal. This signal can drive a buzzer and LED for alerts or be interfaced with other control systems via a latched output.

---

## Features

- **Differential Antenna Input** with ESD protection
- **10 kHz – 180 MHz Bandpass Filter** for noise immunity
- **High-Speed Amplification** using LMH6702MA
- **Passive Peak Detector** to extract signal envelope
- **Adjustable Threshold Comparator** with internal hysteresis
- **Optional 1-Second Output Pulse Generator**
- **Buzzer and LED Indicator**
- **Wide Operating Voltage Range:** 9–15V external input

---

## Hardware Overview

### 1. **Differential RF Front-End**
- **Antenna:** PCB-based, differential, optimized for broadband detection
- **ESD Protection:** ESD diodes on both inputs (D4, D5)

### 2. **Bandpass Filter**
- Twin LC π-filter tuned between ~10 kHz and 180 MHz.

### 3. **High-Speed Amplifier**
- **LMH6702MA**: Provides wideband gain with 50Ω I/O matching.
- RF output (J3) optionally accessible for oscilloscope viewing or chaining.

### 4. **Peak Detector**
- Passive envelope detector (BAT54J, C8) extracts the peak value of arc noise.

### 5. **Comparator Section**
- **LMV7219**: Single comparator with internal hysteresis.
- Comparator threshold adjustable via RV1 Potentiometer/Trimmer.

### 6. **Optional Pulse Extension**
- **Monostable (U1A - 74LVC1G123)** triggers a 1-second pulse on arc detection.
- Drives a piezo buzzer and indicator LED (D5, BZ1).
- Useful for ensuring transient arcs are not missed.

### 7. **Output Control**
- **Output Header (J4)**: Provides comparator signal, latched pulse, and power rails.
- Ready for connection to digital systems, relays, or logging modules.

---

## Files Included

| File | Description |
|------|-------------|
| `\Renders\NonContactHVDetector.png` | [Top-side PCB render] |
| `\Renders\PCB Sheet.pdf` | [Full schematic PDF] |
| `\Renders\Schematic Sheet.pdf` | [PCB layout drawing PDF] |

---

## Setup Instructions

### Power Supply
- Connect a 9–15V DC source to **J1**.
- The onboard regulators (U5, U6) generate ±5V for analog and digital sections.
- Place Holder Switch for 3v3 Operation but Client Does not Require it, No 3v3 Supply Present

### Threshold Adjustment
- Use **RV1** to set the comparator threshold.
- Start low and increase gradually while observing arc detection response.

### Output Behavior
- When an arc is detected:
  - `CompareSig` goes high.
  - If the pulse circuit is enabled, a 1-second pulse drives the buzzer and LED.
  - Use `LongPulse` to trigger downstream systems (e.g., microcontrollers, relays).

---

## Project Details

| Item | Value |
|------|-------|
| PCB Size | ~75mm x 45mm |
| Stackup | JLC04161H-7628 |
| Fabrication | JLCPCB |
| Schematic Tool | KiCad 9.0 |

---

## Customization Options

- **Bandwidth:** Change Filter R and C Values as Per Reference Table.
- **Pulse Width:** Adjust R6 or C1 in U1A section for different pulse durations.
- **RF Output (J3):** Optional, can be disconnected if not used.
- **LED/Buzzer:** Can be disabled by removing R7 or D5 for silent operation.

---

## License & Delivery

This hardware design was custom-made for a Fiverr client, it has been made public only with authorization of the Client. Commercial reuse or redistribution outside the original Fiverr transaction is prohibited unless licensed.

---

## Contact

For revisions, follow-up customizations, or future electronics designs:  
**Contact:** contact@akbarq.com  
**Portfolio:** akbarq.com

---

