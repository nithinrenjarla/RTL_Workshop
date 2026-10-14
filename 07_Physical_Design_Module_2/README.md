# Physical-Design-Module-2
Power Integrity and Chip Floorplanning

# Project Overview
This project is part of the Physical Design (PD) learning journey and focuses on the fundamental concepts of ASIC Chip Floorplanning and Power Integrity.

The module explores how a logical netlist is transformed into a physical representation by defining the chip core and die dimensions, calculating cell area, setting utilization and aspect ratio, and organizing standard cells and pre-placed IP blocks within the floorplan.

The project also covers important power integrity concepts such as switching current, IR drop, inductive voltage drop, noise margin, and decoupling capacitors. In addition, practical aspects of the ASIC physical design flow are explored using the OpenLane flow, OpenROAD-based layout tools, and the SKY130 technology and standard-cell library.

The later stages of the module demonstrate floorplanning, power planning, PDN creation, standard-cell placement, placement blockages, tap cells, LEF and technology files, timing constraints, and binding logical netlist cells with physical library cells.

Overall, this project provides a step-by-step understanding of how an ASIC design progresses from a logical netlist to a structured physical layout.

# Table of Contents
1 - Define Width and Height of Core and Die 

2 - Convert Netlist Symbols into Physical Dimensions

3 - Calculate Area Occupied by the Netlist

4 - Utilization Factor and Aspect Ratio

5 - Core and Die Dimension Example

6 - Define Locations of Pre-placed Cells

7 - Placement of Pre-placed Cells

8 - IP Blocks and Floorplanning

9 - Surrounded Pre-Placed Cells with Decoupling Capacitors

10 - Switching Current and Voltage Drop

11 - Noise Margin

12 - Solution:Add Decoupling Capacitors

13 - Decoupling Capacitor Placement Around Blocks

14 - Decoupling Capacitor Placement in the Floorplan

15 - Power Network,Driver,Load and 16-bit Bus

16 - Floorplanning

17 - Power Planning

18 - Power Distribution Network

19 - Picorv32a ASIC Design Flow using OpenLane

20 - OpenLane Physical Design Configuration

21 - SkyWater PDK LEF File Configuration

22 - OpenLane Floorplanning Configuration

23 - Standard Cell Placement

24 - Decoupling Capacitors

25 - Logical Cell Placement Blockage

26 - Tap Cells

27 - LEF & Technology Files

28 - OpenLane Configuration

29 - Timing Constraints

30 - OpenROAD / Layout View

31 - Bind Netlist with Physical Library Cells

32 - Placement

# Tools and Technologies

| Tool / Technology |	Purpose |
|---|---|
| **OpenLane** | Automated open-source RTL-to-GDSII ASIC design flow |
| **OpenROAD** | Physical design implementation and layout visualization |
| **SKY130 PDK** |	Open-source 130 nm process design kit |
| **sky130_fd_sc_hd** |	High-density standard-cell library |
| **LEF Files**	|Physical abstraction of standard cells, pins, layers, and obstructions |
| **Tcl** |	Configuration and automation of the OpenLane flow |
| **Verilog** |	Hardware description of the design |
| **SDC** |	Timing and clock constraint definition |
| **KLayout / Layout Viewer** |	Visualization and inspection of physical layouts |
| **Linux / Ubuntu** |	Development and execution environment |
| **GitHub** |	Project documentation and version control |

# 1. Define Width and Height of Core and Die
The first step in physical design is to understand the netlist and convert the logical representation of the design into physical dimensions.

A netlist describes the connectivity between different components of an electronic design.

The example netlist contains:

Flip-Flops (FF)
AND gate
OR gate
Clock connection
Data connections
The standard cells and flip-flops in the netlist are later converted into physical dimensions during floorplanning.

<img width="700" alt="height and width" src="https://github.com/user-attachments/assets/a47f2d65-de63-42bd-865c-f20e5e9dfc8f" />


# 2. Convert Netlist Symbols into Physical Dimensions
After understanding the netlist, the logical components are represented as physical standard cells.

The highlighted elements include:

Flip-Flops
Standard cells
AND/OR logic cells
Each logical cell occupies a certain physical area on the silicon.

