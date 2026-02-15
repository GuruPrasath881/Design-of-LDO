# Capless LDO & Bandgap Reference (180nm CMOS)

![Status](https://img.shields.io/badge/Status-Simulated-success)
![Technology](https://img.shields.io/badge/Tech-180nm%20CMOS-blue)
![Type](https://img.shields.io/badge/Type-Analog%20IP-orange)

## 📌 Overview
This repository contains the design and simulation data for a fully integrated **Capless Low-Dropout (LDO) Regulator** and a **Curvature-Corrected Bandgap Reference (BGR)**.

Designed for System-on-Chip (SoC) applications where external capacitors are prohibitive, this LDO utilizes **Miller Compensation with a Nulling Resistor** to ensure unconditional stability across a **100x dynamic load range** (100µA to 10mA) without requiring a large output capacitor ($C_{load} \approx 4pF$).

## ⚡ Key Specifications

| Parameter | Value | Condition |
| :--- | :--- | :--- |
| **Technology** | 180nm CMOS | Generic PDK |
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
A **Single-Stage Differential Amplifier with Current Mirror Load** was selected for its high DC gain and wide input common-mode range. The single-stage topology minimizes internal poles, simplifying the frequency compensation network required for the capless architecture.

### 2. Frequency Compensation (The "Stability Secret")
* **Problem:** In capless LDOs, the dominant pole shifts widely with load current ($R_{load}$), often leading to instability at light loads.
* **Solution:** Implemented **Miller Compensation ($C_c$)** with a **Nulling Resistor ($R_z$)**.
    * $C_c$ performs pole-splitting, locking the dominant pole at the Gate of the Pass Transistor.
    * $R_z$ pushes the Right-Half-Plane (RHP) Zero to high frequencies, preventing phase degradation.

### 3. Bandgap Reference
A self-biased, current-mode Bandgap Reference provides a temperature-independent 1.2V reference.
* **TC:** 12.5 ppm/°C (-40°C to 120°C).
* **Startup:** Robust startup circuit ensures no zero-current lockup.

## 📊 Simulation Results

### Stability Analysis (Bode Plot)
* **Worst Case (100µA):** PM = 67° (Stable)
* **Best Case (10mA):** PM = 100° (Over-damped)

*[Insert Screenshot of Bode Plot Here]*

### Transient Response
* **Load Step:** 0.1mA $\rightarrow$ 10mA (Rise time = 100ns)
* **Undershoot:** < 80 mV (<5% deviation)
* **Settling Time:** Fast recovery due to slew-rate optimized $I_q$.

*[Insert Screenshot of Transient Response Here]*

## 📂 Repository Structure
