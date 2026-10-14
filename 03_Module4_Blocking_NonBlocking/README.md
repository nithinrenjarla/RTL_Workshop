# Module4_GLS-Blocking_NonBlocking
MODLE 4: Blocking vs Non-Blocking Assignments-Simulation Mismatch using Verilog, Icarus Verilog,GTKWave, Yosys and SKY130

# Introduction
Blocking (=) and non-blocking (<=) assignments are fundamental concepts in Verilog HDL. They determine how statements are executed within procedural blocks.
Proper use of these assignments is essential for designing reliable and synthesizable digital circuits. This module demonstrates the implementation of multiplexers, the behavior of blocking and non-blocking assignments, and common synthesis–simulation mismatch scenarios.
The designs are simulated using Icarus Verilog, analyzed using GTKWave, synthesized using Yosys, and mapped to the SKY130 standard-cell library.
# Objectives
To understand the difference between blocking (=) and non-blocking (<=) assignments.
To study the causes of synthesis–simulation mismatch.
To design and simulate multiplexer circuits using Verilog.
To analyze the behavior of blocking assignments through RTL simulation.
To verify circuit functionality using Icarus Verilog and GTKWave.
To synthesize RTL designs using Yosys.
To map synthesized circuits to the SKY130 standard-cell library.
To compare RTL simulation results with synthesized results.

# Tools and Technologies Used
| Tool / Technology |Purpose |
|---|---|
| Verilog HDL |RTL Design |
| Icarus Verilog | Compilation and Simulation |
| GTKWave | Waveform Analysis |
| Yosys | RTL Synthesis |
| SKY130 Standard Cell Library | Technology Mapping |
| Linux Terminal | Command Execution |
| gVim | Verilog Editing |

# Table of Contents
1. RTL Simulation of a 2×1 Multiplexer
2. Technology Mapping of the Multiplexer
3. Functional Verification Using Simulation Waveform
4. Analysis of an Incorrect Multiplexer Design
5. Verification of Bad Multiplexer Behavior
6. Simulation of Blocking Assignment Behavior
7. Synthesis of Blocking Assignment Circuit
8. Blocking Assignment Using Previous Value
9. Overall Result
10. Conclusion

# 1.RTL Simulation of a 2×1 Multiplexer

# Overview
A 2×1 multiplexer was implemented using the Verilog ternary operator to understand combinational logic design. The RTL design was simulated using Icarus Verilog, and the output waveform was verified using GTKWave. The experiment demonstrates how the output changes based on the select signal and validates the functional correctness of the multiplexer before synthesis.

# Simulation Commands
```verilog
iverilog -o mux ternary_operator_mux.v tb_ternary_operator_mux.v

gtkwave ternary_operator_mux.vcd
```
<img width="700" alt="tb_ternary" src="https://github.com/user-attachments/assets/b24a5e29-48a1-46e4-8c57-230c6c943078" />

# ✅Observation
The RTL simulation confirmed the correct functionality of the 2×1 multiplexer

# 2.Technology Mapping of the Multiplexer

# Overview
The multiplexer design was synthesized using Yosys and mapped to the SKY130 standard-cell library. During synthesis, the RTL description was optimized and converted into a technology-specific multiplexer cell, reducing hardware complexity while preserving functionality.

# Synthesis Commands
```verilog
yosys

read_verilog mux_generate.v

synth -top mux_generate

abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

show
```
# Synthesized Circuit

 <img width="700"  alt="ternary show" src="https://github.com/user-attachments/assets/a6006a9f-08e3-4f15-b223-464b96c9fe35" />

 # ✅Observation
The multiplexer was successfully mapped to the SKY130 standard-cell library.

# 3.Functional Verification Using Simulation Waveform

# Overview
The simulation waveform verifies that the multiplexer output correctly follows the selected input. Different input combinations were applied to validate the functionality of the design under various conditions before synthesis.

# Simulation Commands
```verilog
iverilog -o mux mux_generate.v tb_mux_generate.v

gtkwave mux_generate.vcd
```
# Output Waveform
<img width="700"  alt="ternary operator gtkwave" src="https://github.com/user-attachments/assets/4885f07b-81b8-4b18-a9d0-d7f45d98b01a" />

# ✅Observation
The waveform confirmed correct multiplexer operation for all input combinations

# 4.Analysis of an Incorrect Multiplexer Design

