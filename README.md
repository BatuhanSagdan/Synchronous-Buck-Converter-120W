# 120W Synchronous Buck Converter — 48V to 24V / 5A
 
A discrete, digitally-controlled synchronous buck converter designed for a 48V input and 24V/5A output. Built with isolated gate drivers, hall-effect current sensing, and STM32 digital control interface. Designed as a 2-layer home-etched PCB with careful attention to hot-loop minimization, ground domain separation, and EMI management.
 
> **Status:** PCB layout complete · SPICE simulation in progress · Pending fabrication and test
 
---
 
## Specifications
 
| Parameter | Value |
|---|---|
| Input Voltage | 48V DC |
| Output Voltage | 24V DC |
| Output Current | 5A (continuous) |
| Output Power | 120W |
| Topology | Synchronous Buck |
| Switching Frequency | 100 kHz |
| Inductor | 80 µH (E42-CF139 Ferrite core)  |
| Inductor Current Ripple | ~1.50 A peak-to-peak |
| Peak Inductor Current | ~5.75 A |
| RMS Inductor Current | ~5 A |
| Control | Open-loop PWM (STM32 digital control interface ready) |
| PCB | 2-layer, FR4, 1 oz copper, home-etched |

<img width="935" height="660" alt="image" src="https://github.com/user-attachments/assets/ab1c9924-db39-457f-9ff8-b5a448855a78" />
<img width="1152" height="801" alt="image" src="https://github.com/user-attachments/assets/d45ee1e9-fa01-4a9d-bded-425f3d6cda54" />
<img width="1095" height="745" alt="image" src="https://github.com/user-attachments/assets/e72c4cec-f6a2-4280-8846-346c2ed94ac5" />
<img width="1048" height="703" alt="image" src="https://github.com/user-attachments/assets/6772b3f1-d470-4cd5-a711-dc1fd9a2f039" />
<img width="1085" height="719" alt="image" src="https://github.com/user-attachments/assets/4a0cf39a-5d83-4e26-ab08-753e49bb6aa3" />
<img width="867" height="407" alt="image" src="https://github.com/user-attachments/assets/6fdd61a8-dccb-491e-ba31-361d88649a8c" />
<img width="1189" height="631" alt="image" src="https://github.com/user-attachments/assets/b887d9ea-1ab1-4b82-a004-e4b1baf01863" />
<img width="764" height="559" alt="image" src="https://github.com/user-attachments/assets/d53f1db6-ce60-4daf-a8f5-877d43684e8e" />
<img width="1249" height="370" alt="image" src="https://github.com/user-attachments/assets/59174f7e-de94-409f-b220-d58d64a7798c" />
<img width="501" height="242" alt="image" src="https://github.com/user-attachments/assets/d2c11c89-8ae5-45e9-9454-969fc2646d22" />
<img width="423" height="474" alt="image" src="https://github.com/user-attachments/assets/e8b62c4f-b1c8-4430-9ce5-f6e9d82fa04c" />
<img width="694" height="436" alt="image" src="https://github.com/user-attachments/assets/465ee49d-a47b-4903-a98a-1465a357e206" />
<img width="643" height="498" alt="image" src="https://github.com/user-attachments/assets/4a5b82fe-9d80-4026-ab20-03011697b5e4" />






 
---
 
## Block Diagram
 
```
48V_RAW ─→ [Reverse Polarity Protection] ─→ 48V_PROT ─→ [HS MOSFET Q2] ─→ SW Node
                    (IRF5210 P-FET)                        (IRF3710PBF)      │
                    (TVS 58V)                                                 │
                                                                              ├─→ [180µH Inductor] ─→ [Output Caps] ─→ Vout_OUT (24V/5A)
                                                                              │                            │
                                                              [LS MOSFET Q3] ←┘                    [HLSR-10P Current Sensor]
                                                              (IRF3710PBF)                                  │
                                                                                                     [ADC Conditioning]
                                                                                                            │
[B1212S Isolated DC-DC] ─→ 12V_ISO ─→ [TLP250H HS Driver]                                          [STM32 Interface]
                                                                                                      (H1, H2 Headers)
[12V_RAW] ─→ [TLP250H LS Driver]
```
 
---
 
## Design Highlights
 
### Three Isolated Ground Domains
- **PGND** — Power ground carrying high-current switching return paths
- **SGND** — Signal/control ground for ADC, current sensor, and STM32 interface
- **HS_GND** — Floating high-side reference tied to the switch node (high dV/dt)
 
PGND and SGND are kept fully isolated (no single-point connection) to eliminate ground loop coupling between the power stage and sensitive analog measurements.
 
### Isolated Gate Drive
- Two TLP250H optocouplers provide galvanic isolation between STM32 control signals and MOSFET gates
- High-side supply: B1212S-2WR3L isolated DC-DC converter (12V_ISO / HS_GND)
- Low-side supply: 12V_RAW / PGND with local decoupling
- 10Ω gate series resistors with 15V Zener clamps and 10kΩ pull-downs
 
### Hot-Loop Optimization
- Input HF capacitors (4×1µF + 100nF) placed adjacent to MOSFETs, not at input terminals
- Minimized loop area: Cin(+) → Q2 → SW → Q3 → PGND → Cin(−)
- Local PGND return with wide, short copper rather than long traces
- SW node copper area intentionally minimized to reduce EMI radiation
 
