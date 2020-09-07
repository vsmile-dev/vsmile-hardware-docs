# PPU

The V-Smile PPU provides three layers of graphics: Two background planes and one sprite layer.

## Register Layout

| Offset | Name |
| - | - |
| 0x2810 - 0x281B | Page Registers |
| 0x2820 - 0x2822 | Segment Registers 
| 0x282A | Blind Level Control |
| 0x2830 | Fade Effect Control |
| 0x2836 - 0x2837 | IRQ Position |
| 0x283C - 0x283D| TV Control |
| 0x2842  | Sprite Control |
| 0x2862 | Video IRQ Enable | 
| 0x2863 | Video IQQ Acknowledge |
| 0x2870 - 0x2872 | Sprite DMA |

### Page Registers

These registers use the base address of 0x2810 for Page 1 and 0x2816 for Page 2

#### 0x00 - X-Scroll 

Applies a horizontal offset to the plane, for screen-scrolling

#### 0x01 - Y-Scroll 

Applies a vertical offset to the plane, for screen-scrolling

#### 0x02 - Attributes

| Bits | Function | Description | 
| - | - | - |
| 0-1 | Bits Per Pixel | 0 = 2bpp, 1 = 4bpp, 2 = 6bpp, 3 = 8bpp | 
| 2 | X-Flip | 0 = Not Mirrored, 1 = Mirrored Horizontally |
| 3 | Y-Flip | 0 = Not Mirrored, 1 = Mirror Vertically | 
| 4-5 | Width | 0 = 0, 1 = 256, 2 = 512, 3 = 768 |
| 6-7 | Height | 0 = 0, 1 = 256, 2 = 512, 3 = 768 |
| 8-11 | Palette | ? |
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
| 0-12 | Tile Data Address | Memory Address of Tile Data |

#### 0x05 - Extended Attribute Address

| Bits | Function | Description | 
| - | - | - |
| 0-12 | Attribute Data Address | Memory Address of Attribute Data |





