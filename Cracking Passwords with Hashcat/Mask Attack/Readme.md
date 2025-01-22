# Mask Attack

This document provides a reference for using Hashcat's mask attack mode to generate password guesses based on known patterns.

---

## Overview

Mask attacks are effective when the format, structure, or length of a password is known. Masks use static characters, character ranges, and placeholders to generate guesses that fit specific patterns.

---

## Placeholders

| Placeholder | Description                      |
|-------------|----------------------------------|
| `?l`        | Lower-case ASCII letters (a-z)  |
| `?u`        | Upper-case ASCII letters (A-Z)  |
| `?d`        | Digits (0-9)                    |
| `?h`        | Hexadecimal (0-9, a-f)          |
| `?H`        | Hexadecimal (0-9, A-F)          |
| `?s`        | Special characters              |
| `?a`        | Any character (`?l?u?d?s`)      |
| `?b`        | All bytes (0x00-0xff)           |

Custom placeholders (`-1` to `-4`) allow user-defined character sets.

---

## Syntax

```bash
hashcat -a 3 -m <hash_type> <hash_file> <mask>
```

### Key Parameters:
- `-a 3`: Specifies mask attack mode.
- `-m`: Hash type (e.g., `0` for MD5).
- `<mask>`: Defines the pattern (e.g., `ILFREIGHT?l?l?l?l?l20?1?d`).

---

## Example Usage

### Create a Hash
```bash
echo -n 'ILFREIGHTabcxy2015' | md5sum | tr -d " -" > md5_mask_example_hash
```

### Crack the Hash with Mask
```bash
hashcat -a 3 -m 0 md5_mask_example_hash -1 01 'ILFREIGHT?l?l?l?l?l20?1?d'
```

### Output
```
Session..........: hashcat
Status...........: Cracked
Hash.Name........: MD5
Hash.Target......: d53ec4d0b37bbf565b1e09d64834e1ae
Recovered........: 1/1 (100.00%) Digests
Time.Estimated...: 43 seconds
Guess.Mask.......: ILFREIGHT?l?l?l?l?l20?1?d [18]
Speed.#1.........: 3756.3 kH/s
```

---

## Custom Placeholders

Use `-1` to `-4` for user-defined character sets:
```bash
hashcat -a 3 -m 0 hash_file -1 abc123 '?1?1?1?1'
```

---

## Advanced Options

- **Incremental Mode**: Use `--increment` to incrementally increase the mask length.
- **Max Length**: Limit incremental length with `--increment-max`.

Example:
```bash
hashcat -a 3 -m 0 hash_file '?a?a?a' --increment --increment-max 6
```

---

## Summary

1. Use mask attacks to target passwords with known patterns or structures.
2. Leverage placeholders and custom charsets for flexibility.
3. Enable incremental mode for dynamic length adjustments.
4. Optimize performance with tailored masks to reduce cracking time.

Mask attacks are a powerful way to efficiently crack structured passwords using specific patterns.
