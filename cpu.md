# CPU

The V-Smile processor is a Sunplus μnSP, implementing version 1.2 of the ISA.

[Registers](#registers)

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

# Instruction Set
 
TODO.
