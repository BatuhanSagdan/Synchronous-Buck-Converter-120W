# 120W Synchronous Buck Converter — 48V to 24V / 5A

A discrete, digitally-controlled synchronous buck converter delivering 120W from a 48V input to a regulated 24V/5A output. Built with isolated gate drivers, hall-effect current sensing, and STM32 control interface. Designed end-to-end: topology, magnetics (custom-wound inductor), power stage, PCB layout, and PSIM verification.

> **Status:** Schematic & PCB layout complete · PSIM simulation verified · Calculated efficiency: 95.6% · Pending professional fabrication and lab bring-up

---

## Specifications

| Parameter | Value |
|---|---|
| Input Voltage | 48V DC |
| Output Voltage | 24V DC |
| Output Current | 5A continuous (CCM) |
| Output Power | 120W |
| Topology | Synchronous Buck (hard-switched) |
| Switching Frequency | 100 kHz |
| Inductor | 100 µH @ 0A / 81 µH @ 5A (2× T130-26 stack, custom-wound) |
| Inductor Current Ripple | ~1.5 A peak-to-peak |
| Peak / RMS Inductor Current | 5.75 A / 5.01 A |
| Calculated Efficiency | **95.6%** (5.5 W total loss) |
| Control | PI current control (PSIM-tuned), STM32 PWM @ 100 kHz |
| PCB | 2-layer, FR4, 1 oz copper, professional fabrication |


<img width="969" height="647" alt="image" src="https://github.com/user-attachments/assets/6389e49c-58b4-4eaf-8eb9-52bb986d4dfe" />
<img width="1136" height="760" alt="image" src="https://github.com/user-attachments/assets/656cb170-0a76-4284-a9b5-d2df55893550" />
<img width="1066" height="713" alt="image" src="https://github.com/user-attachments/assets/61d08577-af57-49de-88d6-4fa987c263ef" />
<img width="1278" height="520" alt="image" src="https://github.com/user-attachments/assets/81a6e82f-5003-4459-98a0-763c88478377" />
<img width="1898" height="871" alt="image" src="https://github.com/user-attachments/assets/fa6e32c6-ca24-4d32-b004-29f4259516e5" />
<img width="1896" height="877" alt="image" src="https://github.com/user-attachments/assets/3c478d91-96bf-4e75-ba09-a72760013631" />
<img width="1911" height="864" alt="image" src="https://github.com/user-attachments/assets/394f8750-82be-4471-bf6a-b664ec6a96b8" />
<img width="1895" height="872" alt="image" src="https://github.com/user-attachments/assets/d865698c-3294-45c2-805c-90ae0877f8aa" />
<img width="1901" height="854" alt="image" src="https://github.com/user-attachments/assets/8c9db0eb-48d0-45f0-9b87-b74826392b24" />
<img width="1013" height="530" alt="image" src="https://github.com/user-attachments/assets/4e78a6cb-e71a-4e42-8ddb-d92f4a9f4836" />
<img width="790" height="344" alt="image" src="https://github.com/user-attachments/assets/dc66b3d1-cb89-4b33-93ef-855494b37b64" />
<img width="564" height="638" alt="image" src="https://github.com/user-attachments/assets/1d11ded9-721b-4393-aff7-fd81120df8ae" />
<img width="551" height="319" alt="image" src="https://github.com/user-attachments/assets/ec94a592-5a25-4160-a973-975d9e83acb5" />
<img width="582" height="446" alt="image" src="https://github.com/user-attachments/assets/84132152-55f4-4ff9-924d-2d665da8c7f7" />
<img width="550" height="485" alt="image" src="https://github.com/user-attachments/assets/62d848c8-b51a-4c06-b9f0-a5b920fd88fa" />
<img width="402" height="577" alt="image" src="https://github.com/user-attachments/assets/571d69f6-68ea-4b9d-9654-6686db4d7e92" />



---

