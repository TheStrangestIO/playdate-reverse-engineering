A file with the `.pdz` extension represents an executable file that has been compiled by `pdc`. This format uses little endian byte order.

## Header

| Offset | Type     | Detail |
|:-------|:---------|:-------|
| `0`    | `chr[12]` | "Playdate PDZ" |
| `12`   | `int32` | Unknown, seen as `0x00000000`, maybe just reserved? | |

## File Entries

Following the header is a list of file entries. These can represent either Lua bytecode, compiled C binaries(?), or extra data and standard library image assets such as the crank prompt.

Each entry has the following:

| Type    | Detail |
|:--------|:-------|
| `uint8`  | Bitflags |
| `uint24` | Compressed data length + 4 |
| `string` | Filename as null-terminated string |
| `-` | Optional null-padding if needed to align to the next multiple of 4 bytes |
| `uint32` | Decompressed data length |
| `data` | Zlib-compressed file data |

### Bitflags

I haven't seen enough samples to know what these are for certain:

| Flag | Detail |
|:-------|:-------|
| `(flags >> 7) & 0x1` | Is file compressed |
| `(flags >> 1) & 0x1` | Is file non-executable? |

## Lua bytecode

Playdate (at the time of writing) seems to use the prerelease version of Lua 5.4. This version uses a slightly nonstandard bytecode header structure, before it was reverted for the 5.4 release.

### Header

| Offset | Type    | Detail |
|:-------|:--------|:-------|
| `0`    | `byte[4]` | Constant `LUA_SIGNATURE` (hex `1B 4C 75 61`) |
| `4`    | `uint16`  | Version (`0x03F8` = 5.4.0 prerelease) |
| `6`    | `byte`    | Constant `LUAC_FORMAT` (hex `00`) |
| `7`    | `byte[6]` | Constant `LUAC_DATA` (hex `19 93 0D 0A 1A 0A`) |
| `13`   | `uint8`   | Instruction size (always `4`) |
| `14`   | `uint8`   | Integer size (always `4`) |
| `15`   | `uint8`   | Number size (always `4`) |
| `16`   | `lua int`  | Constant `LUA_INT` (`0x5678`) |
| `20`   | `lua float`  | Constant `LUA_NUM` (`370.5`) |

## Image assets

TODO