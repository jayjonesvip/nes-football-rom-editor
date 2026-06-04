# Tecmo Bowl (U) (PRG0) NES ROM Layout

Source ROM: C:\Users\jjones\Downloads\Tecmo Bowl (U) (PRG0).nes
File size: 262,160 bytes
Container: iNES

## iNES Header

| Field | Value |
| --- | --- |
| Header bytes | 4E 45 53 1A 08 10 11 00 00 00 00 4E 49 31 2E 33 |
| Magic | NES<EOF> |
| PRG ROM banks | 8 x 16 KB = 128 KB |
| CHR ROM banks | 16 x 8 KB = 128 KB |
| Mapper | 1 (MMC1) |
| Mirroring | vertical |
| Battery-backed RAM flag | no |
| Trainer | no |
| Extra bytes after expected ROM | 0 |

## File Segments

| Segment | File offset range | Size | Notes |
| --- | ---: | ---: | --- |
| iNES header | 0x000000-0x00000F (16 bytes) | 16 bytes | ROM metadata |
| PRG ROM | 0x000010-0x02000F (131,072 bytes) | 128 KB | 6502 program/data, MMC1 bank-switched |
| CHR ROM | 0x020010-0x04000F (131,072 bytes) | 128 KB | Raw NES pattern/tile graphics |

## PRG ROM Banks

MMC1 commonly exposes one 16 KB switchable PRG window and one fixed 16 KB PRG window. The reset/NMI/IRQ vectors are at the end of the final PRG bank. Exact runtime bank mode is controlled by writes to MMC1 registers.

| Bank | File offset range | Size | CPU mapping note |
| --- | ---: | ---: | --- |
| PRG bank 00 | 0x000010-0x00400F | 16 KB | $8000-$BFFF switchable MMC1 bank candidate |
| PRG bank 01 | 0x004010-0x00800F | 16 KB | $8000-$BFFF switchable MMC1 bank candidate |
| PRG bank 02 | 0x008010-0x00C00F | 16 KB | $8000-$BFFF switchable MMC1 bank candidate |
| PRG bank 03 | 0x00C010-0x01000F | 16 KB | $8000-$BFFF switchable MMC1 bank candidate |
| PRG bank 04 | 0x010010-0x01400F | 16 KB | $8000-$BFFF switchable MMC1 bank candidate |
| PRG bank 05 | 0x014010-0x01800F | 16 KB | $8000-$BFFF switchable MMC1 bank candidate |
| PRG bank 06 | 0x018010-0x01C00F | 16 KB | $8000-$BFFF switchable MMC1 bank candidate |
| PRG bank 07 | 0x01C010-0x02000F | 16 KB | $C000-$FFFF fixed reset/vector bank |

## 6502 Vectors

| Vector | Address | File bytes |
| --- | ---: | --- |
| NMI | $C060 | 60 C0 |
| Reset | $C000 | 00 C0 |
| IRQ/BRK | $C3A0 | A0 C3 |

## CHR ROM Banks

Each 8 KB CHR bank contains 512 8x8 pixel tiles. NES CHR stores two bitplanes per tile, giving palette indices 0-3; the actual in-game colors come from PPU palette RAM at runtime and are not stored directly in CHR ROM.

| Bank | File offset range | Size | Graphics content |
| --- | ---: | ---: | --- |
| CHR bank 00 | 0x020010-0x02200F | 8 KB | 512 raw 8x8 2bpp tiles |
| CHR bank 01 | 0x022010-0x02400F | 8 KB | 512 raw 8x8 2bpp tiles |
| CHR bank 02 | 0x024010-0x02600F | 8 KB | 512 raw 8x8 2bpp tiles |
| CHR bank 03 | 0x026010-0x02800F | 8 KB | 512 raw 8x8 2bpp tiles |
| CHR bank 04 | 0x028010-0x02A00F | 8 KB | 512 raw 8x8 2bpp tiles |
| CHR bank 05 | 0x02A010-0x02C00F | 8 KB | 512 raw 8x8 2bpp tiles |
| CHR bank 06 | 0x02C010-0x02E00F | 8 KB | 512 raw 8x8 2bpp tiles |
| CHR bank 07 | 0x02E010-0x03000F | 8 KB | 512 raw 8x8 2bpp tiles |
| CHR bank 08 | 0x030010-0x03200F | 8 KB | 512 raw 8x8 2bpp tiles |
| CHR bank 09 | 0x032010-0x03400F | 8 KB | 512 raw 8x8 2bpp tiles |
| CHR bank 10 | 0x034010-0x03600F | 8 KB | 512 raw 8x8 2bpp tiles |
| CHR bank 11 | 0x036010-0x03800F | 8 KB | 512 raw 8x8 2bpp tiles |
| CHR bank 12 | 0x038010-0x03A00F | 8 KB | 512 raw 8x8 2bpp tiles |
| CHR bank 13 | 0x03A010-0x03C00F | 8 KB | 512 raw 8x8 2bpp tiles |
| CHR bank 14 | 0x03C010-0x03E00F | 8 KB | 512 raw 8x8 2bpp tiles |
| CHR bank 15 | 0x03E010-0x04000F | 8 KB | 512 raw 8x8 2bpp tiles |

## Generated Graphics Sheets

- All-bank preview: tecmo_chr_all_banks_preview.png
- tecmo_chr_bank_00.png
- tecmo_chr_bank_01.png
- tecmo_chr_bank_02.png
- tecmo_chr_bank_03.png
- tecmo_chr_bank_04.png
- tecmo_chr_bank_05.png
- tecmo_chr_bank_06.png
- tecmo_chr_bank_07.png
- tecmo_chr_bank_08.png
- tecmo_chr_bank_09.png
- tecmo_chr_bank_10.png
- tecmo_chr_bank_11.png
- tecmo_chr_bank_12.png
- tecmo_chr_bank_13.png
- tecmo_chr_bank_14.png
- tecmo_chr_bank_15.png

## Notes For Deeper Hacking

- PRG is 6502 code plus data tables. A disassembler should treat this as mapper 1/MMC1 with 16 KB PRG banks.
- CHR is already ROM-backed graphics, so the raw tile sheets are direct extractions rather than screenshots.
- Tilemaps, sprite composition, team/player data, and actual palettes live in PRG data and PPU runtime state, so mapping those needs a second pass through code/data tables or emulator traces.