 ># Smart Street Light Controller Using Verilog HDL

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



## 7. RTL Design

**Design name:** `smart_street_light`

**Expected source location:** `rtl/smart_street_light.v`

The RTL describes the digital control behavior of the street light system. The module's ports, registers, combinational logic, and sequential logic should be documented directly from the source.

```verilog
module smart_street_light #(
    parameter DELAY_CYCLES = 4
)(
    input  wire clk,
    input  wire reset,
    input  wire sensor_night,
    input  wire sensor_valid,

    output reg  street_light,
    output reg  fault
);

    // FSM states
    localparam LIGHT_OFF   = 3'b000;
    localparam WAIT_NIGHT  = 3'b001;
    localparam LIGHT_ON    = 3'b010;
    localparam WAIT_DAY    = 3'b011;
    localparam FAULT_STATE = 3'b100;

    localparam COUNT_WIDTH =
        (DELAY_CYCLES <= 1) ? 1 : $clog2(DELAY_CYCLES);

    reg [2:0] state;
    reg [COUNT_WIDTH-1:0] timer_count;

    // Sequential FSM and timer
    always @(posedge clk or posedge reset) begin
        if (reset) begin
            state        <= LIGHT_OFF;
            timer_count  <= 0;
            street_light <= 1'b0;
            fault        <= 1'b0;
        end
        else begin
            case (state)

                LIGHT_OFF: begin
                    street_light <= 1'b0;
                    timer_count  <= 0;
                    fault        <= 1'b0;

                    if (!sensor_valid) begin
                        state <= FAULT_STATE;
                        fault <= 1'b1;
                    end
                    else if (sensor_night) begin
                        state <= WAIT_NIGHT;
                    end
                end

                WAIT_NIGHT: begin
                    street_light <= 1'b0;
                    fault <= 1'b0;

                    if (!sensor_valid) begin
                        state <= FAULT_STATE;
                        timer_count <= 0;
                        fault <= 1'b1;
                    end
                    else if (!sensor_night) begin
                        state <= LIGHT_OFF;
                        timer_count <= 0;
                    end
                    else if (timer_count >= DELAY_CYCLES - 1) begin
                        state <= LIGHT_ON;
                        street_light <= 1'b1;
                        timer_count <= 0;
                    end
                    else begin
                        timer_count <= timer_count + 1'b1;
                    end
                end

                LIGHT_ON: begin
                    street_light <= 1'b1;
                    timer_count <= 0;
                    fault <= 1'b0;

                    if (!sensor_valid) begin
                        state <= FAULT_STATE;
                        street_light <= 1'b0;
                        fault <= 1'b1;
                    end
                    else if (!sensor_night) begin
                        state <= WAIT_DAY;
                    end
                end

                WAIT_DAY: begin
                    street_light <= 1'b1;
                    fault <= 1'b0;

                    if (!sensor_valid) begin
                        state <= FAULT_STATE;
                        street_light <= 1'b0;
                        timer_count <= 0;
                        fault <= 1'b1;
                    end
                    else if (sensor_night) begin
                        state <= LIGHT_ON;
                        timer_count <= 0;
                    end
                    else if (timer_count >= DELAY_CYCLES - 1) begin
                        state <= LIGHT_OFF;
                        street_light <= 1'b0;
                        timer_count <= 0;
                    end
                    else begin
                        timer_count <= timer_count + 1'b1;
                    end
                end

                FAULT_STATE: begin
                    street_light <= 1'b0;
                    timer_count <= 0;
                    fault <= 1'b1;

                    if (sensor_valid) begin
                        state <= LIGHT_OFF;
                        fault <= 1'b0;
                    end
                end

                default: begin
                    state <= FAULT_STATE;
                    street_light <= 1'b0;
                    timer_count <= 0;
                    fault <= 1'b1;
                end

            endcase
        end
    end

endmodule
```

## 8. Testbench

**Expected source location:** `testbench/tb_smart_street_light.v`

The testbench should provide the clock and input sequences required to exercise the controller and observe its outputs.
```verilog
`timescale 1ns/1ps

