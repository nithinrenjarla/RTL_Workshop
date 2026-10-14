 # Physical-Design-Module-1
# Complete ASIC Physical Design Flow Using OpenLane and SKY130

# Project Overview
This project presents the implementation of a digital design using the ASIC PPHYSICAL DESIGNhysical Design flow with OpenLane and the SkyWater SKY130 PDK.

The project covers the major stages involved in converting a synthesized digital design into a physically implemented layout. The flow includes floorplanning, power planning, placement, Clock Tree Synthesis (CTS), routing, timing analysis, antenna checking, and physical verification.

The main purpose of this project is to gain practical understanding of the ASIC physical design process using open-source EDA tools.

# Objectives
The objectives of this project are:

To understand the ASIC Physical Design flow.

To perform floorplanning and power planning.

To understand standard-cell placement and optimization.

To perform Clock Tree Synthesis.

To perform global and detailed routing.

To analyze timing using Static Timing Analysis.

To understand and resolve antenna rule violations.

To perform physical verification using DRC and LVS.

To understand the role of open-source EDA tools in ASIC implementation.

To analyze the effect of different physical design configurations on the final implementation.

# Tools and Technologies
| Tool / Technology |	Purpose |
|---|---|
| OpenLane |	Automated ASIC implementation flow |
| SKY130 PDK |	Target process technology |
| OpenROAD |	Physical design implementation |
| Yosys |	Logic synthesis |
| OpenSTA |	Static Timing Analysis |
| Magic |	Design Rule Checking |
| Netgen |	Layout Versus Schematic |
| SPEF |	Parasitic extraction and timing analysis |

# Table of Contents
1 - Floorplanning

2 - Power Planning

3 - Placement

4 - Clock Tree Synthesis

5 - Routing

6 - Antenna Rule Checking

7 - Antenna Violation Repair

8 - Static Timing Analysis

9 - Parasitic Extraction

10 - Logic Equivalence Check

11 - Physical Verification

12 - OpenLane Design Exploration

13 - OpenLane ASIC Flow

14 - OpenLane and OpenROAD

15 - SKY130 PDK

16 - OpenLane Design Configuration

17 - Floorplan Configuration Parameters

18 - Design for Test (DFT)

19 - Physical Implementation

20 - Project Execution

21 - Physical Design Results

22 - Verification Summary

23 - Design Space Exploration

24 - OpenLane Regression Testing

25 - Final Physical Design Flow

26 - Terminal Execution and Command Evidence

27 - Key Learnings

28 - Conclusion

# Physical Design Flow
The physical design process follows a sequence of implementation and verification stages:
```text
Synthesis
    ↓
Floorplanning
    ↓
Power Planning
    ↓
Placement
    ↓
Clock Tree Synthesis
    ↓
Routing
    ↓
Parasitic Extraction
    ↓
Static Timing Analysis
    ↓
DRC / LVS / Antenna Checks
    ↓
Final Layout
```
Each stage has a specific role in converting the synthesized design into a physically valid layout.

# 1. Floorplanning
Floorplanning is the first major stage of physical design. It defines the physical dimensions of the core and die and determines how the design will be organized inside the available area. The important floorplanning parameters
include: 
FP_CORE_UTIL 

FP_ASPECT_RATIO FP_SIZING 

FP_IO_MODE FP_IO_HMETAL

FP_IO_VMETAL FP_PDN_*

These parameters control the core utilization, aspect ratio, I/O placement, metal layers, and power distribution network configuration. Floorplan Configuration The floorplan configuration is controlled using the OpenLane configuration and floorplan settings.
<img width="700" alt="Screenshot (187)" src="https://github.com/user-attachments/assets/29803410-9386-4071-be90-6dd60d369891" />


#  2. Power Planning
Power planning establishes the power distribution network (PDN) required to supply power to the standard cells.

The PDN provides reliable connections for:

- VDD 
- VSS
- Standard-cell power rails
- Core power distribution
 Proper power planning is important for reliable operation of the implemented design.
<img width="700" alt="Screenshot (179)" src="https://github.com/user-attachments/assets/ae8fe4fc-7b99-4b2e-bf9d-9727ca79141c" />


# 3. Placement
Placement determines the physical locations of the standard cells within the floorplan.

The placement process generally consists of:

# Global Placement
Global placement distributes the cells while considering wirelength, timing, and congestion.

# Placement Optimization
The placement is optimized to improve timing and routing characteristics.