## Key Engineering Decisions

### MOSFET Selection — Qrr-Driven, Not Rds(on)
Initial choice was IRF3710 (low Rds(on)). Loss analysis revealed reverse recovery loss dominated, contributing **~3.2 W (58% of Q2 losses)** in hard-switched operation. Switched to **IRFB4110**: drop-in TO-220 replacement, **Qrr reduced from 670 nC → 140 nC @ Tj=125°C (7× improvement)**. Reverse recovery loss dropped to 0.67 W; system efficiency improved from ~92% to 95.6%.

### Hot-Loop Optimization
The Cin → Q2 → Q3 → PGND high-frequency current path was compressed to **<200 mm² loop area**:
- 4× 1µF MLCC + 100nF placed directly adjacent to MOSFET drain/source pads
- SW node copper area intentionally minimized to reduce EMI antenna effect
- Wide, short PGND return polygon for hot-loop closure
- No bulk electrolytic at input (lab PSU is clean source; bulk would be too far from switching devices to help at 100 kHz)

### Three Isolated Ground Domains
- **PGND** — Power return for switching currents (bottom-layer polygon)
- **SGND** — Analog ground for ADC, current sensor, STM32 interface (top-layer polygon, isolated from PGND)
- **HS_GND** — Floating high-side reference tied to SW node (high dV/dt domain, kept isolated with keepout zones)

PGND and SGND are not joined on the PCB — eliminates ground loop coupling between the power stage and sensitive analog measurements.

### Inductor — Custom Designed and Hand-Wound
Designed from first principles using area-product method, iterated with DC bias roll-off from the manufacturer's permeability curve. Hand-wound in the lab.

| Parameter | Value |
|---|---|
| Core | 2× T130-26 stack (Iron Powder, Fuan KT130-26A) |
| Material | Iron Powder Mix -26, µᵢ = 75 |
| Ae (stack) | 72.2 mm² |
| le | 82.8 mm |
| Ve (stack) | 5.98 cm³ |
| AL (stack) | 82 nH/turn² |
| Turns | 35 |
| Wire | 9× parallel AWG 26 enameled (litz-style bundle) |
| L @ 0 A | ~100 µH |
| L @ 5 A | ~81 µH (82% permeability per DC bias curve) |
| B_DC,peak | 183 mT (vs. 1300 mT saturation → 84% margin) |
| Copper Loss | 0.61 W (24 mΩ @ 56°C) |
| Core Loss | 0.18 W (Steinmetz @ 100 kHz) |
| Temperature Rise | ~16°C |

Multi-strand AWG 26 chosen for **skin depth** (δ = 0.21 mm @ 100 kHz, AWG 26 radius ≈ δ) and **proximity effect** management. Two toroids stacked with cyanoacrylate at three 120° points → effectively parallel magnetic paths (AL doubled, Le constant, Cu length stays minimal vs. series configuration).

### Reverse Polarity Protection — Properly Sized Gate Drive
P-channel MOSFET (IRF5210) source-referenced gate divider: **R1 = 10 kΩ, R2 = 30 kΩ → Vgs = −12 V**, achieving full enhancement (Rds(on) at datasheet value). 12V Zener clamps gate against transient overvoltage. 58V TVS handles input surge events.

### Isolated Gate Drive
- Two TLP250H optocouplers — galvanic isolation, ±2.0 A peak output, ±40 kV/µs CMTI
- High-side supply: **B1212S-2WR3L** isolated DC-DC (12V_ISO referenced to HS_GND)
- Low-side supply: 12V_RAW / PGND with local decoupling (22µF electrolytic + 100nF MLCC)
- Gate drive: 4.7Ω series + 15V Zener clamp + 10kΩ pull-down (each channel)
- 400 ns dead-time for shoot-through prevention (worst-case TLP250H skew + MOSFET fall time + margin)