module tb_smart_street_light;

    reg clk;
    reg reset;
    reg sensor_night;
    reg sensor_valid;

    wire street_light;
    wire fault;

    integer pass_count = 0;
    integer fail_count = 0;

    // Instantiate the Smart Street Light Controller
    smart_street_light #(
        .DELAY_CYCLES(4)
    ) DUT (
        .clk(clk),
        .reset(reset),
        .sensor_night(sensor_night),
        .sensor_valid(sensor_valid),
        .street_light(street_light),
        .fault(fault)
    );

    // Generate 10 ns clock
    always #5 clk = ~clk;

    // Wait for the specified number of clock cycles
    task tick;
        input integer cycles;
        integer i;
        begin
            for (i = 0; i < cycles; i = i + 1) begin
                @(posedge clk);
                #1;
            end
        end
    endtask

    // Check expected outputs
    task check_output;
        input expected_light;
        input expected_fault;
        input [200*8-1:0] test_name;
        begin
            if ((street_light === expected_light) &&
                (fault === expected_fault)) begin
                $display("PASS: %0s", test_name);
                pass_count = pass_count + 1;
            end
            else begin
                $display(
                    "FAIL: %0s | Light=%b expected=%b | Fault=%b expected=%b",
                    test_name, street_light, expected_light,
                    fault, expected_fault
                );
                fail_count = fail_count + 1;
            end
        end
    endtask

    initial begin
        $dumpfile("smart_street_light.vcd");
        $dumpvars(0, tb_smart_street_light);

        clk = 0;
        reset = 1;
        sensor_night = 0;
        sensor_valid = 1;

        $display("====================================");
        $display(" SMART STREET LIGHT TEST STARTED");
        $display("====================================");

        // Test 1: Reset
        #12;
        reset = 0;
        check_output(0, 0, "Reset: light OFF");

        // Test 2: Night detection and delay
        @(negedge clk);
        sensor_night = 1;
        tick(4);
        check_output(0, 0, "Night delay: light remains OFF");

        // Test 3: Night delay expires
        tick(1);
        check_output(1, 0, "Night delay expired: light ON");

        // Test 4: Day detection and delay
        @(negedge clk);
        sensor_night = 0;
        tick(4);
        check_output(1, 0, "Day delay: light remains ON");

        // Test 5: Day delay expires
        tick(1);
        check_output(0, 0, "Day delay expired: light OFF");

        // Test 6: Cancel pending night transition
        @(negedge clk);
        sensor_night = 1;
        tick(2);
        @(negedge clk);
        sensor_night = 0;
        tick(1);
        check_output(0, 0, "Night request cancelled");

        // Test 7: Sensor fault
        @(negedge clk);
        sensor_valid = 0;
        tick(1);
        check_output(0, 1, "Sensor fault: light OFF");

        // Test 8: Recover from sensor fault
        @(negedge clk);
        sensor_valid = 1;
        tick(1);
        check_output(0, 0, "Fault recovery");

        // Test 9: Turn light ON before reset
        @(negedge clk);
        sensor_night = 1;
        tick(5);
        check_output(1, 0, "Light ON before reset");

        // Test 10: Asynchronous reset
        reset = 1;
        #1;
        check_output(0, 0, "Asynchronous reset");

        // Test 11: Normal operation after reset
        reset = 0;
        @(negedge clk);
        sensor_night = 0;
        tick(1);
        check_output(0, 0, "Normal operation after reset");

        $display("====================================");
        $display("Tests passed: %0d", pass_count);
        $display("Tests failed: %0d", fail_count);

        if (fail_count == 0)
            $display("RESULT: ALL TESTS PASSED");
        else
            $display("RESULT: TEST FAILURES FOUND");

        $display("====================================");
        $finish;
    end

