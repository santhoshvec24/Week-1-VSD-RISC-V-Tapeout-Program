# Day 5: Optimization in Synthesis

Welcome to Day 5 of the RTL workshop! Today, we will cover optimization in Verilog synthesis, focusing on `if-else statements`, `for` loops, generate blocks, and explore how improper coding can lead to inferred latches. Labs are included for hands-on experience.

---

## Content

- If-Else Statements in Verilog
- Inferred Latches in Verilog
- Labs for If-Else and Case Statements
- Verilog Looping Constructs: `for` loop & `generate for` loop
- Summary

---

## 1. If-Else Statements in Verilog

- **If-else statements** are used for conditional execution in behavioral modeling, typically within procedural blocks (`always`, `initial`, tasks, or functions).
- Mostly used for priority logic.

### 2. Inferred Latches:
- Inferred Latches in `if` is due to the incomplete `if` statement.
- Due to incomplete `if` statement, the incomplete condition will get latched with either of inputs and with the output.
- Until it is intented to do the incomplete `if-else` statements.
   - For example,
         - The verilog code for the counter is also an incomplete `if-else` statement but it's logic doesn't affected by this and it is intended to have it in the code.
     ```verilog
             module counter (
                   input clk, 
                   input rst, 
                   input enable,
                   output reg [2:0] count
              );

            always @(posedge clk or posedge rst) begin
            if (rst)
             count <= 3'd0;
           else if (enable)
              count <= count + 1;
           end

           endmodule
      ```
### Syntax

```verilog
if (condition) begin
    // Code for true Condition
end else begin
    // Code for false Condition
end
```

### Nested If-Else

```verilog
if (condition1) begin
    // Code for condition1 true
end else if (condition2) begin
    // Code for condition2 true
end else begin
    // Code if all Conditions are false
end
```

---

### Case Statement 
The `if-else` and `case` statement should be in the `always` block and the variable declared in inside it should be `reg <variable>` .

#### Caveats with case statement
- Due to the incomplete `case` statement, it will become inferred latch .
- Due to this, the incomplete case statements is latched with the output.
- To avoid this, code `case` with default case statement.

```verilog
case (expression)
    value1 : begin
                 // statements for value1
             end
    value2 : begin
                 // statements for value2
             end
    ...
    default : begin
                 // statements if none of the above match
              end
endcase
```

#### Solution: Add Else or Default Case
```verilog
module ex (
    input wire a, b, sel,
    output reg y
);
    always @(a, b, sel) begin
        case(sel)
            1'b1 : y = a;
            default : y = 1'b0; // Default assignment
        endcase
    end
endmodule
```



---

## 3. Labs for If-Else and Case Statements

### Lab 1: Incomplete If Statement

The incomplete if verilog code is 

<img width="816" height="302" alt="Screenshot from 2025-09-27 18-58-18" src="https://github.com/user-attachments/assets/5142f3bb-1510-4407-aacb-52a9296b73d2" />

The gtkwave is:

<img width="993" height="706" alt="Screenshot from 2025-09-27 18-56-35" src="https://github.com/user-attachments/assets/ff4a9e76-c271-4316-9bb5-c38c8a856987" />

The Netlist Dot File:

<img width="819" height="557" alt="Screenshot from 2025-09-27 19-02-07" src="https://github.com/user-attachments/assets/dd58d450-9822-427b-83fc-f6c97fba4d54" />

---

### Lab 2: Nested If-Else
The incomplete nested if-else verilog code is

<img width="716" height="468" alt="Screenshot from 2025-09-27 19-21-45" src="https://github.com/user-attachments/assets/b60a9483-2981-4e25-9070-0978d62c20f5" />

gtkwave is:

<img width="1001" height="702" alt="Screenshot from 2025-09-27 19-08-36" src="https://github.com/user-attachments/assets/1ffd0ade-c34e-4c4f-9111-f78df1f092cf" />

Netlist Dot File is:

<img width="743" height="469" alt="Screenshot from 2025-09-27 19-23-30" src="https://github.com/user-attachments/assets/37bb3d74-2e9e-44e8-9c28-5b7b4e36a9a7" />

---


### Lab 3: Complete Case Statement
The complete case statement verilog code of comp_case is

