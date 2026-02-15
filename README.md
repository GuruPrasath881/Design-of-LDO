# High-Speed Capless LDO & Bandgap Reference (180nm CMOS)

![Status](https://img.shields.io/badge/Status-Simulated-success)
![Technology](https://img.shields.io/badge/Tech-180nm%20CMOS-blue)
![Type](https://img.shields.io/badge/Type-Analog%20IP-orange)

## 📌 Overview
This repository contains the design and simulation data for a fully integrated **Capless Low-Dropout (LDO) Regulator** and a **Voltage-Mode Bandgap Reference (BGR)**.

Designed for System-on-Chip (SoC) applications where external capacitors are prohibitive, this LDO utilizes **Miller Compensation with a Nulling Resistor** to ensure unconditional stability across a **100x dynamic load range** (100µA to 10mA) without requiring a large output capacitor ($C_{load} \approx 4pF$).

## ⚡ Key Specifications

| Parameter | Value | Condition |
| :--- | :--- | :--- |
| **Technology** | 180nm | UMC180 CMOS |
| **Input Voltage ($V_{in}$)** | 1.8 V | Nominal |
| **Output Voltage ($V_{out}$)** | 1.6 V | Regulated |
| **Dropout Voltage** | < 100 mV | @ 10mA Full Load |
| **Max Load Current** | 10 mA | Industrial Standard |
| **Quiescent Current ($I_q$)** | ~400 µA | Optimized for Slew Rate |
| **Current Efficiency** | 97% | @ Full Load |
| **Phase Margin** | **67°** | @ Worst-Case (0.1mA) |
| **Load Regulation** | 0.8 mV/mA | 0.1mA to 10mA Step |
| **Line Regulation** | 10 mV/V | 1.7V to 2.0V Sweep |

## 🏗️ Architecture
### 1. Error Amplifier
A **Single-Stage Differential Amplifier** with a PMOS pass device was selected for its high DC gain and wide input common-mode range. The single-stage topology minimizes internal poles, simplifying the frequency compensation network required for the capless architecture.

### 2. Frequency Compensation
* **Problem:** In capless LDOs, the dominant pole shifts widely with load current ($R_{load}$), often leading to instability at light loads.
* **Solution:** Implemented **Miller Compensation ($C_c$)** with a **Nulling Resistor ($R_z$)**.
    * $C_c$ performs pole-splitting, locking the dominant pole at the Gate of the Pass Transistor.
    * $R_z$ pushes the Right-Half-Plane (RHP) Zero to high frequencies, preventing phase degradation.

### 3. Bandgap Reference
A Self-Biased **Voltage-Mode Bandgap Reference** provides a temperature-independent 1.2V reference.
* **TC:** 25.9 ppm/°C (-40°C to 120°C).
* **Startup:** Robust startup circuit ensures no zero-current lockup.

## 📊 Simulation Results

### 1. Line Regulation & Dropout
* **Observation:** The LDO achieves regulation at $V_{in} \approx 1.67V$ (Dropout $\approx$ 70mV) and maintains a steady 1.6V output up to 2.0V.
* **Metric:** Line Regulation $\approx$ 10 mV/V.

![Line Regulation Plot](plots/Line%20regulation.png)

---

### 2. Stability & PSRR
* **PSRR:** Achieves **>42 dB** rejection at low frequencies (DC). The rejection bandwidth is limited by the Error Amplifier loop gain, dropping to 0dB at high frequencies as expected for Capless topologies.
* **Stability:** Phase Margin of **67°** at worst-case light load (100µA) and **100°** at full load (10mA).

![PSRR Plot](plots/PSRR_best_case.png)

*(Note: High-frequency degradation is expected in Capless topologies due to lack of output capacitance)*

---

### 3. Transient Response
* **Condition:** Fast load step from 0.1mA to 10mA (Rise time = 100ns).
* **Performance:** * **Undershoot:** < 80 mV (<5% deviation) recovery limited by Slew Rate.
  * **Overshoot:** ~110 mV (Within acceptable digital logic safety margins).
  * **Efficiency:** High current efficiency (97%) achieved by optimizing the Speed-Power trade-off.

![Transient Response](plots/Transient%20response.png)

---

### 4. Bandgap Reference (BGR) Output
* **Performance:** Stable 1.2V reference output with curvature correction achieving 25.9 ppm/°C.

![BGR Output](plots/BGR%20output.png)
