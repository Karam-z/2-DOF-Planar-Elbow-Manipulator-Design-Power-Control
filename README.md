# 2-DOF-Planar-Elbow-Manipulator-Design-Power-Control


![Status](https://img.shields.io/badge/Status-Complete-success)
![Platform](https://img.shields.io/badge/Platform-MATLAB%20|%20Simulink%20|%20SolidWorks%20|%20LTspice-blue)

## 📖 Project Overview
This project integrates mechanical design, power electronics, and non-linear control to create a robotic manipulator capable of precise trajectory tracking.

**System Constraints:**
* **Mechanical:** 700mm reach, 1N payload, Aluminum 6063-T4 construction.
* **Electrical:** 12V DC input, Buck-Boost drive, $<0.05A$ ripple.
* **Control:** Track multi-point path in $<6$ seconds with zero steady-state error.

---

## 📂 Repository Structure
```text
├── 01_Mechanical_Design/       # CAD & FEA Verification
│   ├── Assembly_Final.SLDASM
│   └── Mechanical_Design_Report.docx
│
├── 02_Electrical_Drive/        # Power Electronics Simulation
│   ├── BuckBoost_Design.asc
│   └── Electrical_Drive_Report.docx
│
├── 03_Control_System/          # Control Algorithms & Simulation
│   ├── Combined_System.slx     # MASTER SIMULATION
│   ├── reference.m             # Trajectory Planner
│   ├── dynamics.m              # Physics Engine
│   └── Control_System_Report.docx
│
└── Assets/                     # Project Visuals



**## 🛠️ Part 1: Mechanical Design (SolidWorks)**

### Objective
Design a 700 mm manipulator arm capable of supporting a 1 N payload while satisfying:

- Factor of Safety (FOS) > 1 for aluminum parts  
- Factor of Safety (FOS) > 2 for bolted connections  
- Static loading conditions  

---

### Design Strategy – Parallel Plate Truss

To minimize inertia while maintaining stiffness, a parallel plate truss topology was selected.

**Material:**
- Aluminum 6063-T4  
- Plate thickness: 2 mm  

**Key Design Features:**
- Triangular cutouts applied along the neutral axis to reduce mass with minimal effect on bending stress  
- Cylindrical spacers connecting the plates to resist torsional buckling  
- Symmetric geometry to ensure predictable load paths  

---

### Finite Element Analysis (FEA)

Static simulations were performed in SolidWorks Simulation.

**Deflection Requirements:**
- Test 1: Deflection < 1 mm  
- Test 2: Deflection < 5 mm  

**Results:**
- Maximum von Mises stress < yield stress  
- Maximum deflection < 5 mm  

**Outcome:**  
The design passed all strength and deflection requirements with FOS > 1 for aluminum parts and FOS > 2 for all bolted connections.

## ⚡ Part 2: Electrical Power System (LTspice)

### Objective
Design a DC-DC Buck-Boost converter to drive the DC motor armature from a 12 V source while ensuring:

- Continuous Conduction Mode (CCM)  
- Output current ripple ≤ 0.05 A  
- Stable operation during regenerative braking  

---

### Motor Parameters

- Armature inductance:  
  L_arm = 2.2 mH  

- Armature resistance:  
  R_arm = 0.96 Ω  

---

### Converter Topology

An inverting buck-boost converter was selected to support the required voltage range for torque control.

**Design Parameters:**
- Switching frequency: 50 kHz  
- Inductor: 3.36 mH (CCM operation)  
- Output capacitor: 350 µF (ΔV_out ≤ 0.2 V)  
- MOSFET: IRF530  
- Floating gate driver configuration  

---

### Back-EMF & Regenerative Braking

At high angular velocities (ω = 20 rad/s), back-EMF becomes significant.  
Since the converter output voltage is negative, the motor polarity was reversed to match the induced voltage direction.

**Outcome:**  
Simulation results confirm stable operation with output current ripple below 0.05 A, even during regenerative braking scenarios.


## 🤖 Part 3: Control System Design (MATLAB / Simulink)

### Objective
Track the following trajectory within 6 seconds:

(0.7, 0) → (0.4, 0) → (0.3, 0.4) → (0.5, 0.4)

**Performance Requirements:**
- Overshoot < 10%  
- Rise time < 2 s  
- Zero steady-state error  

---

### Control Architecture – Cascaded Control

A cascaded control architecture was implemented to decouple electrical and mechanical dynamics.

**Inner Loop (Electrical):**
- PI controller regulating buck-boost duty cycle  
- Controls motor torque  
- Saturation limits set to ±0.95 to handle high back-EMF  

**Outer Loop (Mechanical):**
- PID controller regulating joint angles based on position error  

---

### Trajectory Planner & Start-Shock Issue

**Problem:**  
Direct step inputs caused large initial error spikes, leading to solver instability and simulation crashes.

**Solution:**  
A custom trajectory planner (`reference.m`) interpolates smoothly from the robot’s actual home position (X ≈ 2.0) to the first target (0.7) before executing the task trajectory.

**Outcome:**  
The robot successfully traced the required trajectory within the 6-second limit with zero steady-state error.



## 🚀 How to Run the Simulation

### 1. Clone the Repository
```bash
git clone https://github.com/YourUsername/2DOF_Manipulator_Project.git
```

### 2. MATLAB Setup
- Open MATLAB  
- Navigate to `03_Control_System/`  
- Ensure the following files are added to the MATLAB path:
  - `buckboost.m`
  - `dynamics.m`
  - `reference.m`

### 3. Run Simulink
- Open `Combined_System.slx`  
- Click **Run**  
- Observe the end-effector path in the XY Graph block  

---

## ⚠️ Disclaimer

This project is for educational purposes only.  
Controller gains are tuned specifically for the inertia matrix and mechanical configuration of this manipulator and may not generalize to other systems.
