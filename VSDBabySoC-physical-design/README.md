# VSDBabySoC – SKY130 HD Physical Design and Static Timing Analysis

## 1. Project Overview

This project documents the physical-design flow and Static Timing Analysis (STA) of the VSDBabySoC design using the SKY130 HD standard-cell library.

The objective is to understand the RTL-to-GDSII implementation flow, examine timing reports, and study setup and hold timing requirements using the available design tools.

This repository contains project documentation, screenshots, and reports collected during the practical work.

## 2. Physical Design Flow

The RTL-to-GDSII flow may include the following stages, depending on the stages completed in the project:

1. RTL synthesis
2. Floorplanning
3. Power planning
4. Placement
5. Clock Tree Synthesis (CTS)
6. Routing
7. Parasitic extraction
8. Static Timing Analysis (STA)
9. Physical verification
10. GDSII layout generation

Each stage contributes to transforming the design into a physical implementation suitable for further analysis and verification.

**Physical Design Screenshot**

<img width="600"  alt="s" src="https://github.com/user-attachments/assets/16de77aa-036e-4838-80eb-df4be811bd93" />




## 3. Multi-Corner Static Timing Analysis

Static Timing Analysis evaluates whether signal paths satisfy the timing constraints defined for a digital design.

Multi-corner analysis examines timing under different process, voltage, and temperature (PVT) conditions when the design flow and available reports support it.

The analysis helps identify timing concerns and assess whether the relevant setup and hold requirements are satisfied.

**STA Terminal Screenshot**

<img width="700" alt="VSDBabySoC_STA_Timing" src="https://github.com/user-attachments/assets/b134b607-c271-4d38-ad86-acb79dfb36f9" />


<img width="700" alt="W" src="https://github.com/user-attachments/assets/95350d1e-c9fa-4f5d-ba95-f07dcc6834ad" />


## 4. Setup and Hold Timing

### 4.1 Setup Timing

Setup timing checks whether data reaches a register sufficiently early before the active clock edge.

A negative setup slack indicates a setup timing violation for the reported path and analysis conditions.

### 4.2 Hold Timing

Hold timing checks whether data remains stable for the required interval after the active clock edge.

A negative hold slack indicates a hold timing violation for the reported path and analysis conditions.

The actual timing report should be used to determine whether setup and hold requirements pass or fail.

## 5. Reported Timing Results

Record the values from your own STA report in the table below.

| Timing Metric | Actual Reported Value |
|---|---|
| Analysis corner | Add the corner from your report |
| Worst Negative Slack (WNS) | Add the actual value |
| Total Negative Slack (TNS) | Add the actual value |
| Worst hold slack | Add the actual value |
| Setup timing status | Add the reported status |
| Hold timing status | Add the reported status |

## 6. Timing Analysis Pictures

<img width="600" alt="h" src="https://github.com/user-attachments/assets/a9e96b5a-e17e-486a-93e0-ec158d53eefb" />



## 7. Results and Observations

Summarize the physical-design stages completed and the timing results observed in the actual tool reports.

Discuss any setup or hold violations only if they appear in the reports. Include relevant observations about the implementation flow and the results obtained.

The final implementation status should be based on the available tool logs, reports, and generated design files.

## 8. Conclusion

This project documents the VSDBabySoC physical-design workflow and timing-analysis concepts associated with the SKY130 HD standard-cell library.

The pictures and reports provide evidence of the practical work performed. The final conclusions will be based on the actual implementation stages completed and the timing results obtained.

