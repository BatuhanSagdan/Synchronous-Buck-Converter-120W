# Synchronous-Buck-Converter-120W
48V IN - 24V @ 5A OUT, Buck Converter design utilizing Altium Designer and PI control simulation in MATLAB/Simulink
![Status](https://img.shields.io/badge/Status-In%20Progress-yellow)
![Tools](https://img.shields.io/badge/Tools-Altium%20Designer%20%7C%20MATLAB%2FSimulink-blue)

## 📖 Overview
This repository contains the design files for a **120W Synchronous Buck Converter**. The project aims to design a high-efficiency DC-DC step-down converter, including simulation verification in MATLAB/Simulink and PCB implementation using Altium Designer.

The primary goal is to achieve stable voltage regulation with high efficiency suitable for power electronics applications.

## 🚧 Current Status: Routing Phase
**This project is currently under active development.** * ✅ **Circuit Design & Component Selection:** Completed.
* ✅ **Simulation (MATLAB/Simulink):** Verified with PI control.
* 🔄 **PCB Layout:** The component placement is finalized. Currently, the **routing process is in the final stage** and is expected to be completed soon.

## ⚙️ Technical Specifications

| Parameter | Value |
| :--- | :--- |
| **Topology** | Synchronous Buck Converter |
| **Input Voltage** | [24V - Buraya Giriş Voltajını Yaz] |
| **Output Voltage** | [12V - Buraya Çıkış Voltajını Yaz] |
| **Output Current** | [10A - Buraya Akımı Yaz] |
| **Switching Frequency** | [100 kHz - Buraya Frekansı Yaz] |
| **Control Method** | Analog / PI Control |

## 📂 Repository Structure
* **Hardware/**: Contains Altium Designer project files (Schematic `.SchDoc` and PCB `.PcbDoc` files).
* **Simulation/**: Contains MATLAB/Simulink (`.slx`) simulation models and control loop designs.

## 🛠️ Tools & Technologies
* **PCB Design:** Altium Designer 20
* **Simulation:** MATLAB & Simulink
* **Component Libraries:** Custom created libraries included in the `Hardware` folder.

## 🔜 Next Steps (To-Do)
- [ ] Complete the final PCB routing.
- [ ] Generate Gerber and NC Drill files for manufacturing.
- [ ] Bill of Materials (BOM) finalization.
- [ ] Prototype assembly and bench testing.

---
*Created by [Batuhan Sağdan](https://www.linkedin.com/in/batuhansagdan/)*
