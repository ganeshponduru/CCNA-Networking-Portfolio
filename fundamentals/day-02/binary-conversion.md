# Binary and Decimal Conversion for IPv4

I practise binary conversion because the bit values make IPv4 octets and subnet masks easier to understand.
Each IPv4 octet is eight bits. From left to right, the bit weights are 128, 64, 32, 16, 8, 4, 2, 1.

## Binary to decimal
Add the weights for each bit set to 1.
- 11000100 = 128 + 64 + 4 = 196
- 10111001 = 128 + 32 + 16 + 8 + 1 = 185

## Decimal to binary
For each weight from 128 down to 1, write 1 if the weight fits into the remaining value; otherwise write 0.

| Decimal | Binary (8 bits) | Check |
|---:|---|---|
| 196 | 11000100 | 128 + 64 + 4 |
| 124 | 01111100 | 64 + 32 + 16 + 8 + 4 |
| 75 | 01001011 | 64 + 8 + 2 + 1 |
| 255 | 11111111 | All weights sum to 255 |
| 0 | 00000000 | No weights are set |

Eight bits have 2^8 = 256 combinations, numbered 0 through 255, so an octet does not range from 1 to 256.

## Practice
1. Convert 10101010 to decimal.
2. Convert 200 to 8-bit binary.
3. Convert 00001111 to decimal.
4. Convert 172 to 8-bit binary.
5. Write 192.168.1.10 in binary, octet by octet.

Try these yourself before checking. These exercises are not marked as completed practical work.