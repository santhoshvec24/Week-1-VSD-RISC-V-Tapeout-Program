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
iverilog -o ~/vcd/a.out ternary_operator_mux.v tb_ternary_operator_mux.v
```
then,
```bash
cd ~/vcd
./a.out
gtkwave tb_ternary_operator_mux.vcd
```
<img width="995" height="704" alt="Screenshot from 2025-09-27 05-35-15" src="https://github.com/user-attachments/assets/6c7a718d-d675-453a-9f01-7ec33e02cab5" />

To view the verilog file 
```bash
gedit ternary_operator_mux.v
```
<img width="817" height="790" alt="Screenshot from 2025-09-27 07-26-59" src="https://github.com/user-attachments/assets/38499bc7-ccf8-467f-b80c-e9b4afdb4fa5" />

#### Enter into the yosys by running it in the verilog_files directory:
```bash
cd ~/vsd/VLSI/sky130RTLDesignAndSynthesisWorkshop/verilog_files
yosys
```
Inside yosys, run
```bash
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog ternary_operator_mux.v
synth -top  ternary_operator_mux
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```
<img width="788" height="581" alt="Screenshot from 2025-09-27 05-59-34" src="https://github.com/user-attachments/assets/44f99ec0-a71e-4e51-88f6-51f4cabf3434" />

---
## GLS of Ternary Operator MUX
Gate-Level Simulation is performed using the synthesized netlist (ternary_operator_mux_net.v). This helps verify the functional correctness of the design after synthesis, using the actual standard cells and any delays (if modeled)

```bash
iverilog -o ~/vsd/a.out ../my_lib/verilog_model/primitives.v  ../my_lib/verilog_model/sky130_fd_sc_hd.v ternary_operator_mux_net.v tb_ternary_operator_mux.v
cd ~vsd
./a.out
gtkwave tb_ternary_operator_mux.vcd
```
<img width="994" height="706" alt="Screenshot from 2025-09-27 10-04-53" src="https://github.com/user-attachments/assets/7e567477-14d7-412b-8dc5-4cc3d1788fc1" />

---

## Lab 2

```bash
iverilog -o ~/vcd/a.out bad_mux.v tb_bad_mux.v
```
then,
```bash
cd ~/vcd
./a.out
gtkwave tb_bad_mux.vcd
```
<img width="994" height="706" alt="Screenshot from 2025-09-27 10-24-36" src="https://github.com/user-attachments/assets/4b3b8975-d08a-4f8b-a806-f003cbb56535" />

To view the verilog file 
```bash
gedit bad_mux.v
```
<img width="822" height="509" alt="Screenshot from 2025-09-27 10-27-14" src="https://github.com/user-attachments/assets/11d40bff-c868-46a8-b891-2b8900a99969" />

#### Enter into the yosys by running it in the verilog_files directory:
```bash
cd ~/vsd/VLSI/sky130RTLDesignAndSynthesisWorkshop/verilog_files
yosys
```
Inside yosys, run
```bash
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog bad_mux.v
synth -top  bad_mux
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```
<img width="749" height="460" alt="Screenshot from 2025-09-27 10-28-47" src="https://github.com/user-attachments/assets/22e030e6-7ef7-4a45-baed-8be33dcfe6fd" />

---
## GLS of Ternary Operator MUX
Gate-Level Simulation is performed using the synthesized netlist (bad_mux_net.v). This helps verify the functional correctness of the design after synthesis, using the actual standard cells and any delays (if modeled)

```bash
iverilog -o ~/vsd/a.out ../my_lib/verilog_model/primitives.v  ../my_lib/verilog_model/sky130_fd_sc_hd.v bad_mux_net.v tb_bad_mux.v
cd ~/vsd
./a.out
gtkwave tb_bad_mux.vcd
```
<img width="998" height="692" alt="Screenshot from 2025-09-27 10-33-31" src="https://github.com/user-attachments/assets/e6dde509-f6b7-4ba4-8b66-714afa3c2870" />

---

### Lab 3

```bash
iverilog -o ~/vcd/a.out blocking_caveat.v tb_blocking_caveat.v
```
then,
```bash
cd ~/vcd
./a.out
gtkwave tb_blocking_caveat.vcd
```
<img width="1713" height="670" alt="Screenshot from 2025-09-27 10-49-56" src="https://github.com/user-attachments/assets/a105d57c-4a8b-4456-9719-b28ea206cb00" />


To view the verilog file 
```bash
gedit blocking_caveat.v
```
<img width="826" height="495" alt="Screenshot from 2025-09-27 10-53-13" src="https://github.com/user-attachments/assets/bea46222-d0e2-4933-be56-5b9cc6c2044f" />

#### Enter into the yosys by running it in the verilog_files directory:
```bash
cd ~/vsd/VLSI/sky130RTLDesignAndSynthesisWorkshop/verilog_files
yosys
```
Inside yosys, run
```bash
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog blocking_caveat.v
synth -top  blocking_caveat
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```
<img width="766" height="538" alt="Screenshot from 2025-09-27 10-54-38" src="https://github.com/user-attachments/assets/12c6040f-a16f-476a-a1a8-dc84ffa3e500" />

---
## GLS of Blocking Caveat Mux
Gate-Level Simulation is performed using the synthesized netlist (blocking_caveat_mux_net.v). This helps verify the functional correctness of the design after synthesis, using the actual standard cells and any delays (if modeled)

```bash
iverilog -o ~/vsd/a.out ../my_lib/verilog_model/primitives.v  ../my_lib/verilog_model/sky130_fd_sc_hd.v blocking_caveat_net.v tb_blocking_caveat.v
cd ~/vsd
./a.out
gtkwave tb_blocking_caveat.vcd
```
<img width="1595" height="698" alt="Screenshot from 2025-09-27 11-03-47" src="https://github.com/user-attachments/assets/d25b484f-2bd5-4ad0-94b7-6aff777530d5" />

---

## Summary

- Gate-Level Simulation (GLS): Validates netlist functionality, timing, and testability after synthesis.
- Synthesis-Simulation Mismatch: Avoid by using synthesizable RTL code.
- Blocking vs. Non-Blocking: Use blocking (=) for combinational, non-blocking (<=) for sequential logic.