Therefore, the total area occupied by all cells must be calculated before determining the core dimensions.

<img width="700" alt="netlist symbols" src="https://github.com/user-attachments/assets/f7ae9bd9-1e7d-4f2a-997a-1c709cd8b79e" />


# 3. Calculate Area Occupied by the Netlist
For the given example, each standard cell and flip-flop is represented as a unit square.

For example:
``` text 
Width  = 1 unit
Height = 1 unit

Area = Width × Height
     = 1 × 1
     = 1 sq. unit
```
The total area occupied by the netlist is calculated by adding the area of all standard cells and flip-flops. This gives the total cell area required inside the core.

<img width="700" alt="calculate area" src="https://github.com/user-attachments/assets/6e9195ca-2213-4dba-a88b-0b5fcb66e2e6" />


# 4. Utilization Factor and Aspect Ratio
Two important parameters used to define the core dimensions are: Utilization Factor
```text
Utilization Factor =
Area Occupied by Netlist
-------------------------
Total Area of Core
```
<img width="700" alt="aspect ratio" src="https://github.com/user-attachments/assets/5e34f6b3-c148-47db-8a3e-3bdbc5971c2c" />
The utilization factor indicates how much of the core area is occupied by the placed cells. Aspect Ratio Aspect Ratio = Height / Width For the example shown, the core and die dimensions are selected based on the required utilization factor and aspect ratio. The diagram illustrates a core of approximately 4 units × 2 units with the die surrounding the core.

# 5. Define Locations of Pre-placed Cells
Some cells or blocks cannot be freely placed by the automated placement tool. These cells are known as pre-placed cells. Examples include: Memory Clock-gating cells Comparators Multiplexers Other large IP blocks These blocks have user-defined locations and are placed before automated placement and routing.

<img width="700" alt="pre-placed cells" src="https://github.com/user-attachments/assets/0d4f065a-f921-41e5-9c24-f7a3a1747832" />


# 6. Core and Dimension Example
Another example demonstrates how the dimensions of the core and die change according to the utilization factor. The physical dimensions are selected so that sufficient space is available for: Standard-cell placement Routing Power distribution Decoupling cells Other physical-design requirements A lower utilization factor provides more free area inside the core, which can help in reducing placement and routing congestion.

<img width="700" alt="core and dimension" src="https://github.com/user-attachments/assets/edf7414d-95de-4ea8-9223-7a8eff76d959" />


# 7. IP Blocks and Floorplanning
Modern ASIC designs may contain several pre-designed IP blocks. Examples include: Memory Clock-gating cells Comparator Multiplexer The arrangement of these IPs or blocks inside the chip is called floorplanning. These IPs have user-defined locations and are placed in the chip before automated placement and routing. Therefore, floorplanning determines the physical organization of major blocks inside the chip.

<img width="700" alt="IP Blocks" src="https://github.com/user-attachments/assets/99d30675-5f09-4cf1-ae09-d3da7ae1eed6" />


# 8. Placement of Pre-placed Cells
The location of pre-placed cells is important because their position affects the rest of the physical design. The example shows two blocks containing multiple logic cells. The cells and their connections are arranged so that the required input/output connections can be maintained. I/O pins can be extended to make the connectivity between blocks clear. Proper placement helps reduce: Routing congestion Wire length Timing problems Unnecessary routing detours

<img width="700" alt="pre-placed Cells" src="https://github.com/user-attachments/assets/a45e5e5b-6727-42b5-a7dc-cd8a6204f711" />


# 9. Surround Pre-placed Cells with Decoupling Capacitors
Pre-placed blocks may experience high switching activity and can require a large instantaneous current. To improve power integrity, decoupling capacitors can be placed around the pre-placed cells. The floorplan contains: Core Die Block A Block B Block C Decoupling capacitor regions The decoupling capacitors provide a local source of charge near the blocks. This helps reduce supply voltage fluctuations during switching.

<img width="700" alt="Decoupling capacitors" src="https://github.com/user-attachments/assets/fd2bd47c-26d1-4147-97fa-edce491becba" />


