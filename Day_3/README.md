# Day 3: Combinational and Sequential Optimization

Welcome to Day 3 of this workshop! Today we discuss optimization of combinational and sequential circuits, introducing techniques to enhance efficiency and performance.

---

## Table of Contents

- [Combinational Logic Optimization](#Combinational-Logic-Optimization)
- [Sequential Logic Optimization](#Sequential-Logic-Optimization)
- [Combinational Logic Optimization labs](#Combinational-Logic-Optimization-labs)
- [Sequential Logic Optimization labs](#Sequential-Logic-Optimization-labs)
---

## Combinational Logic Optimization

### Objective:
- Squeeze and simplify logic to get the most optimized design in terms of area and power.

### Techniques:

- #### Constant Propagation:
   - Replaces logic blocks with constants when input values are fixed.
   - - **Example:**
    ```
    Y = ((A·B) + C)' 
    If A = 0 → Y = (0 + C)' = C'
    ```
  - **Result:**
   - Complex gate logic (6 MOS transistors) simplifies to a single inverter (2 MOS transistors).
- #### Boolean Logic Optimization:
   - Simplify Boolean expressions using: Karnaugh Map (K-Map) and Quine McCluskey Method.
   - **Example Verilog:**
    ```verilog
    assign y = a ? (b ? c : (c ? a : 0)) : (!c);
    ```
    
    - Optimized expression:  
      ```
      y = a ⊕ c
      ```
      
### Yosys Optimization Commands:
  
  - The command to perform logic optimization in Yosys is opt_clean.
    ```bash
       opt_clean
    ```
  - Additionally, for a hierarchical design involving multiple sub-modules, the design must be flattened by running the flatten command before executing the opt_clean command.
     ```bash
        flatten
     ```
     then,
    ```bash
        synth -top <module_name>
        opt_clean -purge
    ```

---

## Constant Propagation  
### 1. What is it?  
- Replace signals driven by **constant values (0 or 1)** with fixed logic.  
- Eliminates unnecessary gates and wires.  

### 2. Detailed Info  
Constant propagation checks if a net or signal always evaluates to a fixed value and simplifies the circuit accordingly. For example, if a gate input is always `0`, the output can be optimized to `0` without the actual logic.  

### Advantages  
- Reduces **gate count**  
- Simplifies **logic equations**  
- Improves **area and power efficiency**  

---

## State Optimization  
### 1. What is it?  
- Reduces the number of **states in a finite state machine (FSM)** by merging equivalent or unreachable states.  

### 2. Detailed Info  
In FSMs, some states may be redundant (behaving the same as others) or unreachable. State optimization identifies and removes them, leading to a minimal FSM.  

###  Advantages  
- Reduces **flip-flop count**  
- Improves **timing** (fewer state transitions)  
- Lower **area and power** usage  

---

## Cloning  
### 1. What is it?  
- Duplicate logic or registers to **reduce fanout** and **improve timing**.  

### 2. Detailed Info  
If a single gate or flip-flop drives many loads, the delay increases. Cloning creates a copy of that logic/register so loads are split between them.  

###  Advantages  
- Reduces **net delay** from high fanout  
- Improves **timing closure**  
- Helps achieve **higher operating frequency**

## Retiming  
### 1. What is it?  
- **Reposition registers (flip-flops)** in a circuit without changing functionality.  

### 2. Detailed Info  
Retiming balances combinational delays by moving flip-flops across logic gates. It helps meet setup/hold timing while preserving overall input-output behavior.  

###  Advantages  
- Optimizes **critical path delay**  
- Enables **higher clock frequency**  
- Balances **pipeline stages** for better performance

---
## Sequential Logic Optimization

#### Basic:
- **Sequential Constant Propagation**  
  - Propagates known constant values through flip-flops during synthesis.

### Basic Techniques:

#### 1.Sequential Constant Propagation:

- Propagates known constant values through flip-flops during synthesis.

 **Advanced Techniques:**
 
 #### 1. State Optimization

- Reduces the number of FSM states → optimizing area and transitions.

 #### 2. Retiming

Moves registers across logic boundaries to balance delay and improve timing.

 #### 3. Sequential Logic Cloning

- Duplicates logic in floorplan-aware synthesis to meet timing and congestion goals.

### Outcome:
- These optimization techniques ensure faster, smaller, and power-efficient circuits, ready for real-world chip design.

---
### **To get start through the optimization of design logic**

**Go through the step by step worflow**

#### Locate the file in your project directory. 

```bash
cd ~/vsd/VLSI/sky130RTLDesignAndSynthesisWorkshop/verilog_files
```
#### List the optimization designs
 ```bash
 ls *opt*
```

#### list the sequential optimization designs
 ```bash
 ls *dff*
```
---

# Combinational Logic Optimization labs

### Lab 1 :

To see the verilog logic of Lab1.
```bash
gedit opt_check.v
```

<img width="923" height="697" alt="Screenshot from 2025-09-26 13-40-57" src="https://github.com/user-attachments/assets/f7411013-2856-4703-a3aa-6ad796e3c587" />

#### Explanation:

- assign y = a ? b : 0; means:
- If a is true, y is assigned the value of b.
- If a is false, y is 0.

#### Lanch yosys:
```bash
yosys
```

#### 6. **Run this command inside yosys:**
```bash
read_liberty -lib ~/vsd/VLSI/sky130RTLDesignAndSynthesisWorkshop/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog opt_check.v
synth -top opt_check
opt_clean -purge
abc -liberty ~/vsd/VLSI/sky130RTLDesignAndSynthesisWorkshop/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
write_verilog opt_check.v
```

<img width="895" height="638" alt="Screenshot from 2025-09-26 14-22-21" src="https://github.com/user-attachments/assets/5812f893-d2c8-4442-919e-c6974453210c" />

#### Netlist Dot File

<img width="866" height="584" alt="Screenshot from 2025-09-26 14-17-41" src="https://github.com/user-attachments/assets/2d50aea5-b42b-4872-bbb2-f670de1e7e0d" />

### Statistics
```bash
=== opt_check ===

   Number of wires:                  3
   Number of wire bits:              3
   Number of public wires:           3
   Number of public wire bits:       3
   Number of memories:               0
   Number of memory bits:            0
   Number of processes:              0
   Number of cells:                  1
     $_AND_                          1
```
then, exit from yosys
```bash
exit
```

---
### Lab 2:
To see the verilog logic of Lab 2.
```bash
gedit opt_check2.v
```

<img width="931" height="394" alt="Screenshot from 2025-09-26 14-30-08" src="https://github.com/user-attachments/assets/20cefffb-aff4-4039-a930-f4ece063d85f" />

#### Code analysis:
1. if a is true then the output is 1.
2. else 0.

#### launch yosys:
```bash
yosys
```
Inside yosys:
```bash
read_liberty -lib ~/vsd/VLSI/sky130RTLDesignAndSynthesisWorkshop/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog opt_check2.v
synth -top opt_check2
opt_clean -purge
abc -liberty ~/vsd/VLSI/sky130RTLDesignAndSynthesisWorkshop/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
write_verilog opt_check2.v
```

<img width="1920" height="922" alt="Screenshot from 2025-09-26 14-41-13" src="https://github.com/user-attachments/assets/b5b0f210-106c-44de-b406-354e4417488c" />


#### Netlist Dot File 
<img width="791" height="533" alt="Screenshot from 2025-09-26 14-40-56" src="https://github.com/user-attachments/assets/cf8de016-7682-457a-9a4c-3505d0895a80" />

#### Statistics
```bash
=== opt_check2 ===

   Number of wires:                  3
   Number of wire bits:              3
   Number of public wires:           3
   Number of public wire bits:       3
   Number of memories:               0
   Number of memory bits:            0
   Number of processes:              0
   Number of cells:                  1
     $_OR_                           1
```
then, exit from yosys
```bash
exit
```

---
### Lab 3
To see the verilog logic of Lab3.
```bash
gedit opt_check3.v
```

<img width="918" height="505" alt="Screenshot from 2025-09-26 15-07-07" src="https://github.com/user-attachments/assets/66f66c49-3d2e-41d6-a7c7-e994a0b5e7a3" />

#### Code explanation
- if c is true, then the output is b, else 0.
- if c is b then the value of y be b or 0 depends on the value of a.

Enter inside the yosys by the command:
```bash
yosys
```
Inside yosys
```bash
read_liberty -lib ~/vsd/VLSI/sky130RTLDesignAndSynthesisWorkshop/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog opt_check3.v
synth -top opt_check3
opt_clean -purge
abc -liberty ~/vsd/VLSI/sky130RTLDesignAndSynthesisWorkshop/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
write_verilog opt_check3.v
```
<img width="1920" height="922" alt="Screenshot from 2025-09-26 15-47-49" src="https://github.com/user-attachments/assets/1ff6146b-fa38-4720-a589-1dffd73f6ccd" />

### Netlist Dot File
<img width="823" height="478" alt="Screenshot from 2025-09-26 15-42-29" src="https://github.com/user-attachments/assets/a0a25c1a-987a-4352-ac4e-cd4694f45b40" />

#### Statistics:
```bash
=== opt_check3 ===

   Number of wires:                  5
   Number of wire bits:              5
   Number of public wires:           4
   Number of public wire bits:       4
   Number of memories:               0
   Number of memory bits:            0
   Number of processes:              0
   Number of cells:                  2
     $_ANDNOT_                       1
     $_NAND_                         1
```
---

# Sequential Logic Optimization labs

### Lab 4
To see the logic of verilog code for lab 4.
```bash
gedit dff_const1.v
```

<img width="927" height="586" alt="Screenshot from 2025-09-26 15-56-33" src="https://github.com/user-attachments/assets/12ad6b57-b89f-444e-bd47-4f739feff7af" />

#### Code Analysis
- At positive edge of the clock:
  - if reset is high, then the value of q is 0.
  - if reset is low, then the value of q is 1.

To go inside the yosys, run
```bash
yosys
```
then, Inside yosys
```bash
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog dff_const1.v
synth -top dff_const1
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
write_verilog dff_constant1.v
```
### Netlist Dot File

<img width="1033" height="581" alt="Screenshot from 2025-09-26 19-24-52" src="https://github.com/user-attachments/assets/7ff4cfbf-2b98-4b02-8f50-5e2f874c321f" />

---
## Lab 5
To see the logiv verilog file, run this
```bash
gvim dff_const2.v
```
<img width="1159" height="534" alt="Screenshot from 2025-09-26 19-29-52" src="https://github.com/user-attachments/assets/600422ea-d0cf-4a57-bd6f-cb40ec1fa1ac" />

### Code logic
- In all positive edge of reset and clock, the output q will high always.
- q=1
Enter into the yosys
```bash
yosys
```
Inside the yosys, run
```bash
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog dff_const2.v
synth -top dff_const2
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
write_verilog dff_const2.v
```

#### Netlist Dot File
<img width="832" height="507" alt="Screenshot from 2025-09-26 19-46-41" src="https://github.com/user-attachments/assets/a9745b8a-856e-4a7f-9c78-e84aace0ba83" />

---
### Lab 6
To see the logic of the verilog file
```bash
gvim dff_const3.v
```
<img width="1318" height="735" alt="Screenshot from 2025-09-26 19-59-23" src="https://github.com/user-attachments/assets/db0b8f77-500f-44d4-ae7c-1a19dba2b973" />

Entering into the yosys
```bash
yosys
```

Inside yosys
```bash
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog dff_const3.v
synth -top dff_const3
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
write_verilog dff_const3.v
```

<img width="1210" height="670" alt="Screenshot from 2025-09-26 20-03-05" src="https://github.com/user-attachments/assets/ab04ebe6-aa5f-4f0d-a240-027057b85b7a" />

---
## Summary
### Combinational Optimization Techniques:
  - Constant Propagation: Replaces logic with constants → fewer gates, less power.
  - Boolean Logic Optimization: Uses K-Map / Quine-McCluskey to simplify expressions.
   
### Sequential Optimization Techniques:
    - State Optimization: Minimizes FSM states → reduces flip-flops and transitions.
    - Retiming: Moves registers to balance delay → improves timing.
    - Cloning: Duplicates logic/registers to reduce fanout → helps timing closure.

### Yosys Commands Used:
   - flatten → Converts hierarchical design into flat design.
   - opt_clean -purge → Removes unused nets and redundant logic.
   - dfflibmap / abc → Map logic to technology libraries for synthesis.

### Lab Outcomes:
   - Optimized designs show reduced transistor count, logic levels, and area.
   - Some flip-flops collapsed into constants → saving resources.
   -  Visualized dot files confirm simplifications in synthesized netlists.

By applying these optimizations, designs become smaller, faster, and more power-efficient, ready for real-world VLSI implementation.
