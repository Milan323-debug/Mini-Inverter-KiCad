# ⚡ Mini Inverter — KiCad PCB Design

> A compact **12V DC → 240V AC half-bridge inverter** PCB designed from scratch in KiCad.  
> Uses the **IR2153 self-oscillating half-bridge driver** with IRLIZ44N power MOSFETs and a reverse-driven step-down transformer.

---

## 📸 Preview

> _Add schematic screenshot, PCB top/bottom view, and 3D render images here after exporting from KiCad_

---

## 📋 Project Overview

This is a learning project in power electronics and PCB design. The circuit converts **12V DC input** (battery or PSU) to drive the primary of a **240V → 12V step-down transformer in reverse**, producing a high-voltage AC output suitable for demonstration and educational purposes.

The oscillation frequency is user-adjustable via an onboard **10K potentiometer**, making it easy to tune for the transformer's characteristics.

---

## 🧰 Bill of Materials (BOM)

| Reference | Component         | Value / Part     | Package                        | Description                          |
|-----------|-------------------|------------------|-------------------------------|--------------------------------------|
| U1        | Half-Bridge IC    | IR2153           | DIP-8                         | Self-oscillating gate driver         |
| Q1, Q2    | MOSFET            | IRLIZ44N         | TO-220F-3 Vertical            | N-channel logic-level power MOSFET   |
| T1        | Transformer       | 240V→12-0-12V    | 5-pin terminal block          | Step-down transformer (reverse driven)|
| BT1       | Power Input       | 12V              | 7-pin terminal block          | Battery / DC input connector         |
| D2        | Rectifier Diode   | 1N4007           | DO-41                         | Input polarity protection            |
| D3        | LED Indicator     | LED              | 5mm THT                       | Power-on indicator                   |
| D1        | LED Output        | LED              | 2-pin terminal block          | Output indicator                     |
| C1        | Electrolytic Cap  | 47 µF            | Radial D5.0mm                 | Input filter capacitor               |
| C2        | Tantalum Cap      | 470 µF           | Radial D6.0mm                 | Bulk input capacitor                 |
| C3        | Ceramic Disc Cap  | 0.01 µF          | Disc D3.0mm                   | VCC decoupling capacitor             |
| C4        | Ceramic Disc Cap  | 0.2 µF           | Disc D3.0mm                   | CT timing capacitor                  |
| R1        | Resistor          | 1 KΩ             | DIN0204 axial                 | LED current limiting resistor        |
| R2        | Resistor          | 1 KΩ             | DIN0204 axial                 | Gate resistor (Q2)                   |
| R3        | Resistor          | 22 KΩ            | DIN0204 axial                 | Gate pull resistor (Q2 via IR2153 HO)|
| R4        | Resistor          | 22 KΩ            | DIN0204 axial                 | Gate pull resistor (Q1 via IR2153 LO)|
| R5        | Resistor          | 1 KΩ             | DIN0204 axial                 | Gate resistor (Q1)                   |
| R6        | Resistor          | 22 KΩ            | DIN0204 axial                 | RT resistor for IR2153 oscillator    |
| RV1       | Potentiometer     | 10 KΩ            | ACP CA6-H2.5 Horizontal       | Frequency adjustment                 |

---

## 🔌 Circuit Description

### How it works

1. **IR2153 (U1)** is a self-oscillating half-bridge gate driver IC. It generates complementary PWM signals on its **HO** (high-side output) and **LO** (low-side output) pins with built-in dead time to prevent shoot-through.

2. **Frequency** is set by the **RT** resistor (R6 + RV1 potentiometer) and **CT** capacitor (C4):
   ```
   f ≈ 1 / (1.4 × RT × CT)
   ```
   With R6 = 22KΩ, RV1 = 0–10KΩ, and C4 = 0.2µF, frequency is tunable across a range suitable for the transformer.

3. **Q1 and Q2 (IRLIZ44N)** are the half-bridge switching MOSFETs. They alternately switch the transformer primary, driving current in both directions to produce AC.

4. **T1** is a standard 240V→12V step-down transformer wired in reverse — its 12V secondary is driven by the MOSFETs, and the 240V primary produces the high-voltage AC output.

5. **D2 (1N4007)** provides input polarity protection. **C1** and **C2** filter the DC input supply.