# 10. Switching Current and Voltage Drop
During switching operation, a complex digital circuit may demand a large amount of instantaneous current. This is called peak switching current. The power supply network contains parasitic resistance and inductance. When switching current flows through these elements, a voltage drop occurs. The resistive voltage drop is:
```text
V = I × R
```

The inductive voltage variation is:

```text
V = L × di/dt
```
Therefore, because of the resistance and inductance of the power network, the voltage available at the circuit can become lower than the ideal supply voltage.

<img width="700" alt="I and R" src="https://github.com/user-attachments/assets/68363711-aa09-4a61-bab3-c90ad0dbf779" />


# 11. Noise Margin
Noise margin represents the ability of a digital circuit to tolerate unwanted voltage disturbances without changing the interpreted logic value. The two important noise margins are: Noise Margin High

```text
NMH = VOH(min) − VIH(min)
```

Noise Margin Low

```text
NML = VIL(max) − VOL(max)
```
The diagram shows different noise-induced bumps and their relationship with the noise-margin levels. A small noise bump remains within the acceptable region and does not cause a logic error. If the noise exceeds the available noise margin, it may be interpreted as an unwanted logic transition.

<img width="700" alt="Noise Margin" src="https://github.com/user-attachments/assets/2b52a8ab-0b12-4de1-a99f-f3e9c104adea" />


# 12. Solution: Add Decoupling Capacitors
One solution for reducing the effect of switching-current demand is to add a decoupling capacitor. The decoupling capacitor is connected in parallel with the circuit. When the circuit switches and requires a large current, the capacitor can provide charge locally. The power network then replenishes the charge stored in the capacitor.
```text

Switching occurs
       ↓
Circuit demands current
       ↓
Decoupling capacitor supplies charge
       ↓
Power network replenishes the charge
```

<img width="700" alt="Add" src="https://github.com/user-attachments/assets/bcdb3d3b-4eea-4011-bc04-5c8120fd7f7c" />


# 13. Decoupling Capacitor Placement Around Blocks
Decoupling capacitors can be placed around important pre-placed blocks. For example: +--------------------------------+ | | | DECAP1 | | Block A Block B | | DECAP2 | | Block C | | DECAP3 | | | +--------------------------------+ The purpose is to keep the decoupling capacitors close to the blocks that require additional instantaneous current. This provides a local current source and improves power integrity.

<img width="700" alt="Placement" src="https://github.com/user-attachments/assets/1e34e191-b0e7-44d8-84e8-cae11398e31d" />


# 14. Decoupling Capacitor Placement in the Floorplan
The floorplan can be organized using different regions for blocks and decoupling capacitors.

The example shows:
```text
DECAP1
Block A
Block B
DECAP2
Block C
DECAP3
```
The decoupling capacitors are strategically placed around the pre-placed cells. Proper placement helps reduce the distance between the capacitor and the switching circuit. A shorter current path helps reduce the impact of parasitic resistance and inductance.

<img width="700" alt="placement in the floorplan" src="https://github.com/user-attachments/assets/244f9149-4652-4e27-a00e-6d52a533cac4" />


# 15. Power Network, Driver, Load and 16-bit Bus
The final example illustrates the power network connecting multiple driver and load circuits. The power distribution network contains resistance and inductance. Each circuit has associated capacitance and switching requirements. The diagram also illustrates a signal path representing a multi-bit bus. For the given example, the blue path represents a 16-bit bus. The key concept is that switching activity across multiple signals can create a large instantaneous current demand. Therefore, proper power-network design and decoupling are required to maintain stable supply voltage and reliable circuit operation

<img width="700" alt="power" src="https://github.com/user-attachments/assets/8f715971-fe53-498b-a249-54da47793c53" />


# 16. Power Planning
Power planning creates the power-distribution network (PDN) required to deliver stable supply voltages throughout the chip. The screenshots show a power grid consisting of horizontal and vertical metal structures. Typical power connections include:

```text
VDD
│
├── Horizontal Power Rails
│
├── Vertical Power Rails
│
└── Standard Cell Power Connections

VSS
│
├── Horizontal Ground Rails
│
├── Vertical Ground Rails
│
└── Standard Cell Ground Connections
```
The power grid helps reduce voltage drop and provides reliable power delivery to the cells distributed across the core.

