# Combination Attack

This document serves as a reference for using Hashcat’s combination attack mode, which creates combinations of words from two wordlists to crack passwords.

---

## Overview

A combination attack joins two wordlists to create password guesses like `welcomehome` or `hotelcalifornia`. This method is effective when users create passwords by combining multiple simple words.

---

## Syntax

```bash
hashcat -a 1 -m <hash_type> <hash_file> <wordlist1> <wordlist2>
```

### Parameters
- `-a 1`: Combination attack mode.
- `-m`: Hash type (e.g., `0` for MD5).
- `<hash_file>`: File containing the hash to crack.
- `<wordlist1> <wordlist2>`: Input wordlists for the combination.

---

## Example Usage

### Preparing Wordlists
Create two wordlists:
```bash
# wordlist1
super
world
secret

# wordlist2
hello
password
```

### Generating Combinations
Use Hashcat’s `--stdout` to see generated combinations:
```bash
hashcat -a 1 --stdout wordlist1 wordlist2
```
Output:
```
superhello
superpassword
worldhello
worldpassword
secrethello
secretpassword
```

### Cracking MD5 Hash
Generate the hash for `secretpassword`:
```bash
echo -n 'secretpassword' | md5sum | cut -f1 -d' ' > combination_md5
```

Run Hashcat with the combination attack:
```bash
hashcat -a 1 -m 0 combination_md5 wordlist1 wordlist2
```
Output:
```
Session..........: hashcat
Status...........: Cracked
Hash.Name........: MD5
Hash.Target......: 2034f6e32958647fdff75d265b455ebf
Recovered........: 1/1 (100.00%) Digests
```

---

## Additional Notes

- **Efficiency**: Combination attacks are effective for exploring combined word possibilities, but the workload may need to be increased for larger hashes or faster hardware.
- **Supplementary Wordlists**:
  ```bash
  # wordlist1
  sunshine
  happy
  frozen
  golden

  # wordlist2
  hello
  joy
  secret
  apple
  ```
- **Tool for Debugging**: Use the `--stdout` option to preview combinations without executing the cracking process.

---

## Summary

1. Combination attacks test combined wordlists to crack passwords with joined words.
2. Use `-a 1` to specify combination mode.
3. Preview generated combinations with `--stdout` for debugging.
4. Leverage small, targeted wordlists to optimize cracking speed and success.

Combination attacks demonstrate that joining two words does not necessarily create a stronger password.
