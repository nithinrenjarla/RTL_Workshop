# Smart Street Light Controller Using Verilog HDL

A Verilog RTL project for a smart street light controller, developed as part of the RTL Design Workshop. The design was processed through the OpenLane RTL-to-GDSII implementation flow targeting the SkyWater SKY130A technology platform. The recorded run generated a final GDSII layout artifact.

---

## Table of Contents

1. Project Overview
2. Inputs and Outputs
3. Control Logic Used
4. Tools Used
5. Project Folder Structure
6. Test Cases
7. RTL Design
8. Testbench
9. Simulation
10. Waveform
11. Synthesis
12. Pre-Layout STA
13. Physical Design (OpenLane)
14. Post-Route STA
15. Final Layout (GDS)
16. Signoff Summary
17. Implementation Results
18. Additional Verification
19. Feasibility
20. Conclusion

---

## 1. Project Overview

**Problem:** Street lighting requires reliable switching control. A digital controller can implement defined switching conditions and support controlled transitions between operating states.

**Proposed solution:** A Verilog-based Smart Street Light Controller that implements the switching and fault-handling behavior defined in its RTL.

**Project objective:** Develop a synthesizable digital design, evaluate its functional behavior, and process it through an automated physical-design flow to generate a GDSII layout.

**Implementation platform:** OpenLane with the SKY130A PDK and `sky130_fd_sc_hd` standard-cell library.

The implementation is intended to demonstrate the digital design and ASIC implementation workflow. Real-world sensor integration and field deployment require additional validation.

## 2. Inputs and Outputs

The exact signal names and descriptions must match the implemented Verilog module.

| Signal | Direction | Description |
|---|---|---|
| Clock and reset, if defined | Input | Timing and initialization control |
| Switching-control inputs | Input | Inputs used to determine light operation |
| Fault-related inputs, if defined | Input | Inputs used by the implemented fault logic |
| Street light control output | Output | Output defined by the RTL |
| Status or fault outputs, if defined | Output | Status signals defined by the RTL |

Replace these functional descriptions with the actual port names from `smart_street_light.v` before submission.

## 3. Control Logic Used

The controller's behavior is determined by its Verilog implementation.

The relevant logic may include:

- Conditional switching logic.
- Delayed switching, if implemented in the RTL.
- Sequential state or counter logic, if implemented.
- Fault-handling behavior, if implemented.
- Output logic for controlling the street light.

The actual state machine, counter width, switching conditions, and delay threshold should be documented from the source code rather than assumed.

## 4. Tools Used

| Stage | Tool |
|---|---|
| RTL design | Verilog HDL |
| Simulation | Icarus Verilog |
| Waveform viewing | GTKWave |
| Logic synthesis | Yosys through the configured flow |
| Timing analysis | OpenSTA through the configured flow |
| Physical design | OpenLane |
| Process design kit | SkyWater SKY130A |
| Standard-cell library | `sky130_fd_sc_hd` |
| Layout viewing and physical checks | Magic and other tools used in the flow |

## 5. Project Folder Structure

The intended project organization is:

```text
07_Smart_Street_Light_Controller/
├── README.md
├── rtl/
│   └── smart_street_light.v
├── testbench/
│   └── tb_smart_street_light.v
├── docs/
│   └── design_documentation.md
├── screenshots/
│   ├── 01_rtl_simulation.png
│   ├── 02_waveform.png
│   ├── 03_synthesis.png
│   ├── 04_floorplan.png
│   ├── 05_placement.png
│   ├── 06_routing.png
│   ├── 07_timing_report.png
│   ├── 08_physical_verification.png
│   └── 09_final_gds.png
└── reports/
    ├── final_summary_report.csv
    ├── timing/
    └── physical_verification/
```

This is the proposed submission structure. Add the source files, reports, and screenshots when the actual artifacts have been collected.

## 6. Test Cases

Functional tests should be based on the implemented design and its specification.

Suggested checks, where applicable:

1. Reset behavior.
2. Normal switching operation.
3. Switching transition conditions.
4. Delayed switching behavior.
5. Fault detection and response.
6. Boundary conditions for any implemented delay or counter.
7. Recovery or reset following a fault.

Record only the tests actually implemented and run. Include the expected behavior and observed result for each test.

## 7. RTL Design

**Design name:** `smart_street_light`

**Expected source location:** `rtl/smart_street_light.v`

The RTL describes the digital control behavior of the street light system. The module's ports, registers, combinational logic, and sequential logic should be documented directly from the source.

Add a screenshot of the actual Verilog source or a relevant code excerpt here.

![RTL Design](screenshots/01_rtl_simulation.png)

## 8. Testbench

**Expected source location:** `testbench/tb_smart_street_light.v`

The testbench should provide the clock and input sequences required to exercise the controller and observe its outputs.

Document the scenarios covered by the testbench, along with the simulation command and actual pass/fail output.

## 9. Simulation

If Icarus Verilog was used, the simulation can be run from the directory containing the RTL and testbench:

```bash
iverilog -o sim.out rtl/smart_street_light.v testbench/tb_smart_street_light.v
vvp sim.out
```

