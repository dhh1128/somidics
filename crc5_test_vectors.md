# CRC-5-USB Test Vectors for Somidics Specification

## Reference Implementation Verification

All implementations MUST produce identical results for these test vectors.

## Test Vectors

### Somidic Test Vectors (13-bit somid input)

| Somid (decimal) | Somid (binary)    | CRC-5 (binary) | CRC-5 (decimal) | Combined 18-bit | Quant   |
|-----------------|-------------------|----------------|-----------------|-----------------|---------|
| 0               | 0b0000000000000   | 0b10010        | 18              | 0b000000000000010010 | 000018  |
| 1               | 0b0000000000001   | 0b01011        | 11              | 0b000000000000101011 | 000043  |
| 100             | 0b0000001100100   | 0b11111        | 31              | 0b000000110010011111 | 003231  |
| 1000            | 0b0001111101000   | 0b11011        | 27              | 0b000111110100011011 | 032027  |
| 8191            | 0b1111111111111   | 0b11010        | 26              | 0b111111111111111010 | 262138  |

**Note:** The "Combined 18-bit" column shows (somid << 5) | CRC-5. The "Quant" is the decimal representation of this 18-bit value (before adding plane offset).

### USB Token Test Vectors (11-bit input, for reference)

These verify compatibility with USB 2.0 Specification:

| Address | Endpoint | Input (11-bit)  | CRC-5 (bit-reversed) | Description |
|---------|----------|-----------------|----------------------|-------------|
| 0x15    | 0xe      | 0b11100010101   | 10111                | SETUP token |
| 0x3a    | 0xa      | 0b10100111010   | 11100                | OUT token   |
| 0x70    | 0x4      | 0b01001110000   | 01110                | IN token    |
| 0x01    | 0x2      | 0b00100000001   | 00011                | Generic     |
| 0x01    | 0x1      | 0b00010000001   | 11010                | Generic     |
| 0x00    | 0x0      | 0b00000000000   | 01000                | Generic     |

**Note:** USB tokens use bit-reversed input and output. Somidics do not use bit reversal for the 13-bit somid input.

## Algorithm Verification

An implementation is correct if and only if it produces the exact CRC-5 values shown above for the given somid inputs.

## Reference

Based on:
- USB 2.0 Specification, Section 8.3.5
- "Cyclic Redundancy Checks in USB" (USB Implementers Forum)
- CRC-5-USB parameters: polynomial 0x05, width 5, init 0x1F, xorout 0x1F, refin/refout true