# Detailed Placement
Detailed placement legalizes the cell locations according to the physical constraints of the technology.
<img width="700" alt="Screenshot 2026-09-09 224018" src="https://github.com/user-attachments/assets/dc47e268-e60c-4818-99d4-7e92a94471bd" />


# 4. Clock Tree Synthesis
Clock Tree Synthesis (CTS) creates a clock distribution network for sequential elements in the design.

The main objectives of CTS are:

- Reduce clock skew.

- Control clock latency.

- Maintain acceptable clock transition.

- Distribute the clock signal reliably to sequential cells.

Clock buffers and other cells may be inserted during CTS to achieve the required clock distribution.
<img width="700" alt="Screenshot 2026-09-09 224149" src="https://github.com/user-attachments/assets/b0aebc34-b22f-4b8e-86a7-e377714357f3" />


# 5. Routing
Routing creates the physical metal connections between the placed standard cells.

Routing is performed in two major stages:

# Global Routing
Global routing determines the approximate paths for the connections and evaluates routing congestion.

# Detailed Routing
Detailed routing creates the final physical connections while satisfying the design rules of the target technology.

Successful routing is necessary for generating a valid physical layout.
<img width="700" alt="Screenshot (180)" src="https://github.com/user-attachments/assets/de023bad-0cad-4743-9415-c8dcf3429a86" />


# 6. Antenna Rule Checking
During the fabrication process, long metal connections can accumulate electrical charge. This can cause damage to the gate oxide of MOS transistors and is known as the antenna effect.

Therefore, antenna violations are checked after routing.

Antenna violations can be addressed using antenna diode cells. The diode provides a discharge path and helps protect the gate during fabrication.

The OpenLane flow supports antenna checking and repair as part of the physical implementation process.
<img width="700" alt="Screenshot (186)" src="https://github.com/user-attachments/assets/76545fe2-57ef-4521-83f5-672ae3294c13" />


# 7. Antenna Violation Repair
When an antenna violation is identified, an antenna diode can be used to fix the violation.

The general approach is:
```text
Antenna Violation
       ↓
Violation Detection
       ↓
Antenna Diode Insertion
       ↓
Routing / Optimization
       ↓
Antenna Re-check
```
# 8. Static Timing Analysis
Static Timing Analysis (STA) is used to verify whether the implemented design satisfies its timing constraints.

Timing analysis is performed at different stages of the physical design flow, including after synthesis and after routing.

The important timing parameters include:

- Setup time

- Hold time

- Clock latency

- Data arrival time

- Data required time

- Slack

A positive timing slack generally indicates that the corresponding timing constraint is satisfied.
<img width="700" alt="Screenshot (185)" src="https://github.com/user-attachments/assets/1ac4f71c-3e6a-4d50-9ba9-10f2e4ed8fcc" />


# 9. Parasitic Extraction
After routing, the physical interconnects introduce parasitic resistance and capacitance.

These parasitic effects are extracted from the routed design and represented using a Standard Parasitic Exchange Format (SPEF) file.

The extracted parasitic information is used during post-route timing analysis to obtain more realistic timing results.
```text
Routed Design
     ↓
Parasitic Extraction
     ↓
SPEF
     ↓
OpenSTA
     ↓
Post-route Timing Analysis
```
# 10. Logic Equivalence Check
Logic Equivalence Check (LEC) is used to verify that modifications made during physical implementation have not changed the intended logical functionality.

Physical design stages such as optimization and Clock Tree Synthesis can modify the netlist.

LEC compares the reference design with the modified implementation to ensure functional equivalence.
```text
Reference Netlist
       |
       |  LEC
       |
Implemented Netlist
       ↓
Functional Equivalence
```
# 11. Physical Verification
Physical verification ensures that the final layout satisfies manufacturing and connectivity requirements.

The major verification checks performed are:

# Design Rule Check (DRC)
DRC verifies whether the layout follows the manufacturing rules of the SKY130 technology.

It checks physical constraints such as:

- Minimum metal width
- Minimum spacing
- Via rules
- Layer-specific restrictions
- Other geometry constraints
  
# Layout Versus Schematic (LVS)
LVS compares the extracted layout connectivity with the intended circuit netlist.

A successful LVS indicates that the physical implementation represents the intended circuit correctly.

<img width="700" alt="Screenshot (181)" src="https://github.com/user-attachments/assets/34a4c7ef-1c67-45d5-94c9-ebca0be338ca" />