The command assumes the filenames and module structure match those shown above. Adjust the paths if your actual files differ.

**Result:** Insert the observed simulation outcome here. Do not claim all tests passed without the corresponding output.

## 10. Waveform

A waveform viewer can help inspect the timing relationship between input changes and street light control outputs.

If a VCD waveform was generated and GTKWave is installed, it can be opened with:

```bash
gtkwave dump.vcd
```

Replace `dump.vcd` with the actual waveform filename.

![Simulation Waveform](screenshots/02_waveform.png)

## 11. Synthesis

The RTL-to-GDSII flow includes logic synthesis, which maps the RTL into a gate-level representation using the selected standard-cell library.

The synthesis report can be used to examine the mapped cell count, area estimates, and synthesis warnings.

![Synthesis Results](screenshots/03_synthesis.png)

Record the actual synthesis statistics from the generated reports.

## 12. Pre-Layout Static Timing Analysis

Pre-layout static timing analysis evaluates timing paths before final physical routing, based on the constraints and implementation stage.

Relevant evidence may include:

- Clock period and timing constraints.
- Setup slack and hold slack.
- Critical timing paths.
- Timing warnings and unconstrained paths.

![Pre-Layout Timing Analysis](screenshots/07_timing_report.png)

Insert the actual report values when available.

## 13. Physical Design (OpenLane)

The recorded OpenLane run for the design is:

`10-10_09-03`

The flow was executed using the SKY130A PDK and the `sky130_fd_sc_hd` standard-cell library.

### 13.1 Floorplanning

Floorplanning establishes the core and die regions and prepares the design for physical implementation.

![Floorplan](screenshots/04_floorplan.png)

### 13.2 Placement

Placement assigns physical locations to the standard cells within the floorplan.

![Placement](screenshots/05_placement.png)

### 13.3 Clock Tree Synthesis

Clock-tree synthesis is applicable when the configured flow includes a clock tree. Include evidence from the actual run if this stage was executed.

### 13.4 Routing

Routing creates the physical interconnections between the placed cells.

![Routing](screenshots/06_routing.png)

The actual run reports should be used to document the completion status and any routing violations.

## 14. Post-Route Static Timing Analysis

Post-route timing analysis evaluates timing after physical interconnections have been implemented.

Record the available setup and hold slack, critical paths, timing corners, and any remaining violations from the final run's reports.

Do not infer timing closure from GDS generation alone.

## 15. Final Layout (GDS)

The OpenLane run generated a GDSII layout artifact for the Smart Street Light Controller.

The previously observed file was approximately **306 KB**.

The original artifact was reported at:

```text
designs/smart_street_light/runs/10-10_09-03/
results/magic/smart_street_light.gds
```

Confirm that this path and file exist in the original VSD environment before using it as the final artifact location.

The GDSII file can be inspected with a compatible layout viewer such as Magic or KLayout when the necessary technology files are configured.

![Final GDS Layout](screenshots/09_final_gds.png)

## 16. Signoff Summary

Use the final summary report and physical-verification outputs from the recorded run.

| Check | Result |
|---|---|
| OpenLane flow status | Previously reported as `Flow_completed`; verify the final run |
| GDSII generation | Generated; previously observed file approximately 306 KB |
| DRC | Enter the actual final report result |
| LVS | Enter the actual final report result |
| Antenna checks | Enter the actual final report result |
| Setup timing | Enter the actual final report value |
| Hold timing | Enter the actual final report value |

Do not copy verification results or timing values from another project.

## 17. Implementation Results

The previously observed implementation produced a GDSII layout artifact of approximately 306 KB.

The final summary report was also inspected during the earlier project work. For a reproducible submission, include the actual report file and screenshots of the relevant final results.

Suggested report location:

`reports/final_summary_report.csv`

Use the actual CSV and report filenames from the recorded OpenLane run.

## 18. Additional Verification

Additional verification may include:

- Inspection of the final GDS layout.
- Review of DRC, LVS, and antenna reports.
- Review of post-route timing paths.
- Additional simulation of switching and fault scenarios.

Only document checks that were actually executed. No ngspice verification result is currently established for this project.

## 19. Feasibility

This project demonstrates the feasibility of describing digital street light control logic in Verilog and processing it through an automated ASIC implementation flow.

The implementation provides experience with RTL design, synthesis, placement, routing, timing analysis, and GDSII generation.

Practical deployment would require additional validation of the control specification, electrical interfaces, sensor integration where applicable, power requirements, and hardware behavior.

## 20. Conclusion

The Smart Street Light Controller project demonstrates the RTL-to-GDSII implementation workflow using Verilog HDL, OpenLane, and the SKY130A technology platform.

The recorded run generated a final GDSII layout artifact of approximately 306 KB. The final quality of the design should be evaluated using its actual functional simulation, timing, and physical-verification reports.

The project provides a foundation for further work on switching control, fault handling, and physical implementation of digital control systems.

## Author

**Project:** Smart Street Light Controller Using Verilog HDL  
**Workshop:** RTL Design Workshop  
**Technology:** SKY130A  
**Implementation Flow:** OpenLane RTL-to-GDSII
