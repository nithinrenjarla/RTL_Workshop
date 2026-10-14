# Assesment-1

# Sequence Detector

# Introduction
This repository contains the work completed for Assessment 1 on the design, simulation, synthesis, and verification of a Sequence Detector using Verilog HDL.

The work includes RTL design, RTL simulation, waveform analysis using GTKWave, logic synthesis using Yosys, Gate-Level Simulation (GLS), and comparison of RTL and GLS results.

# Objectives
To design a sequence detector using Verilog HDL.

To understand and implement a finite state machine (FSM). 

To perform RTL simulation using Icarus Verilog. 

To generate and analyze VCD waveform files using GTKWave.

To synthesize the RTL design using Yosys.

To analyze the synthesized netlist.

To perform Gate-Level Simulation (GLS).

To compare RTL and GLS simulation results.

# Tools and Technologies
Verilog HDL

Icarus Verilog

GTKWave

Yosys
Linux / VSDSquadron Virtual Machine

Git and GitHub

# 1. Sequence Detector Design
The sequence detector was designed using Verilog HDL based on a finite state machine (FSM) approach.

# Design Files
```verilog/
sequence_detector.v
tb.v
synthesized.v
```
# 2. RTL Simulation
The RTL design was simulated using Icarus Verilog to verify the functional behavior of the sequence detector.

The simulation output was observed for different input sequences and the detected output was verified.

# 3. Waveform Analysis
The generated VCD file was opened using GTKWave to analyze the simulation waveforms.

# RTL Waveform
 <img width="1920" alt="dut" src="https://github.com/user-attachments/assets/3498101f-ee30-40b9-9c9e-da93bc35d961" />

# 4. RTL Synthesis
The RTL design was synthesized using Yosys.

The synthesized design was successfully converted into a gate-level representation, and synthesis statistics were obtained.

# Synthesis Statistics

<img width="1920"  alt="stats" src="https://github.com/user-attachments/assets/bf860756-d785-47f6-bf02-43b226473306" />

# Synthesized Netlist

<img width="1920" alt="ggraphical" src="https://github.com/user-attachments/assets/0b1e49dd-2a02-40dd-8069-bbcc37ef61e4" />

# 7. Gate-Level Simulation
Gate-Level Simulation (GLS) was performed using the synthesized netlist.

The GLS waveform was analyzed using GTKWave to verify the behavior of the synthesized design.

# GLS Waveform

<img width="1920" alt="gllss waveform" src="https://github.com/user-attachments/assets/4165e3ae-8a84-4f55-be2c-d699891a797e" />

# 6. RTL vs GLS Comparison
The RTL and GLS waveforms were compared to verify that the synthesized design maintains the expected functional behavior.

[RTL VS GLS WAVEFORM.pdf](https://github.com/user-attachments/files/31844534/RTL.VS.GLS.WAVEFORM.pdf)

# 7. Overall Result
The Sequence Detector was successfully designed, simulated, synthesized, and verified through Gate-Level Simulation.

The RTL simulation and GLS results were compared, and the expected sequence detection behavior was observed.

# 8. Conclusion
The Assessment 1 work successfully demonstrated the complete RTL-to-GLS flow for a Verilog-based Sequence Detector. The design was verified through RTL simulation, synthesized using Yosys, and validated through Gate-Level Simulation using the generated netlist.


