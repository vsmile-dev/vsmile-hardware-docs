# Memory Map

TODO: Write about bank layout, how to switch banks, etc.

## SoC Internal Memory

Regardless of the selected bank, the memory range 0x0000-0x3FFF is *always* mapped as follows:

| Start | End | Description |
| - | - | - |
| 0x000000 | 0x0027FF | RAM |
| 0x002800 | 0x002FFF | PPU |
| 0x003000 | 0x0037FF | Audio |
| 0x003D00 | 0x003DFF | IO |
| 0x003e00 | 0x003E03 | DMA |

## Banks

Memory Space is split into banks, only one bank can be selected at one time.

TODO: Verify this. It's sourced from mame but it seems a little odd to me.

### Banks 0-3

| Start | End | Description |
| - | - | - |
| 0x000000 | 0x0FFFFF | Internal Rom |
| 0x100000 | 0x1FFFFF | Internal Rom (Mirror) |
| 0x200000 | 0x2FFFFF | Internal Rom (Mirror) |
| 0x300000 | 0x3FFFFF | Internal Rom (Mirror) |

### Bank 4

| Start | End | Description |
| - | - | - |
| 0x000000 | 0x3FFFFF | Cartridge (Bank 0) |

#### Bank 5

| Start | End | Description |
| - | - | - |
| 0x000000 | 0x1FFFFF | Cartridge (Bank 0) |
| 0x200000 | 0x3FFFFF | Cartridge (Bank 2) |


#### Bank 6

| Start | End | Description |
| - | - | - |
| 0x000000 | 0x0FFFFF | Cartridge (Bank 0) |
| 0x100000 | 0x1FFFFF | Cartridge (Bank 1) |
| 0x200000 | 0x2FFFFF | Cartridge (Bank 2) |
| 0x300000 | 0x3FFFFF | Cartridge (Bank 3) |

