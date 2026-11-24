##T16 Processor

**A custom 8-bit processor designed and implemented from scratch using Logisim Evolution.**

T16 is an educational CPU project developed to explore computer architecture, digital logic design, instruction set architecture (ISA), and processor implementation.

The processor was designed from the ground up, including its instruction set, ALU, control unit, registers, memory interface, and execution logic.

## Architecture Specifications

Architecture              :  8-bit 
General-Purpose Registers :  8 (R0–R7) 
Instruction Width         :  18 bits 
Instruction Set           :  16 instructions 
ROM                       :  256 instructions 
RAM                       :  256 × 8 bits 
Design Environment        :  Logisim Evolution
Assembler                 :  Python 


## Main Components

- ALU             : Performs arithmetic and logical operations.
- Control Unit    : Decodes instructions and generates control signals.
- Register File   : Contains eight general-purpose registers.
- Program Counter : Tracks the address of the next instruction.
- ROM             : Stores the instruction program.
- RAM             : Stores data during execution.
- Flags           : Includes status flags such as Zero and Carry.

## Software

A Python-based assembler translates T16 assembly instructions into machine code.
This allows programs to be written in a human-readable assembly format before being loaded into the processor.

## Development Environment

- Logisim Evolution — Digital circuit design and simulation.
- Python — Assembler development.

## Project Goals

The main objectives of T16 are:

1. Understand the internal architecture of a processor.
2. Design a custom instruction set architecture.
3. Implement arithmetic and logical operations using digital logic.
4. Build an instruction decoding and execution system.
5. Understand memory addressing and data transfer.
6. Develop a complete processor capable of executing custom assembly programs.

**T16 is an independent educational hardware design project.**