<img width="700" alt="Power Planning" src="https://github.com/user-attachments/assets/97eb046d-52b9-4d2f-a840-eccc401133a0" />


# 17. Power Distribution Network (PDN)
The project examines the physical organization of power structures across the chip core. The PDN consists of: Horizontal power straps Vertical power straps Standard-cell power rails VDD connections VSS connections Power grid intersections A well-designed PDN is essential for: Reducing IR drop Improving power integrity Providing uniform supply voltage Supporting reliable standard-cell operation

<img width="700" alt="PDN" src="https://github.com/user-attachments/assets/60a43c95-e680-434a-be48-fbacc392e71f" />


# 18. Picorv32a ASIC Design Flow using OpenLane
OpenLane is an automated, open-source RTL-to-GDSII hardware design framework. It automatically transforms human-readable hardware description code (Verilog RTL) into the final physical layout blueprint (GDSII) required to manufacture a physical silicon microchip.The picorv32a is an optimized RISC-V CPU core used as a design benchmark in this automated flow.The config.tcl file acts as the configuration hub for this process. It defines critical hardware parameters—such as target layout names, input file paths, and the required clock speeds—to guide the software engines through synthesis, placement, and routing without human intervention.

<img width="700" alt="config tcl" src="https://github.com/user-attachments/assets/ce05cd3a-5201-42f2-84e8-ddee5d5dde92" />

# 20. Openlane Physical Design Configuration (sky130_fd_sc_hd)
In the OpenLane ASIC design flow, the hardware description language (HDL) code is transformed into a physical layout. This process relies heavily on configuration files (.tcl) to define constraints and optimization goals for the synthesis, floorplanning, placement, and routing stages.The configuration snippet specifically targets the sky130_fd_sc_hd standard cell library (SkyWater 130nm High Density) and establishes several foundational parameters:Synthesis & Timing Control: Variables like SYNTH_MAX_FANOUT define the maximum number of digital inputs that a single logic gate output can drive, balancing signal integrity and delay. The CLOCK_PERIOD sets the targeted clock cycle time in nanoseconds, defining the performance constraint for static timing analysis (STA).Floorplanning & Density: The utilization variables specify how much of the core area will be occupied by standard cells. The core utilization (FP_CORE_UTIL) sets the initial budget, while PL_TARGET_DENSITY dynamically calculates the targeted placement density, ensuring cells are optimally packed without causing unroutable congestion during the physical implementation stage.

<img width="500" alt="config tcl" src="https://github.com/user-attachments/assets/89bca7a9-0149-4367-b4ad-6b1ba43aee83" />


# 21.OpenLane Floorplanning Configuration

# Pin & IO Placement
FP_IO_HMETAL: Specifies the specific metal layer assigned to route the horizontal IO pins on the top and bottom edges of the die block. (Default: 4). FP_IO_VMETAL: Specifies the specific metal layer assigned to route the vertical IO pins on the left and right sides of the die block. (Default: 3). FP_IO_MODE: Determines the strategy for random IO pin placement. Setting it to 0 enables matching node placements, while 1 triggers random but equidistant placement along the core boundaries. (Default: 1). FP_IO_MIN_DISTANCE: Sets the absolute minimum physical spacing required between adjacent IO pins to avoid manufacturing design rule errors.

 # Power Distribution Network (PDN) & Pitch
FP_WELLTAP_CELL / FP_ENDCAP_CELL: The specific physical layout cell names used for tap and endcap insertion to prevent latch-up conditions. FP_PDN_VOFFSET / FP_PDN_HOFFSET: The offset measurements applied to the vertical and horizontal power stripes relative to the design origin. FP_PDN_VPITCH / FP_PDN_HPITCH: The recurring pitch/distance between parallel vertical and horizontal power stripes across the metal stack layers. FP_PDN_AUTO_ADJUST: A boolean switch determining if the flow should automatically scale and adjust the power grid layout to match the core boundaries when adjustments are necessary. (Default: 1 [Enabled]).

 # Taps, Tie-offs, and IO Extensions
