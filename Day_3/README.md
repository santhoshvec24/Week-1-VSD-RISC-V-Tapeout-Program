# Day 3: Combinational and Sequential Optimization

Welcome to Day 3 of this workshop! Today we discuss optimization of combinational and sequential circuits, introducing techniques to enhance efficiency and performance.

---

## Table of Contents

- [Combinational Logic Optimization](#Combinational-Logic-Optimization)
- [Sequential Logic Optimization](#Sequential-Logic-Optimization)
- [Combinational Logic Optimization-labs](#Combinational-Logic-Optimization-examples)
- [Sequential-Logic-Optimization-labs](#Sequential-Logic-Optimization-examples)
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


