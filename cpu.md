# CPU

The V-Smile processor is a Sunplus μnSP implementing version 1.2 of the ISA.

This document does not attempt to distinguish between ISA versions, so information contained may not be applicable to other Sunplus chipsets.

[Registers](#registers)

[Address Space](#address-space)

[Interrupts](#interrupts)

[Instruction Set](#instruction-set)

## Registers

| ID | Name | Function |
| - | - | - |
| 0 | SP | Stack Pointer |
| 1 | R1 | General Purpose |
| 2 | R2 | General Purpose |
| 3 | R3 | General Purpose |
| 4 | R4 | General Purpose |
| 5 | BP | Base Pointer |
| 6 | SR | Status Register |
| 7 | PC | Program Counter |

### Shadow Registers 

Registers R1-R4 have 'shadow' copies that can be enabled via the `SECBANK` instruction. When enabled, all reads and writes to R0-R4 are redirected to SR1-SR4. This can be used t avoid backing up, and restoring registers during Interrupt routines.

### Status Register

The status register contains various status flags, as well as the current code segment (CS) and data segment (DS) base addresses.

| Bits | Name | Description | Notes |
| - | - | - | - |
| 0-5 | CS | Code Segment | |
| 6 | C | Carry Flag | Set if a carry occurred |
| 7 | S | Sign Flag | Set if the result is negative (two's complement) | 
| 8 | Z | Zero Flag | Set when the result is zero |
| 9 | N | Negative Flag | Set when the MSB of the result is 1 |
| 10-15 | DS | Data Segment | |

# Address Space

The memory map of the μnSP is split into 64K sized pages. The entire 4M address space of the CPU is divided into 64 pages (0x00-0x3F). The current page can be selected via the segment registers, CS (for instruction fetch) and DS (for data operations).

When code execution reaches the end of the current page, the CS register is auto-incremented by the hardware.

# Interrupts

| Vector | Name |
| - | - |
| 0xFFF5 | BREAK |
| 0xFFF6 | FIQ |
| 0xFFF7 | RESET |
| 0xFFF8 | IRQ0 |
| 0xFFF9 | IRQ1 |
| 0xFFFA | IRQ2 |
| 0xFFFB | IRQ3 |
| 0xFFFC | IRQ4 |
| 0xFFFD | IRQ5 |
| 0xFFFE | IRQ6 |
| 0xFFFF | IRQ7 |

# Instruction Set

These tables detail the instruction both in the format used by the official μnSP toolchain as well as the mnemonic form used by the open-source Smile Assembler (smasm) 

NOTE: Unless otherwise specified, instructions that operate on memory ignore DS and only operate on page 0 (0x0000-0xFFFF). Instruction varients that allow a "D:" prefix generate a full 22-bit address via `(DS << 16 | addr)`

[Data Transfer](#data-transfer)

[Data Processing](#processing)

[Data Segment Access](#data-segment-access)

[Program Flow](#program-flow)

[Other](#other)

## Data Transfer

### Load

| Instruction | Smasm Form | Notes | Flags Affected |
| - | - | - | - |
| Rd = Value | mov rd, value |  | NZ |
| Rd = [BP + offset] | mov rd, [BP + offset] | Offset is limited to 6 bits | NZ |
| Rd = [addr] | mov rd, [addr] | | NZ | 
| Rd = Rs | mov rd, rs | | NZ |
| Rd = {D:}[Rs] <br> Rd = {D:}[++Rs] <br> Rd = {D:}[Rs--] <br> Rd = {D:}[Rs++] | mov rd, {D:}[rs] <br> mov rd, {D:}[++rs] <br> mov rd, {D:}[rs--] <br> mov rd, {D:}[rs++] | Optional data-segment qualifier (D:) | NZ |

### Store

| Instruction | Smasm Form | Notes | Flags Affected |
| - | - | - | - |
| [BP + offset] = Rd | mov [BP + offset], rd | Offset is limited to 6 bits | |
| [addr] = Rd | mov [addr], rd | | |
| {D:}[Rs] = Rd <br> {D:}[++Rs] = Rd <br> {D:}[Rs--] = Rd <br> {D:}[Rs++] = Rd | mov {D:}[rs], rd <br> mov {D:}[++rs], rd <br> mov {D:}[rs--], rd <br> mov {D:}[rs++] = rd | Optional data-segment qualifier (D:) | |

### Push/Pop

| Instruction | Smasm Form | Notes | Flags Affected |
| - | - | - | - |
| PUSH Rx, Ry to [Rs] <br> PUSH Rx to [Rs] | push rx-ry [rs] <br> push rx, [rs] | rx-ry signifies a range of registers to push | | 
| POP Rx, Ry from [Rs] <br> POP Rx from [Rs] | pop rx-ry [rs] <br> pop rx, [rs] | rx-ry signifies a range of registers to pop | |

## Data Processing

### Add

| Instruction | Smasm Form | Notes | Flags Affected |
| - | - | - | - |
| Rd += Value | add rd, value | | NZSC |
| Rd += [BP + offset] | add rd, [BP + offset] | | NZSC |
| Rd += [addr] | add rd, [addr] | | NZSC |
| [addr] = Rd + Rs | add [addr], rd, rs | | NZSC |
| Rd += {D:}[Rs] <br> Rd += {D:}[++Rs] <br> Rd += {D:}[Rs--] <br> Rd += {D:}[Rs++] | add rd, {D:}[rs] <br> add rd, {D:}[++rs] <br> add rd, {D:}[rs--] <br> add rd, {D:}[rs++] | Optional data-segment qualifier (D:) | NZSC |

### Add with Carry

| Instruction | Smasm Form | Notes | Flags Affected |
| - | - | - | - |
| Rd += Value, Carry | adc rd, value | | NZSC |
| Rd += [BP + offset], Carry | adc rd, [BP + offset] | | NZSC |
| Rd += [addr], Carry | adc rd, [addr] | | NZSC |
| [addr] = Rd + Rs, Carry | adc [addr], rd, rs | | NZSC |
| Rd += {D:}[Rs], Carry <br> Rd += {D:}[++Rs], Carry <br> Rd += {D:}[Rs--], Carry <br> Rd += {D:}[Rs++], Carry | adc rd, {D:}[rs] <br> adc rd, {D:}[++rs] <br> adc rd, {D:}[rs--] <br> adc rd, {D:}[rs++] | Optional data-segment qualifier (D:) | NZSC |


### Sub

| Instruction | Smasm Form | Notes | Flags Affected |
| - | - | - | - |
| Rd -= Value | sub rd, value | | NZSC |
| Rd -= [BP + offset] | sub rd, [BP + offset] | | NZSC |
| Rd -= [addr] | sub rd, [addr] | | NZSC |
| [addr] = Rd - Rs | sub [addr], rd, rs | | NZSC |
| Rd -= {D:}[Rs] <br> Rd -= {D:}[++Rs] <br> Rd -= {D:}[Rs--] <br> Rd -= {D:}[Rs++] | sub rd, {D:}[rs] <br> sub rd, {D:}[++rs] <br> sub rd, {D:}[rs--] <br> sub rd, {D:}[rs++] | Optional data-segment qualifier (D:) | NZSC |

### Sub with Carry

| Instruction | Smasm Form | Notes | Flags Affected |
| - | - | - | - |
| Rd -= Value, Carry | sbc rd, value | | NZSC |
| Rd -= [BP + offset], Carry | sbc rd, [BP + offset] | | NZSC |
| Rd -= [addr], Carry | sbc rd, [addr] | | NZSC |
| [addr] = Rd - Rs, Carry | sbc [addr], rd, rs | | NZSC |
| Rd -= {D:}[Rs], Carry <br> Rd -= {D:}[++Rs], Carry <br> Rd -= {D:}[Rs--], Carry <br> Rd -= {D:}[Rs++], Carry | sbc rd, {D:}[rs] <br> sbc rd, {D:}[++rs] <br> sbc rd, {D:}[rs--] <br> sbc rd, {D:}[rs++] | Optional data-segment qualifier (D:) | NZSC |

### Negate

| Instruction | Smasm Form | Notes | Flags Affected |
| - | - | - | - |
| Rd = -Value | neg rd, value | | NZ |
| Rd = -[BP + offset] | neg rd, [BP + offset] | | NZ |
| Rd = -[addr] | neg rd, [addr] | | NZ |
| [addr] = -Rd | neg [addr], rd, rs | | NZ |
| Rd = -{D:}[Rs] <br> Rd = -{D:}[++Rs] <br> Rd = -{D:}[Rs--] <br> Rd =- {D:}[Rs++] | neg rd, {D:}[rs] <br> neg rd, {D:}[++rs] <br> neg rd, {D:}[rs--] <br> neg rd, {D:}[rs++] | Optional data-segment qualifier (D:) | NZ |

### Compare
| Instruction | Smasm Form | Notes | Flags Affected |
| - | - | - | - |
| CMP Rd, Value | cmp rd, value | | NZSC |
| CMP Rd, [BP + offset] | cmp rd, [BP + offset] | | NZSC |
| CMP Rd, [addr] | cmp rd, [addr] | | NZSC |
| CMP Rd, Rs | cmp rd, rs | | NZSC |
| CMP Rd, {D:}[Rs] <br> CMP Rd, {D:}[++Rs] <br> CMP Rd, {D:}[Rs--] <br> CMP Rd, {D:}[Rs++] | cmp rd, {D:}[rs] <br> cmp rd, {D:}[++rs] <br> cmp rd, {D:}[rs--] <br> cmp rd, {D:}[rs++] | Optional data-segment qualifier (D:) | NZSC |

### XOR

### OR

### AND

### TEST

### MUL

Multiplication result is stored in R3 and R4 (the register pair is called MR)

| Instruction | Smasm Form | Notes | Flags Affected |
| - | - | - | - |
|MR = Rd x Rs| |Signed multiplication| |
|MR = Rd x Rs,us| |Rd is unsigned, Rs is signed| |
|FIRJMOV OFF| |Disable automatic data movement for multiply-accumulate operations| |
|FIRJMOV ON| |Enable automatic data movement for multiply-accumulate operations| |
|MR = [Rd] x [Rs],n| |Multiply-accumulate two sets of N signed values pointed by Rd and Rs| |
|MR = [Rd] x [Rs],us,n| |Multiply-accumulate two sets of N values pointed by Rd (unsigned values) and Rs (signed values)| |

## Data Segment Access

TODO:

## Program Flow

### Conditional jumps

Syntax:

    JMP label

The target address is stored as a 6 bit displacement. So this can only jump back and forward 63 addresses.

| Instruction | Smasm Form | Notes | Flags Affected |
| - | - | - | - |
|JCC,JB,JNAE| |Jump if C=0| |
|JCS,JNB,JAE| |Jump if C=1| |
|JSC,JGE,JNL| |Jump if S=0| |
|JSS,JNGE,JL| |Jump if S=1| |
|JNE,JNZ| |Jump if Z=0| |
|JZ,JE| |Jump if Z=1| |
|JPL| |Jump if N=0| |
|JMI| |Jump if N=1| |
|JBE,JNA| |Jump if Z=1 or C=0| |
|JNA| |Jump if Z=1 or C=0| |
|JNBE,JA| |Jump if Z=0 and C=1| |
|JLE,JNG| |Jump if Z=1 or S=1| |
|JNLE,JG| |Jump if Z=0 and S=0| |
|JVC| |Jump if N=S| |
|JVS| |Jump if N != S| |
|JMP| |Jump always| |

### Other instructions

| Instruction | Smasm Form | Notes | Flags Affected |
| - | - | - | - |
|CALL Label| |Push PC and SR to the stack, and far jump to label. This sets both PC and the extended PC bits in SR, allowing to call anywhere in memory| |
|RETF| |Return from subroutine. Restores SR and PC from the stack| |
|RETI| |Return from interrupt. Restores SR and PC from the stack and restores interrupt flags| |
|GOTO Label| |Far jump (sets both PC and SR). Previous PC and SR are not saved on the stack| |
|BREAK| |System call| |
|NOP| |Do nothing| |

## Interrupt control

| Instruction | Smasm Form | Notes | Flags Affected |
| - | - | - | - |
|IRQ OFF| |Disable interrupts| |
|IRQ ON| |Enable interrupts| |
|FIQ OFF| |Disable fast interrupts| |
|FIQ ON| |Enable fast interrupts| |
|INT FIQ| |Enable FIQ, disable IRQ| |
|INT FIQ,IRQ| |Enable FIQ and IRQ| |
|INT IRQ| |Disable FIQ, enable IRQ| |
|INT OFF| |Disable FIQ and IRQ| |