# Overview
This experiment demonstrates an improperly coded multiplexer that leads to synthesis-simulation mismatch. Incomplete assignments inside the always block may infer latches during synthesis, resulting in hardware behavior different from RTL simulation.

# Simulation Commands
```verilog
iverilog -o bad_mux bad_mux.v tb_bad_mux.v

gtkwave bad_mux.vcd
```
# Output

<img width="700"  alt="bad mux gtkwave" src="https://github.com/user-attachments/assets/3f82d09b-77a6-4e53-8782-9de230dc10ba" />

# ✅Observation
The incorrect coding style produced unexpected output behaviour during simulation

# 5.Verification of Bad Multiplexer Behavior

# Overview
The waveform illustrates the behavior of the incorrectly implemented multiplexer. Missing assignments cause previous output values to be retained, resulting in latch inference and mismatch between simulation and synthesized hardware.

# Simulation Commands
```verilog
iverilog -o bad_mux bad_mux.v tb_bad_mux.v

gtkwave bad_mux.vcd
```
# Output Waveform

<img width="700"  alt=" badmux gtkwave" src="https://github.com/user-attachments/assets/539bfe9a-103a-4027-aba4-696618f31443" />

# ✅Observation
The Waveform demonstrated synthesis-simulation mismatch caused by improper RTL coding.

# 6.Simulation of Blocking Assignment Behavior

# Overview
Blocking assignments execute statements sequentially within an always block. This experiment illustrates their behavior during simulation and explains why they are mainly recommended for combinational logic. Incorrect usage in sequential circuits may produce unexpected results.

# Simulation Commands
```verilog
iverilog -o blocking blocking_caveat.v tb_blocking_caveat.v

gtkwave blocking_caveat.vcd
```
# Output Waveform
<img width="700"  alt="blocking caveat gtk" src="https://github.com/user-attachments/assets/ea966d4b-f0ba-470f-8e1e-81e25b14bf17" />

# ✅Observation
The simulation demonstrated the sequential execution behaviour of blocking assignments

# 7.Synthesis of Blocking Assignment Circuit

# Overview
The blocking assignment circuit was synthesized using Yosys to compare RTL simulation with the generated hardware. The synthesized circuit illustrates how the Verilog description is interpreted and mapped to the SKY130 standard-cell library.

# Synthesis Commands
```verilog
yosys

read_verilog blocking_caveat.v

synth -top blocking_caveat

abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

show
```
# Technology-Mapped Circuit

<img width="700"  alt="blocking caveat" src="https://github.com/user-attachments/assets/e0bda30d-37d3-44e2-9ef6-b1ff7481ee33" />

# ✅Observation
The synthesized hardware accurately represented the intended circuit implementation.

# 8.Blocking Assignment Using Past Value

This experiment demonstrates how blocking assignments (=) use the updated value immediately within the same procedural block. The simulation illustrates how the output depends on the sequence of statements and the previously assigned values. This helps in understanding the behavior of blocking assignments and why they can sometimes lead to unexpected results if used in sequential logic. Therefore, blocking assignments are generally recommended for combinational logic, while non-blocking assignments (<=) are preferred for sequential circuits.

# Commands
```verilog
iverilog -o blocking_past blocking_caveat.v tb_blocking_caveat.v

gtkwave blocking_caveat.vcd
```
# Output
<img width="700"  alt="tb blocking caveat" src="https://github.com/user-attachments/assets/e23dd32e-fde5-4f82-a2f8-857c2b5ebcea" />

# ✅Observation
The waveform shows that blocking assignments execute sequentially and immediately update variable values within the same procedural block. This demonstrates how statement ordering influences simulation behavior and highlights the importance of using blocking assignments appropriately.

# Overall Result

The experiments successfully demonstrated the implementation of multiplexers, blocking assignments, and synthesis-simulation mismatch scenarios. RTL simulation, waveform analysis, and hardware synthesis verified the importance of writing synthesizable Verilog code. The generated circuits were successfully mapped to the SKY130 standard-cell library.

# Conclusion
This module provided a comprehensive understanding of blocking and non-blocking assignments, multiplexer implementation, and synthesis-simulation mismatch. Through simulation, synthesis, and technology mapping, the experiments emphasized the importance of proper RTL coding practices for generating reliable and optimized digital hardware. The complete RTL-to-GDS front-end flow using Icarus Verilog, GTKWave, Yosys, and SKY130 was successfully demonstrated.


























