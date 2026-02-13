
# 120W Synchronous Buck Converter Design ⚡
![Status](https://img.shields.io/badge/Status-In%20Progress-yellow)

![Tools](https://img.shields.io/badge/Tools-Altium%20Designer%20%7C%20MATLAB%2FSimulink-blue)


## 🛠 Technical Specifications (Phase 1)

| Parameter | Value | Description |
| :--- | :--- | :--- |
| **Topology** | Synchronous Buck | High-side & Low-side MOSFETs are used for better efficiency compared to diode rectification. |
| **Input Voltage ($V_{in}$)** | $30V - 80V$ DC | Components rated for **100V**. Safe operating range includes a 20% safety margin. |
| **Output Voltage ($V_{out}$)** | Variable ($0 - V_{in}$) | Controlled via Duty Cycle adjustment (Potentiometer). |
| **Current Capacity** | $5A$ Continuous | PCB traces and components designed for **7A peak** thermal limit. |
| **Max Power** | $120W$ | Rated @ 24V Output / 5A Load. |
| **Switching Freq ($f_{sw}$)** | $30 kHz$ | Limited by Arduino PWM resolution. Target is 100 kHz+ with STM32 migration. |
| **Control Method** | Open-Loop PWM | Manual Duty Cycle control via Potentiometer (Prototyping Stage). |
| **Power Stage** | IRF3710 MOSFETs | $100V$, $57A$, $23m\Omega$. Driven by **TLP250H** Isolated Gate Drivers. |
| **Isolation** | Fully Isolated | Control logic is isolated from the HV power stage using TLP250H and B1212S DC-DC. |
| **PCB Construction** | DIY 2-Layer | Custom etched on FR4 Copper Clad. Power traces are reinforced for high current. |

## 🚀 Key Design Features

### 1. Isolated Gate Drive Architecture
Unlike standard bootstrap circuits, this design uses **TLP250H Optocouplers** with a dedicated **B1212S isolated power supply**.
* **Why?** This prevents high-voltage switching noise from the power stage (48V+) from interfering with the sensitive MCU logic. It also protects the microcontroller in case of catastrophic MOSFET failure.

### 2. Synchronous Rectification
Instead of a passive Schottky diode, a second MOSFET (Low-Side) is used.
* **Benefit:** Reduces conduction losses significantly at high currents (5A+), minimizing thermal dissipation and improving overall efficiency.

### 3. DIY PCB Fabrication Strategy
The PCB is designed for in-house fabrication using a **Double-Sided FR4 Copper Clad Board**.
* **Routing Strategy:** Majority of tracks are routed on the Bottom Layer for ease of soldering.
* **High Current Handling:** Top layer jumpers are used only for signal crossing, while power traces are widened to handle up to **7A**.

## 🔮 Roadmap (Future Work)

- [ ] **MCU Upgrade:** Migration to STM32F401RE for higher switching frequency ($>100kHz$).
- [ ] **Closed-Loop Control:** Implementing PI algorithm for Constant Current (CC) and Constant Voltage (CV) modes.
- [ ] **Sensing:** Adding Shunt Resistors/ACS712 for current feedback and Voltage Dividers for voltage feedback.
- [ ] **Efficiency Analysis:** Plotting Efficiency vs. Load curves.
<img width="860" height="757" alt="image" src="https://github.com/user-attachments/assets/8a0d0e67-3432-45ea-9ebb-7886c4c51f05" />
<img width="926" height="762" alt="image" src="https://github.com/user-attachments/assets/fda4f6c3-c6eb-40dd-9e71-75b8eaa5a476" />
<img width="1385" height="822" alt="image" src="https://github.com/user-attachments/assets/11076121-f8c0-4764-88fd-e6806bef0756" />
<img width="1801" height="448" alt="image" src="https://github.com/user-attachments/assets/a90355f3-fdc3-46d2-8bdc-77ef47c3fbc9" />




Created by [Batuhan Sağdan](https://www.linkedin.com/in/batuhansagdan/)


