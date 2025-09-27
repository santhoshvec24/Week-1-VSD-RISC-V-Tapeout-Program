# Day 5: Optimization in Synthesis

Welcome to Day 5 of the RTL workshop! Today, we will cover optimization in Verilog synthesis, focusing on `if-else statements`, `for` loops, generate blocks, and explore how improper coding can lead to inferred latches. Labs are included for hands-on experience.

---

## Content

- [1. If-Else Statements in Verilog](#1-if-else-statements-in-verilog)
- [2. Inferred Latches in Verilog](#2-inferred-latches-in-verilog)
- [3. Labs for If-Else and Case Statements](#3-labs-for-if-else-and-case-statements)
  - [Lab 1: Incomplete If Statement](#lab-1-incomplete-if-statement)
  - [Lab 2: Synthesis Result of Lab 1](#lab-2-synthesis-result-of-lab-1)
  - [Lab 3: Nested If-Else](#lab-3-nested-if-else)
  - [Lab 4: Synthesis Result of Lab 3](#lab-4-synthesis-result-of-lab-3)
  - [Lab 5: Complete Case Statement](#lab-5-complete-case-statement)
  - [Lab 6: Synthesis Result of Lab 5](#lab-6-synthesis-result-of-lab-5)
  - [Lab 7: Incomplete Case Handling](#lab-7-incomplete-case-handling)
  - [Lab 8: Partial Assignments in Case](#lab-8-partial-assignments-in-case)
- [4. For Loops in Verilog](#4-for-loops-in-verilog)
- [5. Generate Blocks in Verilog](#5-generate-blocks-in-verilog)
- [6. What is an RCA (Ripple Carry Adder)?](#6-what-is-an-rca-ripple-carry-adder)
- [7. Labs on Loops and Generate Blocks](#7-labs-on-loops-and-generate-blocks)
  - [Lab 9: 4-to-1 MUX Using For Loop](#lab-9-4-to-1-mux-using-for-loop)
  - [Lab 10: 8-to-1 Demux Using Case](#lab-10-8-to-1-demux-using-case)
  - [Lab 11: 8-to-1 Demux Using For Loop](#lab-11-8-to-1-demux-using-for-loop)
  - [Lab 12: 8-bit Ripple Carry Adder with Generate Block](#lab-12-8-bit-ripple-carry-adder-with-generate-block)
- [Summary](#summary)

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
## 4. For Loops in Verilog

A **for loop** is used within procedural blocks (`initial`, `always`, tasks/functions) to execute statements multiple times based on a loop counter.

### Syntax

```verilog
for (initialization; condition; increment) begin
    // Statements to execute
end
```

- Must be inside procedural blocks.
- Synthesizable only if the number of iterations is fixed at compile time.

#### Example: 4-to-1 MUX Using a For Loop

```verilog
module mux_4to1_for_loop (
    input wire [3:0] data, // 4 input lines
    input wire [1:0] sel,  // 2-bit select
    output reg y           // Output
);
    integer i;
    always @(data, sel) begin
        y = 1'b0; // Default output
        for (i = 0; i < 4; i = i + 1) begin
            if (i == sel)
                y = data[i];
        end
    end
endmodule
```

---

## 5. Generate Blocks in Verilog

A **generate block** is used to create hardware structures such as module instances or logic at compile time. Typically used with `for` loops and the `genvar` keyword.

#### Example

```verilog
genvar i;
generate
    for (i = 0; i < 4; i = i + 1) begin : gen_loop
        and_gate and_inst (.a(in[i]), .b(in[i+1]), .y(out[i]));
    end
endgenerate
```

---

## 6. What is an RCA (Ripple Carry Adder)?

An RCA adds binary numbers using a chain of full adders. To add `n` bits, you need `n` full adders. Each carry-out connects to the carry-in of the next stage.

![image](https://github.com/user-attachments/assets/f1ec27d4-b770-4d7a-a418-6435fc81f538)

---

## 7. Labs on Loops and Generate Blocks

### Lab 9: 4-to-1 MUX Using For Loop

```verilog
module mux_generate (
    input i0, input i1, input i2, input i3,
    input [1:0] sel,
    output reg y
);
wire [3:0] i_int;
assign i_int = {i3, i2, i1, i0};
integer k;
always @(*) begin
    for (k = 0; k < 4; k = k + 1) begin
        if (k == sel)
            y = i_int[k];
    end
end
endmodule
```
![mux_generate](https://github.com/user-attachments/assets/80789638-c349-44a9-92f4-7597d5925c63)

---

### Lab 10: 8-to-1 Demux Using Case

```verilog
module demux_case (
    output o0, output o1, output o2, output o3,
    output o4, output o5, output o6, output o7,
    input [2:0] sel,
    input i
);
reg [7:0] y_int;
assign {o7, o6, o5, o4, o3, o2, o1, o0} = y_int;
always @(*) begin
    y_int = 8'b0;
    case(sel)
        3'b000 : y_int[0] = i;
        3'b001 : y_int[1] = i;
        3'b010 : y_int[2] = i;
        3'b011 : y_int[3] = i;
        3'b100 : y_int[4] = i;
        3'b101 : y_int[5] = i;
        3'b110 : y_int[6] = i;
        3'b111 : y_int[7] = i;
    endcase
end
endmodule
```
![demux-case](https://github.com/user-attachments/assets/1836a255-e260-47de-9a8e-45899b19fc03)

---

### Lab 11: 8-to-1 Demux Using For Loop

```verilog
module demux_generate (
    output o0, output o1, output o2, output o3,
    output o4, output o5, output o6, output o7,
    input [2:0] sel,
    input i
);
reg [7:0] y_int;
assign {o7, o6, o5, o4, o3, o2, o1, o0} = y_int;
integer k;
always @(*) begin
    y_int = 8'b0;
    for (k = 0; k < 8; k = k + 1) begin
        if (k == sel)
            y_int[k] = i;
    end
end
endmodule
```
![demux-generate](https://github.com/user-attachments/assets/a5a2c004-a16f-44cd-8d80-c23f1c932e6c)

---

### Lab 12: 8-bit Ripple Carry Adder with Generate Block

```verilog
module rca (
    input [7:0] num1,
    input [7:0] num2,
    output [8:0] sum
);
wire [7:0] int_sum;
wire [7:0] int_co;

genvar i;
generate
    for (i = 1; i < 8; i = i + 1) begin
        fa u_fa_1 (.a(num1[i]), .b(num2[i]), .c(int_co[i-1]), .co(int_co[i]), .sum(int_sum[i]));
    end
endgenerate

fa u_fa_0 (.a(num1[0]), .b(num2[0]), .c(1'b0), .co(int_co[0]), .sum(int_sum[0]));

assign sum[7:0] = int_sum;
assign sum[8] = int_co[7];
endmodule
```
**Full Adder Module:**
```verilog
module fa (input a, input b, input c, output co, output sum);
    assign {co, sum} = a + b + c;
endmodule
```
![rca_org](https://github.com/user-attachments/assets/1d8876f9-e303-4a73-945e-97756a37bb73)

---

> **Note:** Steps to perform the above labs are already shown in [Day 1](https://github.com/Ahtesham18112011/RTL_workshop/tree/main/Day_1).

---

## Summary

- Use complete if-else and case statements to avoid unintended latch inference.
- For loops and generate blocks are powerful for writing scalable, synthesizable code.
- Always ensure every signal is assigned in every possible execution path for combinational logic.
- Use labs to reinforce concepts with practical Verilog code and synthesis results.

---