FP_TAPCELL_DIST: Defines the horizontal distance limits between adjacent welltap columns across the layout row structures. (Default: 14). FP_IO_VEXTEND / FP_IO_HEXTEND: Extends the routing pins slightly outside the core/die perimeter to make external macro routing simpler. FP_IO_VLENGTH / FP_IO_HLENGTH: Dictates the absolute length of vertical and horizontal physical pins. (Default: 4). FP_IO_VTHICKNESS_MULT / FP_IO_HTICKNESS_MULT: A multiplier value scaling the thickness of pins over the standard minimum layer widths.

<img width="700" alt="floor planning" src="https://github.com/user-attachments/assets/e448144f-42af-4caf-bb40-eef051036227" />

# 22. Standard Cell Placement
After floorplanning and power planning, logical cells are placed inside the core region. The placement process determines the physical location of: Combinational cells Sequential cells Buffers Inverters Logic gates Other standard cells The screenshots demonstrate the placement of cells in organized rows inside the defined core area.

<img width="500" alt="f command" src="https://github.com/user-attachments/assets/f7b5cb78-bd29-46b9-895b-b94e4821b0ae" />

Good placement is important for achieving: Shorter interconnects Better timing Lower congestion Efficient routing Lower power consumption.

<img width="700" alt="fff" src="https://github.com/user-attachments/assets/01034e30-a816-4b06-9973-f2b66ca6f384" />

# 23. OpenLane Configuration
The project uses Tcl-based OpenLane configuration files to define the physical-design flow. Typical configuration variables include:
```text
set ::env(DESIGN_NAME) "picorv32a"

set ::env(VERILOG_FILES) \
    "$::env(DESIGN_DIR)/src/picorv32a.v"

set ::env(SDC_FILE) \
    "$::env(DESIGN_DIR)/src/picorv32a.sdc"

set ::env(CLOCK_PERIOD) "5.000"
set ::env(CLOCK_PORT) "clk"

set ::env(FP_CORE_UTIL) 50
set ::env(FP_ASPECT_RATIO) 1
```

# 24. Timing Constraints
The design uses an SDC file to define timing constraints. Important timing parameters include: Clock period Clock port Input delays Output delays Timing uncertainty For example:
```text
create_clock \
    -name clk \
    -period 5.0 \
    [get_ports clk]
```
A correct timing constraint setup is necessary for timing-driven synthesis, placement, and routing.

# 25. OpenROAD / Layout View
The physical layout can be inspected using OpenROAD-based tools. The screenshots demonstrate a layout containing: Standard-cell rows Power structures Cell instances Core boundaries I/O regions Metal layers The layout view allows the physical implementation to be visually inspected before final signoff.

# 26.Bind netlist with physical library cells
Logical cells such as FF1, FF2, etc. are mapped to their corresponding physical standard cells from the technology library.

<img width="700" alt="Bind Netlist" src="https://github.com/user-attachments/assets/665e6f49-779e-495d-abf5-778c9ed7e57a" />

# 27.Key Learnings
Through this module, the following concepts were studied: Understanding a netlist Converting logical cells into physical dimensions Cell area calculation Core and die dimensions Utilization factor Aspect ratio Pre-placed cells IP blocks Floorplanning Placement of pre-placed cells Switching current Peak current IR/voltage drop due to resistance and inductance Noise margin Noise-induced voltage bumps Decoupling capacitors Decoupling capacitor placement Power integrity Driver and load connectivity Multi-bit bus considerations

# 28.Conclusion
Chip floorplanning is a critical stage of ASIC physical design. The dimensions of the core and die must be selected based on cell area, utilization factor, and aspect ratio. Pre-placed cells and IP blocks must be positioned carefully to achieve good connectivity and efficient routing. During switching, large instantaneous current demand can cause voltage fluctuations because of resistance and inductance in the power network. Noise margin determines the ability of the circuit to tolerate such disturbances. Decoupling capacitors provide local charge during high-current switching events and help maintain supply stability. Thus, proper floorplanning, pre-placed-cell placement, power planning, noise-margin analysis, and decoupling-capacitor placement are essential for reliable physical design.


