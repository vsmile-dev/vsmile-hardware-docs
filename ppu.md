# PPU

The V-Smile PPU provides three layers of graphics: Two background planes and one sprite layer.

Currently this information is sourced from unununium and mame. Some features may be exclusive to newer chipsets and not be available on V-Smile.

## Registers

| Offset | Name |
| - | - |
| 0x2810 - 0x281B | [Page Registers](#page-registers) |
| 0x2820 - 0x2822 | [Segment Registers](#segment-registers) |
| 0x282A | Blind Level Control |
| 0x2830 | Fade Effect Control |
| 0x2836 - 0x2837 | IRQ Position |
| 0x283C - 0x283D| TV Control |
| 0x2842  | Sprite Control |
| 0x2862 | Video IRQ Enable | 
| 0x2863 | Video IRQ Acknowledge |
| 0x2870 - 0x2872 | Sprite DMA |

### Page Registers

These registers use the base address of 0x2810 for Page 1 and 0x2816 for Page 2

 [0x00 - Scroll](#0x00---scroll)

 [0x02 - Attributes](#0x02---attributes)
 
 [0x03 - Control](#0x03---control)
 
 [0x04 - Tile Address](#0x04---tile-address)
 
 [0x05 - Extended Attribute Address](#0x05---extended-attribute-address)
 
#### 0x00 - Scroll 

| Register | Function | Description | 
| - | - | - |
| 0x0 | X-Scroll | ? |
| 0x1 | Y-Scroll | ? |

#### 0x02 - Attributes

| Bits | Function | Description | 
| - | - | - |
| 0-1 | Bits Per Pixel | 0 = 2bpp, 1 = 4bpp, 2 = 6bpp, 3 = 8bpp | 
| 2 | X-Flip | 0 = Not Mirrored, 1 = Mirrored Horizontally |
| 3 | Y-Flip | 0 = Not Mirrored, 1 = Mirror Vertically | 
| 4-5 | Width | 0 = 0, 1 = 256, 2 = 512, 3 = 768 |
| 6-7 | Height | 0 = 0, 1 = 256, 2 = 512, 3 = 768 |
| 8-11 | Palette Bank | Offset into Palette Ram. 0 = 0, 1 = 0x100 |
| 12-13 | Depth | ? |
| 14-15 | Unused? | ? |

#### 0x03 - Control

| Bits | Function | Description | 
| - | - | - |
| 0 | Bitmap Mode | 0 = Tiled, 1 = Bitmap | 
| 1 | RegSet | ? |
| 2 | Wallpaper | ? |
| 3 | Page Enable | 0 = Disabled, 1 = Enabled |
| 4 | Row Scroll | 0 = Disabled, 1 = Enabled |
| 5 | Horizontal Compression | ? |
| 6 | Vertical Compression | ? |
| 7 | Hi-Color | 0 = Paletted, 1 = Direct Colour |
| 8 | Blend Enable | 0 = Not Blended, 1 = Blended |
| 9-15 | Unused? | ? |

#### 0x04 - Tile Address

| Bits | Function | Description | 
| - | - | - |
| 0-12 | Tilemap Address | Memory Address of Tile-Map (0x0000-0x1FFF) |
| 13-15 | Unused | |

#### 0x05 - Extended Attribute Address

| Bits | Function | Description | 
| - | - | - |
| 0-12 | Attribute Data Address | Memory Address of Attribute Data |
| 13-15 | Unused | |

## Segment Registers

| Register | Function | Description | 
| - | - | - |
| 0x2820 | Page 1 Segment | Page 1 Tile Data Address (Gets multipied by 0x40) |
| 0x2821 | Page 2 Segment | Page 2 Tile Data Address (Gets multipied by 0x40) |
| 0x2822 | Sprite Segment | Sprite Data Address (Gets multipied by 0x40) |





