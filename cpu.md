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

## Load

| Instruction | Description | Flags Effected |
| - | - | - |
| Rd = Imm6 | Load 6-bit value into Rd | NZ |
| Rd = Imm16 | Load 16-bit value into Rd| NZ |
| Rd = [BP + Imm6] | Load 16-bit value from memory address (BP + 6-bit offset) | NZ |
| Rd = [A6] | Load 16-bit value from 6-bit memory address | NZ | 
| Rd = [A16] | Load 16-bit value from 16-bit memory address | NZ |
| Rd = Rs | Load 16-bit value from Rs | NZ |
| Rd = {D}:[({++}Rs{--/++)] | Load 16-bit value from Memory Address, with optional data-segment qualifier, optional pre-increment, optional post inc/decrement | NZ |

