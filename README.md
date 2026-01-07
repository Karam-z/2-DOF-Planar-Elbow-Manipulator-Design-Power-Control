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
