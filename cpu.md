# CPU

The V-Smile processor is a Sunplus μnSP, implementing version 1.2 of the ISA.

[Registers](#registers)

[Address Space](#address-space)

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

# Instruction Set
 
TODO.