<img width="716" height="436" alt="Screenshot from 2025-09-27 19-33-44" src="https://github.com/user-attachments/assets/67662a54-ce17-4694-b397-e4ea3367e28f" />

gtkwave is:

<img width="999" height="698" alt="Screenshot from 2025-09-27 19-28-47" src="https://github.com/user-attachments/assets/8351776e-4827-4277-8b83-2fe962cb69ce" />

Netlist Dot File is:

<img width="890" height="545" alt="Screenshot from 2025-09-27 19-35-04" src="https://github.com/user-attachments/assets/84abd83d-e010-4e06-8fb8-4a7e3a329b1f" />


### Lab 4: Incomplete Case Handling

The bad_case verilog code is

<img width="726" height="373" alt="Screenshot from 2025-09-27 19-51-46" src="https://github.com/user-attachments/assets/e3352bc0-625d-4576-8e8f-e70ad5818d4a" />

gtkwave is:

<img width="1361" height="787" alt="Screenshot from 2025-09-27 19-50-26" src="https://github.com/user-attachments/assets/b30f6611-b268-47ea-b564-73f9faf8ff0d" />

Netlist Dot File is:

<img width="736" height="627" alt="Screenshot from 2025-09-27 19-52-46" src="https://github.com/user-attachments/assets/617498c8-829f-4b40-90b5-236ec159001e" />

---
## Verilog Looping Constructs: `for` loop & `generate for` loop

### 1. `for` Loop

- Repeatedly executes a block of statements for a fixed number of times within an `always` block.
- Mainly used for iterative assignments in **combinational or sequential logic**.
- `for` loop must be inside procedural blocks.

**Syntax:**
```verilog
for (initialization; condition; increment) begin
    // Statements to execute
end
```

- **Notes:**  
  - Executes **during simulation**.  
  - Loop index must be an **integer**.  
  - Useful for operations like summing arrays, shifting, or copying signals.

---

### 2. `generate for` Loop

- Used at **compile/synthesis time** to generate multiple instances of modules or repeated RTL structures.
- Creates hardware replication.

**Syntax:**
```verilog
genvar i;  // loop variable must be declared as genvar

generate
    for (i = 0; i < N; i = i + 1) begin : block_name
        // Code to be generated
    end
endgenerate

```

- **Notes:**  
  - Executes **at synthesis/compile time**, producing multiple hardware instances.  
  - `genvar` is a variable which is required for the loop index.  
  - Commonly used for arrays of registers, multiplexers, or replicated modules.

---

## Labs on `for` loop

## `mux_generate.v`

**Verilog Code**

<img width="734" height="406" alt="Screenshot from 2025-09-27 21-27-29" src="https://github.com/user-attachments/assets/06c034b6-5c99-462e-b26e-73abfcb64110" />


**Simulation**

Waveform :

<img width="1427" height="773" alt="Screenshot from 2025-09-27 21-23-55" src="https://github.com/user-attachments/assets/65bdac1c-4f44-4235-990b-bc03e6681376" />



- Implements a 4-to-1 multiplexer using a `for` loop.
- Inputs `i0`–`i3` are packed into a 4-bit wire `i_int` for indexing.
- Loop iterates over indices 0 to 3 inside an `always` block.
- Assigns `y = i_int[k]` when `k` matches the select signal `sel`.
- Only the selected input is passed to the output.
- Avoids multiple nested `if-else` or `case` statements, making code concise.

**Netlist Dot File**
<img width="944" height="723" alt="Screenshot from 2025-09-27 21-29-32" src="https://github.com/user-attachments/assets/d1c68c31-3bc7-400e-a106-7a48ab640636" />

---


## `demux_generate.v`

**Verilog Code**

Verilog Code:

<img width="713" height="471" alt="Screenshot from 2025-09-27 21-31-40" src="https://github.com/user-attachments/assets/d2108660-a88d-4246-8d66-886106777b7f" />

**Simulation**

Waveform :

<img width="1432" height="655" alt="Screenshot from 2025-09-27 21-37-44" src="https://github.com/user-attachments/assets/c4fbc93f-0bc7-4cc3-a6bb-622b8c1f1e8f" />

