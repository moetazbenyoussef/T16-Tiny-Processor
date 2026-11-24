# T16 Instruction Set Architecture (ISA)

## 1. Overview

T16 is an 8-bit processor designed and implemented from scratch using Logisim Evolution.

The processor features a custom instruction set architecture consisting of 16 instructions, with a fixed-width 18-bit instruction format.

During development, the original architecture was extended with new memory addressing capabilities and input/output interfaces. These improvements were implemented without modifying the control unit, by adding combinational logic gates to the datapath.

This document describes both the original architecture and its subsequent improvements.

## 2. Instruction Format

All instructions share the same 18-bit encoding format.

| Field | Width | Description |
|---|---:|---|
| OPCODE | 4 bits | Specifies the operation |
| RD | 3 bits | Destination or first register |
| RS | 3 bits | Source or second register |
| IMM | 8 bits | Immediate value or address |
| **Total** | **18 bits** | |

Binary representation:

```text
OPCODE | RD | RS | IMM
   4   | 3  | 3  |  8 bits
```

All instructions use this format, even when some fields are not required by a particular operation.

## 3. Register Architecture

T16 contains eight general-purpose registers:

| Register | Binary |
|---|---|
| R0 | 000 |
| R1 | 001 |
| R2 | 010 |
| R3 | 011 |
| R4 | 100 |
| R5 | 101 |
| R6 | 110 |
| R7 | 111 |

In the original architecture, registers were used for arithmetic, logical operations, and data storage.

In the extended architecture, registers can also serve as memory pointers through indexed addressing.

R0 has a special role in memory addressing: selecting R0 as RS disables indexed addressing and uses the immediate value directly.

## 4. Instruction Set

The processor implements 16 instructions:

| Opcode | Mnemonic | Description |
|---|---|---|
| 0000 | NOP | No operation |
| 0001 | MOVI | Move immediate value |
| 0010 | ADD | Addition |
| 0011 | SUB | Subtraction |
| 0100 | AND | Bitwise AND |
| 0101 | OR | Bitwise OR |
| 0110 | XOR | Bitwise XOR |
| 0111 | NOT | Bitwise NOT |
| 1000 | LOAD | Load data from memory or input |
| 1001 | STORE | Store data in memory or send output |
| 1010 | JMP | Unconditional jump |
| 1011 | JZ | Jump if Zero flag is set |
| 1100 | JC | Jump if Carry flag is set |
| 1101 | CMP | Compare values |
| 1110 | MOV | Move data between registers |
| 1111 | HALT | Halt execution |

The instruction set remained unchanged during the architectural improvements described below.

## 5. Original Memory Architecture

In the initial version, LOAD and STORE used direct addressing.

The immediate field specified the memory address.

Example:

```text
LOAD 1 0 20;
```

This instruction reads the value stored at memory address `0x20` and places it in R1.

The RS field was not used to calculate the memory address.

## 6. Architectural Improvements

The original processor was extended with three main improvements:

1. Indexed memory addressing.
2. Dynamic output interface.
3. Input interface.

These improvements were implemented by adding combinational logic gates to the datapath.

The control unit was not modified. The new logic uses the existing LOAD and STORE control signals to determine when the additional data-routing operations should take place.

### 6.1 Indexed Addressing

**Original behavior:**

The effective memory address was always equal to IMM.

**Extended behavior:**

The processor can now calculate the effective address using a register value.

When RS is R0, the processor uses direct addressing:

```text
EA = IMM
```

When RS is not R0, the processor adds the contents of RS to IMM:

```text
EA = IMM + R[RS]
```

Example:

```text
R2 = 0x10
LOAD 1 2 05;
```

The effective address becomes:

```text
EA = 0x10 + 0x05
EA = 0x15
```

The processor reads the value at address `0x15` and stores it in R1.

This allows registers to act as memory pointers and makes memory access more flexible.

### 6.2 Dynamic Output Interface

**Original behavior:**

The output mechanism was limited to a specific register and immediate-value configuration.

This restricted the flexibility of communication with external devices.

**Extended behavior:**

The STORE instruction now supports dynamic output selection.

When IMM equals `0xFF`, the processor activates the output interface and sends the contents of RD and RS to it.

Example:

```text
STORE 2 3 FF;
```

This sends the values stored in R2 and R3 to the output interface.

The output data is no longer restricted to a single register.

The new output routing is handled by additional combinational logic using the existing STORE control signal.

### 6.3 Input Interface

The original architecture did not provide the current dedicated input mechanism.

The extended architecture introduces an input interface accessible through LOAD.

When IMM equals `0xFE`, the processor reads data from the input interface and stores it in RD.

Example:

```text
LOAD 1 0 FE;
```

The input value is loaded into R1.

The input routing is implemented using additional combinational logic controlled by the existing LOAD signal.

## 7. Datapath Modifications

An important aspect of the processor's development is that the control unit remained unchanged.

Instead of introducing new instructions or redesigning the control unit, the additional capabilities were implemented by extending the datapath.

The added combinational logic uses:

- Existing LOAD and STORE control signals.
- The immediate field (IMM).
- Register selection fields (RD and RS).
- Register values where required.

This logic determines which data source or destination should be selected for each operation.

Consequently, the processor gained new addressing and I/O capabilities while preserving its original instruction set and control-unit design.

## 8. Machine Code Representation

Each instruction occupies 18 bits.

For hexadecimal representation, two leading zero bits are added to produce a 20-bit value.

Therefore, each instruction is represented by five hexadecimal digits.

The two leading bits are padding bits and are not part of the actual instruction.

## 9. Architectural Comparison

| Feature | Original Version | Extended Version |
|---|---|---|
| Instruction set | 16 instructions | 16 instructions |
| Instruction width | 18 bits | 18 bits |
| Registers | 8 | 8 |
| Direct addressing | Supported | Supported |
| Indexed addressing | Not supported | Supported |
| Register-based memory pointers | Not supported | Supported |
| Dynamic output | Restricted output mechanism | RD and RS can be sent to output |
| Dedicated input interface | Not available | Supported |
| Control unit | Original design | Unchanged |
| Datapath | Original data paths | Extended with combinational routing logic |

## 10. Project Development

T16 was designed and implemented from scratch using Logisim Evolution.

Its development involved designing:

- The instruction set architecture.
- The register architecture.
- The arithmetic and logic unit (ALU).
- The control unit.
- Memory interfaces.
- The execution datapath.
- The input/output interfaces.

A Python-based assembler was also developed to translate assembly instructions into machine code.

The project evolved from a basic processor with direct memory addressing into a more flexible architecture supporting indexed addressing and external input/output communication, while maintaining the original instruction set and control unit.