### IR2153 Pin Connections (U1)

| Pin | Name | Connected To           |
|-----|------|------------------------|
| 1   | VCC  | Input (+12V)           |
| 2   | RT   | R6 / RV1 (freq. set)   |
| 3   | CT   | C4 (timing cap)        |
| 4   | GND  | GND                    |
| 5   | LO   | R4 → Q1 Gate           |
| 6   | COM  | GND                    |
| 7   | HO   | R3 → Q2 Gate           |
| 8   | VB   | Input (+12V)           |

---

## 📁 Repository Structure

```
Mini-Inverter-KiCad/
│
├── Miniinverter.kicad_sch        # KiCad schematic file
├── Miniinverter.kicad_pcb        # KiCad PCB layout file
├── Miniinverter.kicad_pro        # KiCad project file
├── Miniinverter.kicad_prl        # KiCad local settings
│
├── MiniInverter/                 # Gerber files (ready for fabrication)
│   ├── Miniinverter-F_Cu.gbr         # Front copper layer
│   ├── Miniinverter-B_Cu.gbr         # Back copper layer
│   ├── Miniinverter-F_Silkscreen.gbr # Front silkscreen
│   ├── Miniinverter-B_Silkscreen.gbr # Back silkscreen
│   ├── Miniinverter-F_Mask.gbr       # Front solder mask
│   ├── Miniinverter-B_Mask.gbr       # Back solder mask
│   ├── Miniinverter-F_Paste.gbr      # Front paste layer
│   ├── Miniinverter-B_Paste.gbr      # Back paste layer
│   ├── Miniinverter-Edge_Cuts.gbr    # Board outline
│   ├── Miniinverter-PTH.drl          # Plated through-hole drill file
│   ├── Miniinverter-NPTH.drl         # Non-plated drill file
│   └── Miniinverter-job.gbrjob       # Gerber job file
│
└── README.md
```

---

## 🛠️ Tools Used

- **[KiCad 8](https://www.kicad.org/)** — Schematic capture and PCB layout (free & open source)
- **[Tracespace Gerber Viewer](https://tracespace.io/view/)** — Online Gerber preview
- All components are **through-hole (THT)** for easy hand soldering

---

## ⚠️ Safety Warning

> **THIS CIRCUIT PRODUCES HIGH VOLTAGE AC OUTPUT (up to 240V).**  
> High voltage can cause **serious injury or death**.  
> - Do NOT touch the transformer output or any connected wiring while powered.  
> - Build and test in an **insulated, safe environment**.  
> - This project is for **educational and demonstration purposes only**.  
> - Always follow your local electrical safety regulations.

---

## 🚀 Getting Started

### Prerequisites
- KiCad 8 or later ([download here](https://www.kicad.org/download/))

### Open the Project
1. Clone this repository:
   ```bash
   git clone https://github.com/YOUR_USERNAME/Mini-Inverter-KiCad.git
   ```
2. Open KiCad → **File → Open Project** → select `Miniinverter.kicad_pro`
3. Open the schematic or PCB editor from the KiCad project manager

### Order the PCB
The `MiniInverter/` folder contains ready-to-use **Gerber files**. Upload them to any PCB manufacturer:
- [JLCPCB](https://jlcpcb.com) _(recommended, very affordable)_
- [PCBWay](https://www.pcbway.com)
- [OSH Park](https://oshpark.com)

---

## 📖 Learning Resources

- [IR2153 Datasheet (PDF)](https://www.infineon.com/dgdl/ir2153.pdf?fileId=5546d462533600a401535668d7c216d0)
- [IRLIZ44N Datasheet (PDF)](https://www.infineon.com/dgdl/irliz44npbf.pdf?fileId=5546d462533600a4015356600f9a1fff)
- [Half-Bridge Inverter Theory — Wikipedia](https://en.wikipedia.org/wiki/H-bridge)
- [KiCad Documentation](https://docs.kicad.org/)

---

## 📄 License

This project is released under the **MIT License** — free to use, modify, and distribute with attribution.

---

## 🙋 Author

**Milan Pramanik**  
Electronics enthusiast | PCB Designer | Learning power electronics  

---

_⭐ If you found this project useful or interesting, consider giving it a star!_