### Current Sensing & ADC Interface
- HLSR-10P hall-effect current sensor (isolated, ±10A range, ratiometric output)
- Differential measurement: Vout and Vref both routed to STM32 ADC for common-mode rejection
- Per channel: 10k/20k voltage divider → 200Ω series → 1 nF NP0/C0G anti-aliasing filter (fc ≈ 796 kHz)
- Dedicated SGND domain for analog signal integrity

### Thermal Management — Component-Level Derating
| MOSFET | Power Loss | Heatsink | Tj @ Tamb=40°C |
|---|---|---|---|
| Q2 (HS) | 2.4 W | 45AS + SilPad isolated | ~94°C (47% derating from Tj_max) |
| Q3 (LS) | 0.47 W | None | ~69°C |
| Q1 (PMOS) | 0.35 W | None | ~62°C |

Avoiding unnecessary BOM additions is also engineering — Q3 and Q1 do not need heatsinks at this power level.

---

## Loss Breakdown (Calculated)

| Source | Loss | Share |
|---|---|---|
| Q2 Switching (V-I overlap) | 1.57 W | 28% |
| Q2 Reverse Recovery (Qrr=140nC) | 0.67 W | 12% |
| Q3 Body Diode (dead-time) | 0.40 W | 7% |
| Gate Drive (Q2 + Q3) | 0.36 W | 7% |
| Q1 PMOS (conduction) | 0.35 W | 6% |
| Inductor (Cu + Core) | 0.77 W | 14% |
| Coss (Q2 + Q3) | 0.19 W | 3% |
| Q2 + Q3 Conduction | 0.14 W | 2% |
| PCB Traces | 0.15 W | 3% |
| Sensor + Driver + Divider | 0.24 W | 4% |
| Margin (5% uncertainty) | 0.26 W | 5% |
| **Total** | **5.5 W** | **100%** |

**η = 120 / (120 + 5.5) = 95.6%**

---

## Block Diagram

```
48V_RAW ─→ [Reverse Polarity Protection] ─→ 48V_PROT ─→ [HS MOSFET Q2] ─→ SW Node
                (IRF5210 P-FET)                          (IRFB4110)         │
                (TVS 58V, 12V Zener clamp)                                  │
                                                                            ├─→ [100µH Stack Inductor] ─→ [Output Caps] ─→ Vout (24V/5A)
                                                                            │                                  │
                                                            [LS MOSFET Q3] ←┘                          [HLSR-10P Current Sensor]
                                                            (IRFB4110)                                          │
                                                                                                       [ADC Conditioning]
                                                                                                                │
[B1212S Isolated DC-DC] ─→ 12V_ISO ─→ [TLP250H HS Driver]                                              [STM32 Interface]
                                                                                                          (H1, H2 Headers)
[12V_RAW] ─────────────────────────→ [TLP250H LS Driver]
```

---

## PCB Design

### Layer Strategy
| Layer | Purpose |
|---|---|
| Top | All power routing, switching node, gate drive, signal traces |
| Bottom | PGND polygon, selective SGND polygon, short signal crossovers |

### Design Rules (IPC-2221 compliant with margin)
- Minimum trace width: 0.7 mm
- Minimum clearance: 0.8 mm
- Via diameter: 1.8 mm, hole 0.8 mm
- HS_GND ↔ PGND clearance: 1.0 mm (high-voltage domain isolation)

### Trace Widths (DC current-driven)
| Net | Width | Rationale |
|---|---|---|
| 48V_RAW, 48V_PROT | 2–3 mm | ~2.5 A input current |
| SW node / HS_GND | 2–3 mm, minimum area | High dV/dt — minimize copper area for EMI |
| Output path (Vout) | 3 mm | 5 A continuous (IPC-2221 ΔT < 10°C) |
| PGND return | Wide polygon | Hot-loop closure, low impedance |
| Gate drive | 1 mm | Short paths, switching transient handling |
| 12V supplies | 1 mm | ~300 mA auxiliary |
| Analog signals | 0.7 mm | µA-level, noise immunity priority |

