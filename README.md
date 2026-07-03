
# 100 MHz High-Linearity CMOS Amplifier Design (EE210)

This repository contains the design, schematics, and simulation setups for a high-linearity, high-frequency single-stage NMOS Common-Source (CS) amplifier operating at **100 MHz** using a **180nm CMOS process node**. 

The project outlines an iterative engineering evolution: starting with a simple resistive load, moving to a CMOS active-load current mirror network (Failed Setup), and finally converging on an optimized single-transistor architecture featuring robust passive source degeneration to fulfill strict harmonic constraints.

---

## 📋 Design Specifications & Constraints

The amplifier was designed strictly around the project constraints outlined below:
* **Load Resistance ($R_L$):** $1\text{ k}\Omega$ (one terminal grounded, the other AC-coupled to the amplifier network).
* **Linearity Metric:** Harmonic ratio $\frac{HD1}{HD3} \ge 40\text{ dB}$ for an output peak-to-peak voltage swing ($V_{o,\text{pk-pk}}$) of $\approx 1\text{ V}$.
* **Operating Frequency:** $100\text{ MHz}$ sinusoidal excitation centered around a $0\text{ V}$ DC baseline.
* **Supply Voltage ($V_{DD}$):** Single $1.8\text{ V}$ DC rail.
* **Component Boundaries:** No passive inductors allowed. Maximum individual capacitance capped at $100\text{ pF}$.

---

## 🚀 Design Evolution & Iterative Process

### ❌ Phase 1 & 2: CMOS Active-Load Pair (Failed Approach)
* **Topology:** An NMOS driver ($W=100\,\mu\text{m}, L=180\text{ nm}$) paired with a PMOS active current-source load ($W=200\,\mu\text{m}, L=180\text{ nm}$) and a minimal source degeneration resistor ($R_S = 10\,\Omega$).
* **The Issue:** Attempting to maintain the required output headroom triggered a severe biasing "Tug-of-War." Due to the acute operating point sensitivity of the PMOS current source under high-swing regimes ($V_a = 450\text{ mV}$), symmetrical operational tracking broke down. 
* **Result:** The third harmonic spiked drastically ($HD3 = 28.96\text{ mV}$ vs $HD1 = 572.67\text{ mV}$), yielding an unsatisfactory linearity profile of **$25.92\text{ dB}$** and a low large-signal voltage gain of **$1.27\text{ V/V}$ ($2.09\text{ dB}$)**.

###  Final Configuration: Degenerated NMOS Common-Source
* **Topology:** Single-transistor NMOS CS architecture leveraging a tuned, deep passive source degeneration resistor ($R_S = 40\,\Omega$) to smooth out the non-linear transconductance loop ($G_m \approx \frac{g_m}{1+g_mR_S}$).
* **Sizing Optimization:** Transistor dimensions scaled to $W=550\,\mu\text{m}, L=180\text{ nm}$ to recover gain lost from the negative feedback path.
* **Result:** The layout achieved exceptional third-harmonic suppression ($HD3$ dropped to just $5.49\text{ mV}$), successfully elevating the linearity ratio to **$38.58\text{ dB}$** and maximizing fundamental large-signal voltage gain to **$2.66\text{ V/V}$ ($8.50\text{ dB}$)** with an output envelope of $0.92\text{ V}_{\text{pk-pk}}$.

---

## 📊 Performance Comparison Summary

| Parameter | Failed Architecture (Active Load) | Final Optimized Architecture (Degenerated NMOS) |
| :--- | :---: | :---: |
| **Input Signal Drive ($V_a$)** | $450\text{ mV}$ | $175\text{ mV}$ |
| **Fundamental Tone ($HD1$)** | $572.67\text{ mV}$ | $465.94\text{ mV}$ |
| **Third Harmonic ($HD3$)** | $28.96\text{ mV}$ | $5.49\text{ mV}$ |
| **Linear Voltage Gain ($A_v$)** | $1.27\text{ V/V}$ ($2.09\text{ dB}$) | **$2.66\text{ V/V}$ ($8.50\text{ dB}$)** |
| **Linearity Ratio ($\frac{HD1}{HD3}$)** | $25.92\text{ dB}$ | **$38.58\text{ dB}$** |
| **Output Peak-to-Peak Swing** | $\approx 1.1\text{ V}$ | **$0.92\text{ V}$** |

---

## 📂 Repository Structure

```text
├── Active_Load_Plot_1.png     # Transient output envelope graph for the active load setup
├── Active_Load_Plot_2.png     # High-harmonic DFT spectrum plot for the active load setup
├── Active_Load_Schematic.png # Cadence schematic view of the Phase 2 CMOS active load pair
├── AmplifierDesign_PS.pdf     # Official EE210 design problem specification sheet
├── Amplifier_Report.pdf       # Formal compiled academic engineering report (LaTeX)
├── EE210_final.tar.gz         # Compressed Spectre testbench and circuit netlist archive
├── Final_Plot_1.png          # Symmetrical transient tracking waveform for the final design
├── Final_Plot_2.png          # Discrete Fourier Transform output displaying HD1 vs HD3
└── Final_Schematic.png       # Optimized degenerated NMOS Common-Source schematic layout
