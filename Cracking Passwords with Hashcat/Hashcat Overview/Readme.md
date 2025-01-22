# Hashcat Overview

This document provides a reference for using Hashcat, a powerful open-source password-cracking tool.

---

## Installation

Install Hashcat directly or via source:
```bash
sudo apt install hashcat
```

---

## Usage

Hashcat supports various options for cracking passwords:
```bash
hashcat [options] hash|hashfile [dictionary|mask|directory]
```

### Key Options:
- `-m`: Hash type (e.g., `-m 0` for MD5).
- `-a`: Attack mode (e.g., `-a 3` for brute force).
- `-b`: Benchmark hash performance.
- `-h`: Display help.

---

## Supported Attack Modes
| Mode | Description               |
|------|---------------------------|
| 0    | Straight                 |
| 1    | Combination              |
| 3    | Brute-force              |
| 6    | Hybrid Wordlist + Mask   |
| 7    | Hybrid Mask + Wordlist   |

---

## Example Hashes

View example hashes with:
```bash
hashcat --example-hashes
```

Example output:
```
MODE: 0
TYPE: MD5
HASH: 8743b52063cd84097a65d1633f5c74f5
PASS: hashcat
```

---

## Benchmarking

Run benchmarks to test hash performance:
```bash
hashcat -b -m 0
```

Example output:
```
Hashmode: 0 - MD5
Speed.#1.........: 449.4 MH/s
```

---

## Optimizations

1. **Optimized Kernels (`-O`)**:
   - Increases speed by limiting password length (default: 32 characters).

2. **Workload Profiles (`-w`)**:
   - `1`: Low usage (work alongside other tasks).
   - `3`: High usage (dedicated Hashcat session).

---

## Best Practices

- Avoid using `--force` unless necessary, as it bypasses safety checks and may produce false results.
- Reference the [Hashcat GitHub](https://github.com/hashcat/hashcat) for updates and supported hash types.

---

## Summary

1. Install Hashcat and use `-h` to explore available options.
2. Choose appropriate `-m` and `-a` values for your hash and attack type.
3. Optimize cracking speed with `-O` and `-w` as needed.
4. Use benchmarks to evaluate performance for specific hash types.

Hashcat provides a robust toolkit for cracking over 320 hash types with flexible optimization options and reliable community support.