### Input Protection
- **Reverse polarity:** IRF5210PBF P-channel MOSFET with 12V Zener gate clamp
- **Transient suppression:** 58V TVS diode on 48V_RAW node
- **Filtering:** Bulk + HF ceramic input capacitors
 
### Current Sensing & ADC Interface
- HLSR-10P hall-effect current sensor (isolated, ratiometric output)
- Differential measurement: Vout and Vref both routed to STM32 ADC
- Voltage divider (10k/20k) + 200Ω series + 1nF NP0/C0G anti-aliasing filter per channel
- Dedicated SGND domain for analog signal integrity
 
### Snubber
- RC snubber (4.7Ω + 1nF) across low-side MOSFET to damp switch-node ringing
 
---
 
## PCB Design Decisions
 
### 2-Layer Home-Etched Constraints
- No plated vias — wire vias (0.6mm wire, 0.8mm drill) used for layer transitions
- No solder mask — increased clearance rules (0.5mm minimum)
- THT component pins serve as natural top-bottom layer connections
- Bottom layer reserved for ground management, not power routing
 
### Layer Strategy
| Layer | Purpose |
|---|---|
| Top | All power routing, switching node, gate drive, signal traces |
| Bottom | PGND polygon, selective SGND polygon, short signal crossovers only |
 
### Trace Widths
| Net | Width | Rationale |
|---|---|---|
| 48V_RAW, 48V_PROT | 2–3 mm | ~2.5A average input current |
| SW node / HS_GND | 2–3 mm, minimum area | High dV/dt — minimize copper area for EMI |
| Output path (Vout) | 4–6 mm | 5A continuous DC |
| PGND return | Wide polygon | Hot-loop closure, low impedance |
| Gate drive | 0.5 mm | Low average current, short length priority |
| 12V supplies | 1.0 mm | ~300mA auxiliary |
| Analog signals | 0.4 mm | µA-level, noise immunity priority |
| Digital control | 0.4–0.5 mm | Logic-level signals |
 
### Ground Polygon Strategy (Bottom Layer)
- **PGND polygon covers:** Input capacitor area, output capacitor area, Q3 source region, terminal areas
- **Keepout zones (no copper):** SW node area, HS_GND/isolated supply area, optocoupler footprints, analog SGND region
- **SGND:** Top-layer polygon only, confined to analog section (R11–R16, C18–C22, H2)
 
---
 
## Component List
 
### Power Stage
| Ref | Component | Package | Description |
|---|---|---|---|
| Q1 | IRF5210PBF | TO-220 | P-channel MOSFET, reverse polarity protection |
| Q2 | IRF3710PBF | TO-220 | N-channel MOSFET, high-side switch |
| Q3 | IRF3710PBF | TO-220 | N-channel MOSFET, low-side switch |
| L1 | 180 µH | Toroidal (THT) | Output inductor |
| D1 | TVS 58V | THT | Input transient suppression |
| D2 | 12V Zener | THT | Q1 gate clamp |
| D3, D4 | 15V Zener | THT | Q2/Q3 gate clamps |
 
### Gate Drive
| Ref | Component | Package | Description |
|---|---|---|---|
| U2 | TLP250H | DIP-8 | Optocoupled gate driver (high-side) |
| U3 | TLP250H | DIP-8 | Optocoupled gate driver (low-side) |
| U1 | B1212S-2WR3L | SIP | Isolated DC-DC for HS supply |
| R3, R8 | 10Ω | 0805 | Gate series resistors |
| R7, R10 | 200Ω | SMD | Opto LED current limit |
| R4, R9 | 10kΩ | SMD | Gate pull-down |
 
### Sensing & Interface
| Ref | Component | Description |
|---|---|---|
| S1 | HLSR-10P | Hall-effect current sensor |
| R11, R14 | 10kΩ | ADC voltage divider (upper) |
| R13, R16 | 20kΩ | ADC voltage divider (lower) |
| R12, R15 | 200Ω | ADC series protection |
| C19, C21 | 1nF NP0/C0G | ADC anti-aliasing filter |
| H1 | 6-pin header | STM32 control interface |
| H2 | 6-pin header | STM32 ADC interface |
 
### Capacitors
| Ref | Value | Type | Description |
|---|---|---|---|
| C1–C4 | 1µF / 100V | 1210 MLCC | Input HF decoupling |
| C5 | 100nF | MLCC | Input HF decoupling |
| C11, C12 | 470µF / 50V | Electrolytic (THT) | Output bulk |
| C13 | 100nF | MLCC | Output HF decoupling |
| C14 | 1µF | MLCC | Output decoupling |
| C7 | 2.2µF | MLCC | B1212S output |
| C6 | 2.2µF | MLCC | B1212S input |
| C8, C10 | 100nF | MLCC | Driver local decoupling |
| C16 | 22µF | Electrolytic | LS driver bulk decoupling |
| C17 | 100nF | MLCC | LS driver HF decoupling |
 
---
 
## Design Tools
 
- **Schematic & PCB:** Altium Designer 20.2.7
- **Simulation:** PSIM 2026 + LTSpice 24.1
 
---
 
## Author
 
**Batuhan Sagdan**
Electrical Engineering, 2nd Year
Yıldız Technical University (YTÜ)
 
---
 
## License
 
This project is shared for educational purposes. Feel free to reference the design decisions and methodology.