- Implements an 8-output demultiplexer using a `for` loop.  
- Outputs `o0`–`o7` are packed into an 8-bit register `y_int` for easy indexing.  
- `y_int` is initialized to 0, then only the bit matching `sel` is set to input `i`.  
- Routes the single input `i` to the selected output while others remain 0.  
- Loop executes sequentially during simulation; no latches are inferred.  
- Provides a concise alternative to multiple if-else or case statements.

**Netlist Dot File**

<img width="840" height="844" alt="Screenshot from 2025-09-27 21-40-56" src="https://github.com/user-attachments/assets/7b2bfe02-0152-4206-8401-c63c805e49d3" />

---

## Labs on `generate for` loop

## 8-bit Ripple Carry Adder (RCA)

- An RCA is built by cascading multiple **full adders**.  
- Each full adder adds two input bits along with a carry-in, producing a sum and a carry-out.  
- In an 8-bit RCA, the carry-out of each stage is passed as the carry-in to the next stage.  
- The first adder takes an external carry-in (usually 0), and the final adder produces the overall carry-out.
- The `generate for` loop instantiates one full adder per bit, automatically chaining the carry-out of stage *k* to the carry-in of stage *k+1*.  
- This makes the code **shorter, scalable, and less error-prone**, especially for larger adders (e.g., 16-bit, 32-bit).  
- Only the loop index changes for each stage, while the wiring of sum and carry is handled systematically.  
- If you change the bit-width (say from 8 to 16), only the loop limit needs updating.

**Verilog Code**

RCA (`rca.v`): 8 bit Ripple Carry Adder

```verilog
module rca (input [7:0] num1 , input [7:0] num2 , output [8:0] sum);
wire [7:0] int_sum;
wire [7:0]int_co;

genvar i;
generate
        for (i = 1 ; i < 8; i=i+1) begin
                fa u_fa_1 (.a(num1[i]),.b(num2[i]),.c(int_co[i-1]),.co(int_co[i]),.sum(int_sum[i]));
        end

endgenerate
fa u_fa_0 (.a(num1[0]),.b(num2[0]),.c(1'b0),.co(int_co[0]),.sum(int_sum[0]));


assign sum[7:0] = int_sum;
assign sum[8] = int_co[7];
endmodule
```

FA (`fa.v`): _1 bit Full Adder_

```verilog 
module fa (input a , input b , input c, output co , output sum);
        assign {co,sum}  = a + b + c ;
endmodule
```

**RTL Simulation**

Waveform :

<img width="1524" height="696" alt="Screenshot from 2025-09-27 22-22-38" src="https://github.com/user-attachments/assets/11ea33bd-e62a-44f5-8695-75a7a9539fb4" />

**Synthesis**
Netlist Dot File :

<img width="961" height="789" alt="Screenshot from 2025-09-27 22-58-58" src="https://github.com/user-attachments/assets/81a8b371-ed7e-4c55-a799-dd3a888f189c" />

**Gate-Level Simulation**

Thus, the *RTL Simulation*, *Synthesis*, and *Gate-level simulation* of the **8-bit Ripple Carry Adder** have been successfully completed and verified.

Waveform:

<img width="1425" height="718" alt="Screenshot from 2025-09-27 23-06-05" src="https://github.com/user-attachments/assets/642f8b0f-46cb-458d-9230-1e06daf0723a" />

---

## Summary

- **If-Else & Case:** Control logic in Verilog; `if-else` handles conditional decisions, while `case` is better for multi-way selection. Missing branches in either can cause **inferred latches**, which hold old values unintentionally.  
- **Inferred Latches:** Occur when outputs are not assigned in all paths (incomplete `if`, `case`, or partial assignments). Always use `else` or `default` to avoid them.  
- **for Loops:** Used inside `always` blocks for simulation-time iteration, simplifying repeated assignments.  
- **Generate for Loops:** Create multiple hardware instances at synthesis time; useful for scalable designs like adders or multiplexers.  
- **Lab Highlights:**  
  - Incomplete `if`/`case` → inferred latches.  
  - `mux_generate` and `demux_generate` show how `for` loops reduce repetitive code.  
  - 8-bit RCA with `generate for` demonstrates scalable hardware replication.  

Overall, careful use of conditional constructs and loops helps write clean, latch-free, and scalable Verilog code.

---
