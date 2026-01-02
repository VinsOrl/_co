# The Hack Computer Architecture: From NAND to Tetris
**A 16-bit Von Neumann Machine Built from First Principles**

## 🚀 Project Overview
This repository contains my full implementation of the **Hack Computer**, a 16-bit general-purpose system designed as part of the *Nand2Tetris* curriculum. 

The project follows a strictly bottom-up methodology: starting from a single **NAND** gate, I synthesized the elementary logic gates, the Arithmetic Logic Unit (ALU), the memory hierarchy, and finally the Central Processing Unit (CPU). This architecture is capable of running any program written in the Hack machine language, including interactive programs and mathematical simulations.

---

## 🏗️ Hardware Architecture

### 1. Part I: Combinational Logic & Arithmetic
The initial phase focused on "stateless" logic, optimizing gate count while maintaining clear signal paths.

#### Basic Gate Synthesis
I derived all standard gates using structural **HDL (Hardware Description Language)**:
* **Elementary Gates:** `Not`, `And`, `Or`, `Xor`.
* **Logic Reference:** The implementation relied on a core truth table for basic gate behavior:

| A | B | **Nand** | **And** | **Or** | **Xor** |
| :---: | :---: | :---: | :---: | :---: | :---: |
| 0 | 0 | **1** | 0 | 0 | 0 |
| 0 | 1 | **1** | 0 | 1 | 1 |
| 1 | 0 | **1** | 0 | 1 | 1 |
| 1 | 1 | **0** | 1 | 1 | 0 |


<p align="center">
  <img src="LogicGates.jpg" width="30%">
</p>


* **Advanced Routing:** Multi-way variants (e.g., `Mux8Way16`) utilize a recursive tree structure for logarithmic selection speed.

#### The ALU (Arithmetic Logic Unit)
The ALU is the computational heart of the system, processing two 16-bit inputs ($x, y$) based on six control bits.
* **Logic Flow:** Inputs pass through zeroing and negation logic before entering a parallel adder/AND unit.
* **Status Flags:** Implemented `zr` (zero) and `ng` (negative) flags using internal bus slicing and high-fan-in OR gates for conditional branching.
* **Adder Design:** Built using a chain of `FullAdders` to facilitate 16-bit binary arithmetic.



---

### 2. Part II: Sequential Logic & Memory Systems
The introduction of a master clock allows for stateful devices that "remember" data across clock cycles.

* **Data Flip-Flop (DFF):** The atomic unit of memory used to build 1-bit cells.
* **RAM Hierarchy:** Organized into banks where a 14-bit address navigates the `RAM16K` module; MSBs select the sub-bank and LSBs select the specific register.
* **Program Counter (PC):** A specialized register supporting `Reset`, `Load`, and `Inc` operations, crucial for controlling program flow.



---

### 3. Part III: System Integration (The CPU)
The final milestone was the construction of the CPU and the top-level `Computer.hdl` chip.

#### CPU Architecture
The CPU decodes 16-bit instructions using a control unit to determine:
* **Instruction Type:** Distinguishes between **A-instructions** (data/addressing) and **C-instructions** (computations).
* **ALU Configuration:** Mapping instruction bits directly to ALU control pins.
* **Flow Control:** Executing conditional jumps by evaluating ALU status flags against instruction jump bits.



#### Memory Mapping
Integrated a unified memory space using address decoding:
* `0x0000 - 0x3FFF`: **Data RAM** (16K).
* `0x4000 - 0x5FFF`: **Screen Memory Map** (8K).
* `0x6000`: **Keyboard Input Register**.

---

## 💻 Low-Level Programming
To validate the architecture, I developed programs in **Hack Assembly Language**:
* **`Mult.asm`**: A software implementation of multiplication using an optimized repeated-addition loop.
* **`Fill.asm`**: An I/O driver interacting with memory-mapped Screen and Keyboard to provide a real-time feedback loop.

---

## 🛠️ Testing & Simulation
All components were verified using the **Nand2Tetris Hardware Simulator**:
1. Load the `.hdl` chip definition.
2. Execute the corresponding `.tst` script.
3. Ensure the output matches the gold-standard `.cmp` file.

> **Note:** For large-scale chips like `RAM16K`, simulation speed was set to "Fast" to accommodate the high number of internal gate transitions per clock cycle.

---

## 📂 Engineering Notes
* **Efficiency:** Used internal pin naming conventions in HDL to keep code readable and debugging efficient.
* **Clock Cycles:** All sequential chips update on the "tock" (falling edge) of the system clock, ensuring stable data transmission.