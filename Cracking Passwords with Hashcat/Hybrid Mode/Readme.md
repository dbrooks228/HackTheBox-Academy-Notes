# Hybrid Mode

This document provides a reference for using Hashcat's hybrid attack mode, which combines wordlists with masks to generate targeted password guesses.

---

## Overview

Hybrid mode enhances traditional attacks by combining:
1. A **wordlist** (dictionary of base words).
2. A **mask** (pattern defining additional characters to append or prepend).

### Attack Modes
- `-a 6`: Appends a mask to words in a wordlist.
- `-a 7`: Prepends a mask to words in a wordlist.

---

## Syntax

```bash
hashcat -a <attack_mode> -m <hash_type> <hash_file> <wordlist> <mask>
```

### Key Parameters:
- `-a 6`: Appends the mask to each word.
- `-a 7`: Prepends the mask to each word.
- `-m`: Hash type (e.g., `0` for MD5).
- `<hash_file>`: File containing the hash to crack.
- `<wordlist>`: Path to the base wordlist.
- `<mask>`: Mask defining the appended or prepended pattern.

---

## Examples

### Appending a Mask (Attack Mode `-a 6`)

#### Scenario
Cracking the hash for `football1$`:
1. Base word: `football` (from `rockyou.txt`).
2. Mask: `?d?s` (appends a digit and a special character).

#### Steps
1. Create the hash:
   ```bash
   echo -n 'football1$' | md5sum | tr -d " -" > hybrid_hash
   ```
2. Crack the hash:
   ```bash
   hashcat -a 6 -m 0 hybrid_hash /opt/useful/seclists/Passwords/Leaked-Databases/rockyou.txt '?d?s'
   ```

#### Output
```
Session..........: hashcat
Status...........: Cracked
Hash.Name........: MD5
Hash.Target......: f7a4a94ff3a722bf500d60805e16b604
Recovered........: 1/1 (100.00%) Digests
Candidates.#1....: football1$ -> password2=
```

---

### Prepending a Mask (Attack Mode `-a 7`)

#### Scenario
Cracking the hash for `2015football`:
1. Base word: `football` (from `rockyou.txt`).
2. Mask: `20?1?d` (prepends years like `2010`, `2011`).

#### Steps
1. Create the hash:
   ```bash
   echo -n '2015football' | md5sum | tr -d " -" > hybrid_hash_prefix
   ```
2. Crack the hash:
   ```bash
   hashcat -a 7 -m 0 hybrid_hash_prefix -1 01 '20?1?d' /opt/useful/seclists/Passwords/Leaked-Databases/rockyou.txt
   ```

#### Output
```
Session..........: hashcat
Status...........: Cracked
Hash.Name........: MD5
Hash.Target......: eac4fe196339e1b511278911cb77d453
Recovered........: 1/1 (100.00%) Digests
Candidates.#1....: 2015football -> 2008charlie
```

---

## Custom Masks

### Placeholders
| Placeholder | Description                      |
|-------------|----------------------------------|
| `?l`        | Lowercase letters (a-z)         |
| `?u`        | Uppercase letters (A-Z)         |
| `?d`        | Digits (0-9)                    |
| `?s`        | Special characters              |
| `?a`        | Any character (`?l?u?d?s`)      |

### Example
Using `-1` to define custom character sets:
```bash
hashcat -a 7 -m 0 hash_file -1 01 '20?1?d' /path/to/wordlist
```

---

## Tips for Hybrid Mode

1. **Use Specific Masks**:
   - Focus on known password patterns (e.g., appending `?d?s` for a digit and special character).
   - Combine masks with wordlists to increase efficiency.
   
2. **Monitor Progress**:
   - Press `s` during cracking to view the current status.

3. **Incremental Mode**:
   - Use `--increment` to adjust mask length dynamically.

4. **Smaller Wordlists for Complex Masks**:
   - For hashes like bcrypt or Argon2, reduce wordlist size to optimize performance.

---

## Summary

1. Hybrid mode combines wordlists and masks to crack passwords following specific patterns.
2. Use `-a 6` to append a mask and `-a 7` to prepend a mask.
3. Leverage custom character sets and placeholders for targeted attacks.
4. Monitor and adjust cracking strategies to maximize efficiency.

Hybrid mode provides powerful customization for targeted password attacks, allowing fine-tuned approaches based on known patterns and policies.
