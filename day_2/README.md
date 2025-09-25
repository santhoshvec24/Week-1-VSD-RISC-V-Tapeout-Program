# Day 2: Timing Libraries, Synthesis Approaches, and Efficient Flip-Flop Coding Styles
- In Day 2, we are going to see the Introduction to timing `.lib`(sky130_fd_sc_hd__tt_025C_1v80.lib) used in open-source PDKs.
- Comparing hierarchical vs. flat synthesis methods.
- Exploring efficient coding styles for flip-flops in RTL design.
---

# Contents

- [Timing Libraries](#timing-libraries)
  - [SKY130 PDK Overview](#sky130-pdk-overview)
  - [Decoding tt_025C_1v80 in the SKY130 PDK](#decoding-tt_025c_1v80-in-the-sky130-pdk)
  - [Opening and Exploring the .lib File](#opening-and-exploring-the-lib-file)

- [Hierarchical vs. Flattened Synthesis](#hierarchical-vs-flattened-synthesis)
  - [Hierarchical Synthesis](#hierarchical-synthesis)
  - [Flattened Synthesis](#flattened-synthesis)
  - [Key Differences](#key-differences)

- [Flip-Flop Coding Styles](#flip-flop-coding-styles)
  - [Asynchronous Reset D Flip-Flop](#asynchronous-reset-d-flip-flop)
  - [Asynchronous Set D Flip-Flop](#asynchronous-set-d-flip-flop)
  - [Synchronous Reset D Flip-Flop](#synchronous-reset-d-flip-flop)

- [Simulation and Synthesis Workflow](#simulation-and-synthesis-workflow)
  - [Icarus Verilog Simulation](#icarus-verilog-simulation)
  - [Synthesis with Yosys](#synthesis-with-yosys)

---
**Timing Libraries:**
### SKY130 PDK Overview
The SKY130 PDK is an open-source Process Design Kit based on SkyWater Technology's 130nm CMOS technology. It provides essential models and libraries for integrated circuit (IC) design, including timing, power, and process variation information.

In `.lib` timing libraries like `sky130_fd_sc_hd__tt_025C_1v80.lib`, the filename itself encodes the PVT (Process, Voltage, Temperature) conditions used to characterize the cells. Understanding this naming convention helps correlate the library to the specific conditions:

- **sky130_fd_sc_hd** – Base library name:  
  - `sky130` → SkyWater 130nm process  
  - `fd` → Fully Depleted (process type)  
  - `sc` → Standard Cells  
  - `hd` → High Density variant 

#### Decoding tt_025C_1v80 in the SKY130 PDK

`tt`: Typical process corner.
 - `tt` → Typical-Typical (nominal NMOS and PMOS)

`025C`: Represents a temperature of 25°C, relevant for temperature-dependent performance.

`1v80`: Indicates a core voltage of 1.8V.

This systematic naming allows tools to select the appropriate library for synthesis, timing analysis, or corner-based verification.
The delay model LUT is used for the timing analysis.

--- 

### Understanding the lib file
- Collection of standard logic modules.
- It consists of all the logic gates with the variation in inputs given to it.
- It has same gates with different flavors.
- We need cells that work fast to meet the specific performance and also cells that work slow to meet HOLD.

The `.lib` file contains detailed definitions of each standard cell, including:

- Cell name and type (e.g., AND, OR, DFF)
- Pin definitions (input, output, clock)
- Timing information (setup, hold, propagation delays)
- Power characteristics (dynamic and leakage power)
- Conditions for PVT corners (process, voltage, temperature)

### Opening and Exploring the .lib File

To open the sky130_fd_sc_hd__tt_025C_1v80.lib file:

**Open the file:**
```shell
vim sky130_fd_sc_hd__tt_025C_1v80.lib
```

<img width="1920" height="922" alt="Screenshot from 2025-09-24 16-49-48" src="https://github.com/user-attachments/assets/894de130-7b8d-4a41-a5a1-b68804fc352c" />

---

## *What is Synthesis*

Synthesis is the process of converting RTL code (Verilog/VHDL) into a gate-level netlist using a standard cell library.
It maps high-level design into actual logic gates that can be fabricated.

---

## Hierarchical Synthesis
- The design is synthesized module by module, preserving the hierarchy of the RTL.  
- **Advantages:**
  - Each module is synthesized separately.
  - Preserves design structure → easier debugging and reuse.
  - Netlist is **organized by modules**.
  - Easier to debug and manage large designs.
  - Reusable modules can be synthesized separately.
  - Reduces runtime and memory usage for very large designs.
- **Disadvantages:**
  - May miss some cross-module optimizations.
  - Overall area or timing may be slightly less optimal compared to flat synthesis.

### Flat Synthesis
- The entire design is synthesized as a **single flattened block**, ignoring module boundaries.  
- **Advantages:**
  - Allows the synthesis tool to optimize across module boundaries.
  - Can achieve better area, timing, and power optimization.
  - May achieve **better performance/area**.  
  - Netlist is a **single-level structure**.  

- **Disadvantages:**
  - Requires more memory and runtime for large designs.
  - Harder to debug or modify specific parts of the design.

In practice, designers often use a **mixed approach**, keeping critical modules hierarchical while flattening performance-critical paths to achieve a balance between manageability and optimization.

<img width="867" height="604" alt="Screenshot from 2025-09-25 14-16-19" src="https://github.com/user-attachments/assets/2b57d3f7-76e8-4182-89ec-b48a2f8598a5" />

## Hierarchical Synthesis Workflow

### Hierarchical synthesis of multiple_modules.v using Yosys:
1. Open the directory where you would want to run the synthesis



