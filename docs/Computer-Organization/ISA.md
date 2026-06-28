---
sidebar_position: 1
title: Instruction Set Architecture(MIPS)
sidebar_label: ISA
---

# Introduction

Instruction Set Architecture (ISA) is the "contract" or "communication interface" between hardware and software. It defines the machine characteristics visible to an assembly language programmer or compiler, including instruction types, number and size of registers, and memory addressing modes.

## Instruction Categories

- Load/Store
- Computational
- Jump and Branch
- Floating Point
- Memory Management
- Special

## Registers

- Characteristics: Built inside the CPU, extremely fast, few in number. MIPS has thirty-two 32-bit (4 bytes) general-purpose registers. A 32-bit unit of data is called a Word.

- Common Conventions:
  - $zero (0): Always 0, hardwired.
  - $s0-$s7 (16-23): Saved values (variables).
  - $t0-$t9 (8-15, 24-25): Temporaries (temporary results).
  - $a0-$a3 (4-7): Arguments (passing function parameters).
  - $v0-$v1 (2-3): Values (returning function results).
  - $sp (29): Stack pointer.
  - $ra (31): Return address.

## Memory

- Characteristics: Large capacity, slower speed. Data is a one-dimensional array, addressed by Byte.

- Word Alignment: Because one Word is 4 Bytes, in MIPS, a Word's memory address must always be a multiple of 4 (0, 4, 8, 12...).

- Endianness:
  - Big Endian: Most Significant Byte (MSB) is stored at the lowest address.
  - Little Endian: Least Significant Byte (LSB) is stored at the lowest address (e.g., x86).

## Instruction Formats

All MIPS instructions have a fixed length of 32 bits. To accommodate different needs, they are divided into three formats:

1. R-Format (Register) - For register-only operations

```
op (6) rs (5) rt (5) rd (5) shamt (5) funct (6)
```

- op + funct: Determines what the instruction is (e.g., add, sub).

- rs, rt: Source registers.

- rd: Destination register.

- shamt: Shift amount, used for shift instructions.

2. I-Format (Immediate) - For constant operations, data transfer (Load/Store), conditional branches

```
op (6) rs (5) rt (5) constant / address (16)
```

- rt: Often acts as the destination register in I-format.

- constant: 16-bit immediate value. If it's a signed number, hardware will perform Sign-Extension to 32-bit before the operation.

3. J-Format (Jump) - For unconditional jumps

```
op (6) target address (26)
```

## Procedure Call Mechanism

- When a main program (Caller) calls a function (Callee), a strict S.O.P. must be followed:

1. Pass Arguments: Caller puts parameters in $a0 - $a3.

2. Jump and Link: Use jal ProcedureAddress (Jump and Link). This jumps to the function and saves the "address of the next instruction (PC+4)" into $ra (Return Address).

3. Allocate Space (Stack): If the Callee needs space, or needs to modify $s0-$s7, it must first Push the old values onto the Stack to save them. The stack pointer $sp grows downward (subtraction).

4. Execute Task: Execute the function body.

5. Store Results: Put return values in $v0, $v1.

6. Restore State: Pop the saved values from the Stack to restore them, and add back to $sp.

7. Return to Caller: Use jr $ra (Jump Register) to return to the Caller and continue execution.

## Addressing Modes

- How does MIPS find the location of data or the next instruction in hardware? There are 5 ways:

1. Register Addressing: Data is in a register (e.g., add).

2. Base (Displacement) Addressing: Memory address = register content + 16-bit constant (e.g., lw, sw).

3. Immediate Addressing: Data is written directly in the 16-bit field of the instruction (e.g., addi).

4. PC-relative Addressing: Used for conditional branches (beq, bne). Target address = PC + (16-bit constant \* 4). Jumps forward or backward relative to the current PC.

5. Pseudodirect Addressing: Used for j, jal. Target address = first 4 bits of PC concatenated with 26-bit field \* 4.