endmodule
```
## 9. Simulation

 Icarus Verilog was used, the simulation can be run from the directory containing the RTL and testbench:

```bash
iverilog -o sim.out rtl/smart_street_light.v testbench/tb_smart_street_light.v
vvp sim.out
```

The command assumes the filenames and module structure match those shown above. Adjust the paths if your actual files differ.

## 10. Waveform

A waveform viewer can help inspect the timing relationship between input changes and street light control outputs.


```bash
gtkwave dump.vcd
```
<img width="950" alt="gtkwaveform" src="https://github.com/user-attachments/assets/55dfb2a3-7f1c-4d51-9060-3c53328008f1" />



## 11. Synthesis

The RTL-to-GDSII flow includes logic synthesis, which maps the RTL into a gate-level representation using the selected standard-cell library.

The synthesis report can be used to examine the mapped cell count, area estimates, and synthesis warnings.

<img width="850" alt="yosys res" src="https://github.com/user-attachments/assets/f994250e-c75b-4f6d-b3c0-2209707de9b2" />


<img width="850" alt="yosys show" src="https://github.com/user-attachments/assets/1a5a4b87-420e-4a30-834e-4c97f3864d82" />



## 12. Pre-Layout Static Timing Analysis

Pre-layout static timing analysis evaluates timing paths before final physical routing, based on the constraints and implementation stage.

Relevant evidence may include:

- Clock period and timing constraints.
- Setup slack and hold slack.
- Critical timing paths.
- Timing warnings and unconstrained paths.

<img width="850" alt="prelayout sta" src="https://github.com/user-attachments/assets/eb68d1ad-79ee-4776-93ec-94f0baa7c40f" />


## 13. Physical Design (OpenLane)

The recorded OpenLane run for the design is:

`10-10_09-03`

The flow was executed using the SKY130A PDK and the `sky130_fd_sc_hd` standard-cell library.

### 13.1 Floorplanning

Floorplanning establishes the core and die regions and prepares the design for physical implementation.

<img width="950" alt="floorplan" src="https://github.com/user-attachments/assets/9bd58e05-d53c-4fc4-99e9-ace1bcabe21b" />


### 13.2 Placement

Placement assigns physical locations to the standard cells within the floorplan.

<img width="950" alt="S" src="https://github.com/user-attachments/assets/3934137f-baf4-49a4-bb7a-8f5563604c9d" />


### 13.3 Clock Tree Synthesis

Clock-tree synthesis is applicable when the configured flow includes a clock tree. Include evidence from the actual run if this stage was executed.

<img width="950" alt="CTS" src="https://github.com/user-attachments/assets/62af626d-96b2-4d5d-bac8-be228b5c9c85" />


### 13.4 Routing

Routing creates the physical interconnections between the placed cells.

<img width="950" alt="Routing" src="https://github.com/user-attachments/assets/207ee3a3-6c82-4e44-a1db-f33860628c3d" />


## 14. Post-Route Static Timing Analysis

Post-route timing analysis evaluates timing after physical interconnections have been implemented.

Record the available setup and hold slack, critical paths, timing corners, and any remaining violations from the final run's reports.

<img width="950" alt="STA_tiiming_report" src="https://github.com/user-attachments/assets/6e11f154-a77b-42fd-9095-4acbdb146dc1" />


<img width="950" alt="sta report" src="https://github.com/user-attachments/assets/5e5a26b5-b181-48c2-b9a2-37d3033dd5ab" />



## 15. Final Layout (GDS)

The OpenLane run generated a GDSII layout artifact for the Smart Street Light Controller.

The previously observed file was approximately **306 KB**.

The original artifact was reported at:

```text
designs/smart_street_light/runs/10-10_09-03/
results/magic/smart_street_light.gds
```


The GDSII file can be inspected with a compatible layout viewer such as Magic or KLayout when the necessary technology files are configured.

<img width="950" alt="GDS layout" src="https://github.com/user-attachments/assets/42a04369-72ba-4c5f-a2dc-63feda04977b" />


## 16.Summary


### Physical Design Verification

| Check | Result |
|---|---|
| OpenLane Flow | Completed |
| GDSII Layout | Generated (~306 KB) |
| Magic DRC | 0 violations reported |
| TritonRoute Pin Violations | 0 |
| TritonRoute Net Violations | 0 |
| LVS | PASS — Circuits match uniquely |
| Pre-layout WNS | 0.00 ns |
| Pre-layout TNS | 0.00 ns |
| Antenna Verification | 0 violations |

## 17. Implementation Results

The previously observed implementation produced a GDSII layout artifact of approximately 306 KB.

The final summary report was also inspected during the earlier project work. For a reproducible submission, include the actual report file and screenshots of the relevant final results.

<img width="950" alt="final_flow_summary" src="https://github.com/user-attachments/assets/67b4887b-6853-4ea8-805e-338fb0432143" />


## 18. Additional Verification

Additional verification may include:

- Inspection of the final GDS layout.
- Review of DRC, LVS, and antenna reports.
- Review of post-route timing paths.
- Additional simulation of switching and fault scenarios.

<img width="950" alt="c" src="https://github.com/user-attachments/assets/9959f877-93ae-4272-81c6-72a8af718cde" />


<img width="950" alt="re" src="https://github.com/user-attachments/assets/3699cb11-ba6c-4b1c-99ff-46d2ccacc033" />


<img width="950" alt="ree" src="https://github.com/user-attachments/assets/81836396-f209-4057-9f81-b7c661248a09" />


<img width="950" alt="n" src="https://github.com/user-attachments/assets/988560f2-f022-4986-a6b7-1bd2da24ad55" />




## 19. Feasibility

This project demonstrates the feasibility of describing digital street light control logic in Verilog and processing it through an automated ASIC implementation flow.

The implementation provides experience with RTL design, synthesis, placement, routing, timing analysis, and GDSII generation.

Practical deployment would require additional validation of the control specification, electrical interfaces, sensor integration where applicable, power requirements, and hardware behavior.

## 20. Conclusion

The Smart Street Light Controller project demonstrates the RTL-to-GDSII implementation workflow using Verilog HDL, OpenLane, and the SKY130A technology platform.

The recorded run generated a final GDSII layout artifact of approximately 306 KB. The final quality of the design should be evaluated using its actual functional simulation, timing, and physical-verification reports.

The project provides a foundation for further work on switching control, fault handling, and physical implementation of digital control systems.

#### Author

**Nithin Renjarla**  
B.Tech – Electronics and Communication Engineering (ECE)  
Anurag University, Hyderabad

**Project:** Smart Street Light Controller Using Verilog HDL

**Environment:** Ubuntu on VirtualBox, Icarus Verilog, GTKWave, Yosys, OpenSTA, OpenLane, Sky130 PDK, Magic, and KLayout.

**Repository:** [RTL_Workshop](https://github.com/nithinrenjarla/RTL_Workshop)