---

## Component Summary

### Power Stage
| Ref | Component | Package | Description |
|---|---|---|---|
| Q1 | IRF5210PBF | TO-220 | P-channel, reverse polarity protection |
| Q2 | IRFB4110PBF | TO-220 | N-channel HS switch (Qrr-optimized) |
| Q3 | IRFB4110PBF | TO-220 | N-channel LS synchronous rectifier |
| L1 | 100 µH | 2× T130-26 stack | Custom hand-wound, 35T × 9P AWG26 |
| D1 | TVS 58V | THT | Input transient suppression |
| D2 | 12V Zener | THT | Q1 gate clamp |
| D3, D4 | 15V Zener | THT | Q2/Q3 gate clamps |

### Gate Drive
| Ref | Component | Package | Description |
|---|---|---|---|
| U2, U3 | TLP250H | DIP-8 | Optocoupled isolated gate drivers |
| U1 | B1212S-2WR3L | SIP | 2W isolated DC-DC for HS supply |
| R3, R8 | 4.7 Ω | SMD | Gate series resistors |
| R7, R10 | 200 Ω | SMD | Opto LED current limit |
| R4, R9 | 10 kΩ | SMD | Gate pull-down |

### Sensing & Interface
| Ref | Component | Description |
|---|---|---|
| S1 | HLSR-10P | Hall-effect current sensor (isolated, ±10A) |
| R11, R14 | 10 kΩ | ADC voltage divider (upper) |
| R13, R16 | 20 kΩ | ADC voltage divider (lower) |
| R12, R15 | 200 Ω | ADC series protection |
| C19, C21 | 1 nF NP0/C0G | ADC anti-aliasing filter |
| H1 | 6-pin header | STM32 control interface (PWM_HS, PWM_LS) |
| H2 | 6-pin header | STM32 ADC interface (HLSR_Vout, HLSR_Vref) |

### Capacitors
| Ref | Value | Type | Description |
|---|---|---|---|
| C1–C4 | 1 µF / 100V | 1210 MLCC | Input HF decoupling (adjacent to MOSFETs) |
| C5 | 100 nF | MLCC | Input HF decoupling |
| C11, C12 | 470 µF / 50V | Electrolytic (THT) | Output bulk |
| C13 | 100 nF | MLCC | Output HF decoupling |
| C14 | 1 µF | MLCC | Output decoupling |
| C6, C7 | 2.2 µF | MLCC | B1212S input/output decoupling |
| C8, C10 | 100 nF | MLCC | HS driver local decoupling |
| C9 | 22 µF | Electrolytic | HS driver bulk decoupling |
| C16 | 22 µF | Electrolytic | LS driver bulk decoupling |
| C17 | 100 nF | MLCC | LS driver HF decoupling |

> **Note:** No input bulk electrolytic is used. At 100 kHz, an electrolytic at the input terminal would be too far from the switching devices to be effective. The 4×1µF MLCC array adjacent to the MOSFETs handles HF decoupling; the lab PSU provides bulk energy storage.

---

## Verification

PSIM simulation with SPICE-level MOSFET models confirms:
- Triangle inductor current waveform centered at 5 A with 1.5 A peak-to-peak ripple → CCM operation as designed
- Output voltage settles at ~24 V with <100 mV ripple
- 20 ms soft-start ramp prevents inrush oscillation
- PI controller (Kp=0.15, Ki=100) provides stable steady-state response

---

## Design Tools

- **Schematic & PCB:** Altium Designer
- **Simulation:** PSIM 2026
- **Control:** STM32 (firmware in progress)

---

## Author

**Batuhan Sağdan**
Electrical Engineering, 2nd Year
Yıldız Technical University (YTÜ)
 
---
 
## License
 
This project is shared for educational purposes. Feel free to reference the design decisions and methodology.