# 12. OpenLane Design Exploration
OpenLane supports design-space exploration by allowing different implementation parameters to be evaluated.

Parameters such as core utilization, aspect ratio, and other floorplanning settings can affect the final implementation.

The main metrics considered during exploration include:

- Area
- Cell count
- Utilization
- Timing
- Routing congestion
- Design-rule violations
The results can be compared to select a suitable configuration for the design.
<img width="700" alt="Screenshot (184)" src="https://github.com/user-attachments/assets/5d4b7f32-2d4f-46a6-9df6-db6c746f3011" />


# 13. OpenLane ASIC Flow
OpenLane is an automated RTL-to-GDSII flow that integrates multiple open-source EDA tools into a unified ASIC implementation flow.

It provides an automated sequence of synthesis, floorplanning, placement, CTS, routing, extraction, timing analysis, and physical verification.

The flow is designed to simplify ASIC implementation and enable reproducible physical design experiments.
<img width="700" alt="Screenshot (182)" src="https://github.com/user-attachments/assets/513bf209-0e1c-4cc1-bf73-83fae3e26445" />


# 14. OpenLane and OpenROAD
OpenLane uses OpenROAD as the primary physical implementation engine for several stages of the ASIC flow.

OpenROAD provides capabilities for:

- Floorplanning
- Power planning
- Placement
- Optimization
- Clock Tree Synthesis
- Routing
- Physical implementation
Other open-source tools are integrated with OpenLane to complete the overall flow.

# 15. SKY130 PDK
The project uses the SkyWater SKY130 Process Design Kit (PDK) as the target technology.

The PDK provides the technology-specific information required by the ASIC implementation tools, including:

- Standard-cell libraries
- Technology LEF files
- Liberty timing libraries
- Design rules
- Layer information
- Physical abstracts
These files allow the design to be synthesized, placed, routed, and verified according to the target semiconductor technology.
<img width="700" alt="Screenshot (183)" src="https://github.com/user-attachments/assets/c4235302-d495-447c-991f-93ee1d995d42" />


# 16. OpenLane Design Configuration
OpenLane uses configuration variables to control different stages of the physical design flow.

The configuration file defines design-specific parameters and allows the implementation flow to be reproduced consistently.

Important configuration categories include:

- Design and source configuration
- Clock configuration
- Floorplan configuration
- Placement configuration
- Routing configuration
- Timing constraints
- Power planning parameters
The configuration is selected according to the requirements of the design and the target technology.

# 17. Floorplan Configuration Parameters
The floorplan can be controlled using OpenLane environment variables.

Some important parameters used for floorplanning are:

| Parameter | Description |
|---|---|
| `FP_CORE_UTIL` | Controls the target core utilization |
| `FP_ASPECT_RATIO` | Defines the core aspect ratio |
| `FP_SIZING` | Controls floorplan sizing mode |
| `FP_IO_MODE` | Defines I/O placement mode |
| `FP_IO_HMETAL` | Defines horizontal I/O metal layer |
| `FP_IO_VMETAL` | Defines vertical I/O metal layer |
| `FP_PDN_*` | Controls power distribution network settings |
Proper selection of these parameters affects the available area, placement density, congestion, and routability of the design.

# 18. Design for Test (DFT)
Design for Testability (DFT) techniques improve the ability to test and diagnose an integrated circuit after fabrication.

Scan-based testing is commonly used to provide controllability and observability of internal sequential elements.

DFT is considered as part of the ASIC implementation flow before final verification.


# 19. Physical Implementation
The physical implementation stage integrates the major physical design operations required to transform the synthesized design into a routed layout.

The main operations include:

- Floorplanning
- Power planning
- Placement
- Clock Tree Synthesis
- Routing
- Physical optimization
- 
OpenLane automates these stages while using technology-specific constraints from the SKY130 PDK.

# 20. Project Execution
The OpenLane flow was executed by providing the required RTL design, technology files, and configuration parameters.

The flow was run through the OpenLane flow script, which automatically invokes the required tools for each stage of physical implementation.

The execution generates intermediate files, logs, reports, and final physical design outputs.

A typical OpenLane run contains:
```text
Design
 ├── src/
 ├── config.tcl
 └── runs/
      └── <run_directory>/
           ├── results/
           ├── reports/
           ├── logs/
           └── tmp/
```

# 21. Physical Design Results
After completing the physical implementation flow, the generated results and reports are analyzed to evaluate the quality of the design.

The important implementation metrics include:

| Metric | Purpose |
|---|---|
| Area | Measures the physical size of the implemented design |
| Cell Count | Number of standard cells used |
| Utilization | Percentage of the core occupied by cells |
| Timing | Determines whether timing constraints are satisfied |
| Slack | Indicates the available timing margin |
| Routing | Confirms successful physical connectivity |
| DRC | Checks manufacturing design rules |
| LVS | Checks layout-to-netlist connectivity |
| Antenna | Checks fabrication-related antenna violations |

These metrics are used to evaluate the quality and correctness of the final physical implementation.

# 22. Verification Summary
The implemented design is evaluated through multiple verification stages.

| Verification | Objective |
|---|---|
| STA | Verify timing constraints |
| LEC | Verify logical equivalence |
| DRC | Verify physical design rules |
| LVS | Verify layout connectivity |
| Antenna Check | Identify antenna rule violations |

These checks help ensure that the final physical implementation is logically correct, physically valid, and suitable for further ASIC sign-off activities.

# 23. Design Space Exploration
Different physical design configurations can produce different implementation results.

OpenLane provides design exploration capabilities to study the effect of configuration parameters on:

- Area
- Utilization
- Timing
- Cell density
- Routing congestion
- Overall implementation quality
For example, changing the core utilization or aspect ratio can influence placement density and routing congestion.

The exploration process helps in selecting a configuration that provides a suitable balance between area, timing, and routability.

<img width="700" alt="Screenshot (189)" src="https://github.com/user-attachments/assets/1d6db6c5-b173-4423-b6cd-d802a0873d58" />

# 24. OpenLane Regression Testing
Regression testing is used to verify that changes in the flow, tools, or configuration do not introduce unexpected failures.

OpenLane provides a regression testing mechanism for evaluating designs across different flow configurations.

This helps improve the reliability and reproducibility of the ASIC implementation flow.

<img width="700" alt="Screenshot (188)" src="https://github.com/user-attachments/assets/2a1bbdbe-e937-4190-97df-0b5eb1762150" />

# 25. Final Physical Design Flow

The complete physical design process studied in this project can be summarized as:
```text
             RTL Design
                  |
                  ↓
              Synthesis
                  |
                  ↓
            Floorplanning
                  |
                  ↓
           Power Planning
                  |
                  ↓
              Placement
                  |
                  ↓
                 CTS
                  |
                  ↓
               Routing
                  |
                  ↓
        Parasitic Extraction
                  |
                  ↓
                 STA
                  |
                  ↓
          Antenna Checking
                  |
                  ↓
             DRC / LVS
                  |
                  ↓
          Final Layout / GDSII
```

# 26. Terminal Execution and Command Evidence

The following terminal screenshots provide evidence of the commands executed during the physical design setup and OpenLane flow.

# 26.1 PDK Directory and Library Setup

<img width="700" alt="library" src="https://github.com/user-attachments/assets/7f7015c5-eb06-42dd-8c5c-07d2bb988a66" />

# 26.2 OpenLane Environment Setup

<img width="700" alt="openlane" src="https://github.com/user-attachments/assets/c0b59713-5af8-44ae-ae79-669d19455ec9" />
 
# 26.3 LEF and Library File Verification

<img width="700" alt="LEF" src="https://github.com/user-attachments/assets/db20a8d4-129a-4c66-a692-0914429030d5" />

# 26.4 OpenLane Flow Execution

<img width="700" alt="flow execution" src="https://github.com/user-attachments/assets/03e9eeb3-570f-4301-9ec5-c81d8a4bdf80" />

# 26.5 OpenLane Configuration 

<img width="700" alt="configuration" src="https://github.com/user-attachments/assets/91c75a16-d417-44d1-b447-cdd4f2e3543a" />

# 26.6 Floorplan Configuration

<img width="700" alt="floorplan" src="https://github.com/user-attachments/assets/b395a7cf-4792-4629-8384-5521ec0444a6" />

# 27. Conclusion

This project provided practical exposure to the ASIC Physical Design flow using OpenLane and the SKY130 PDK.

The major physical design stages, from floorplanning to final verification, were studied and implemented using open-source EDA tools.

The project demonstrated the importance of proper floorplanning, placement, clock distribution, routing, timing analysis, and physical verification in achieving a reliable ASIC implementation.

The study also showed that physical design parameters have a direct impact on important design metrics such as area, timing, utilization, and routability.

Overall, the project provided a practical understanding of the complete ASIC physical implementation methodology and the role of open-source tools in modern VLSI design.
