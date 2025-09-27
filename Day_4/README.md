# Day 4: Gate-Level Simulation (GLS), Blocking vs. Non-Blocking in Verilog, and Synthesis-Simulation Mismatch

Welcome to Day 4 of the RTL Workshop!

1. **Gate-Level Simulation (GLS)**
2. **Blocking vs. Non-Blocking Assignments in Verilog**
3. **Synthesis-Simulation Mismatch**

---
## Table of Contents

[1. Gate Level Simulation (GLS)](#1-gate-level-simulation-gls)
   - GLS Workflow in iverilog.
   - Why and When to perform GLS
   - Challenges

[2. Synthesis-Simulation Mismatch](#2-synthesis-simulation-mismatch)
[3. Blocking vs. Non-Blocking Assignments in Verilog](#3-blocking-vs-non-blocking-assignments-in-verilog)
   - Blocking Statements
   - Non-Blocking Statements

[4. Labs](#4-labs)

[5. Summary](#5-summary)





## 1. Gate-Level Simulation (GLS)

**GLS** stands for **Gate-Level Simulation**.  Gate level simulation (GLS) is a technique for verifying the functionality and timing of a digital circuit after it has been synthesized from a high-level description.  The RTL code and the generated netlist are logically equivalent (well, supposed to be!) and hence the same testbenches can be used to verify both. It is a verification step in the VLSI design flow where the synthesized gate-level netlist of a digital circuit is simulated to validate:

- Functional correctness
- Timing behavior
- Power estimates
- Test structures (e.g., scan chains for DFT)

### Why Perform GLS?

- **Synthesis Validation**: Ensures that synthesis tools faithfully translate RTL into gates.
- **Timing Verification**: Simulates with realistic delays (from SDF files), allowing you to check for timing violations.
- **Testability**: Confirms that scan chains and other test features work post-synthesis.

### When is GLS Performed?

- **After synthesis**: Once the RTL is converted into a gate-level netlist.
- **Before physical design**: To catch issues early, before layout.

### Types of GLS

- **Functional GLS**: Logic-only simulation, often with zero or unit delays.
- **Timing GLS**: Uses annotated timing data to check real-world timing behavior.

### GLS using iverilog:

<img width="1905" height="960" alt="Screenshot 2025-09-27 000524" src="https://github.com/user-attachments/assets/ffc36cd4-4a42-497b-96d3-0794f0c9b741" />

### Challenges:
- GLS simulations are inherently more complex than RTL simulations due to the detailed representation of gates and their interactions. This complexity results in longer simulation times and higher memory requirements.

- Modern logic simulators only update the state of the design when an event occurs (e.g., clock edge or input change). This can make the simulations slower and may take hours to finish depending on design complexity.

- Debugging issues in GLS can be challenging due to potential initialization problems and X propagation (unknown states).

---

### Synthesis Simulation Mismatch:
- Misssing sensitivity list
   - As we know that simulator works based on the activity.
   - A **synthesis-simulation mismatch** occurs when the simulation results of RTL (pre-synthesis) do not match simulation results of the gate-level netlist (post-synthesis) or hardware. Reasons include:
        - Non-synthesizable constructs: Use of delays, initial blocks, or other code not supported by synthesis.
        - Incomplete or ambiguous coding: E.g., missing `else` clauses, improper sensitivity lists.
        - Tool interpretation differences: Simulation and synthesis tools may interpret ambiguous RTL differently.

   - Always write synthesizable, unambiguous RTL and follow good coding practices to minimize mismatches.

- Blocking Vs. Non-blocking assignments
    - Inside always block
       - `=`: Blocking assignment
          - executes the statements in the order it is written
          - So the first statement is evaluated before the second statement.
          - **Execution:** Sequential, executes immediately.
          - **Suitable for:** Combinational logic (e.g., `always @(*)`).
          - **Example:**  
           ```verilog
            always @(*)
                y = a & b;
           ```
       - `<=`: Non-blocking assignment
          - Parallel evaluation.
          - Executes all the RHS when always block is entered and assigns to LHS.
          - **Execution:** Scheduled, executes concurrently at the end of the time step.
          - **Suitable for:** Sequential logic (e.g., `always @(posedge clk)`).
          - **Example:**  
          ```verilog
            always @(posedge clk)
            q <= d;
          ```

---
## Labs

### Lab 1

```bash
iverilog -o ~/vcd/photos/ternary_operator_mux_sim.vvp  ternary_operator_mux.v tb_ternary_operator_mux.v
```
then,
```bash
cd ~/vcd/photos
vvp ternary_operator_mux_sim.vvp 
gtkwave tb_gtkwave tb_ternary_operator_mux.vcd
```
To view the verilog file 
```bash
gedit ternary_operator_mux.v
```
#### Enter into the yosys by running it in the verilog_files directory:
```bash
cd ~/vsd/VLSI/sky130RTLDesignAndSynthesisWorkshop/verilog_files
yosys
```
Inside yosys, run
```bash
read_liberty -lib ~/vsd/VLSI/sky130RTLDesignAndSynthesisWorkshop/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog ternary_operator_mux.v
synth -top  ternary_operator_mux
opt_clean -purge
abc -liberty ~/vsd/VLSI/sky130RTLDesignAndSynthesisWorkshop/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show

```
<img width="995" height="704" alt="Screenshot from 2025-09-27 05-35-15" src="https://github.com/user-attachments/assets/6c7a718d-d675-453a-9f01-7ec33e02cab5" />


