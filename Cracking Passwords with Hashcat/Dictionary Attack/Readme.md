# Cracking Passwords with Hashcat

This document serves as a reference for using Hashcat to perform dictionary attacks, a straightforward and effective method for cracking password hashes.

---

## Dictionary Attack Overview

A dictionary attack uses a wordlist to test possible passwords against a given hash. This method is most effective against weak passwords and is faster compared to more complex attack types.

---

## Syntax

```bash
hashcat -a 0 -m <hash_type> <hash_file> <wordlist>
```

### Key Parameters
- `-a 0`: Specifies a dictionary attack (straight mode).
- `-m`: Specifies the hash type (e.g., `1400` for SHA-256).
- `<hash_file>`: File containing the target hash.
- `<wordlist>`: Path to the wordlist file (e.g., `rockyou.txt`).

---

## Example Usage

### Cracking a SHA-256 Hash
```bash
# Generate SHA-256 hash for "!academy"
echo -n '!academy' | sha256sum | cut -f1 -d' ' > sha256_hash_example

# Use Hashcat to crack the hash
hashcat -a 0 -m 1400 sha256_hash_example /opt/useful/seclists/Passwords/Leaked-Databases/rockyou.txt
```

### Output
```
Session..........: hashcat
Status...........: Cracked
Hash.Name........: SHA2-256
Hash.Target......: 006fc3a9613f3edd9f97f8e8a8eff3b899a2d89e1aabf33d7cc...8b0fe6
Recovered........: 1/1 (100.00%) Digests
Guess.Base.......: File (/opt/useful/seclists/Passwords/Leaked-Databases/rockyou.txt)
Time.Estimated...: < 4 seconds
```

---

## Performance Considerations

### Bcrypt Example
For a bcrypt hash like `$2a$05$ZdEkj8cup/JycBRn2CX.B.nIceCYR8GbPbCCg6RlD7uvuREexEbVy`:
- Uses a Blowfish cipher with 5 rounds.
- Cracking time increases exponentially with more rounds.
- Rockyou.txt wordlist on standard hardware may take ~1.5 hours.

```bash
hashcat -a 0 -m 3200 bcrypt_hash_file /path/to/rockyou.txt
```

### Status Check During Cracking
Press `s` during cracking to display progress:
```
Session..........: hashcat
Status...........: Running
Hash.Name........: bcrypt $2*$, Blowfish (Unix)
Time.Estimated...: 1 hour, 33 minutes
Speed.#1.........: 2470 H/s
Progress.........: 468576/14344385
```

---

## Wordlists

Common sources:
- **rockyou.txt**: Included in most pentesting distros.
- **SecLists**: Extensive repository of password lists.
- **CrackStation Dictionary**: Large dictionary with 1.5 billion entries (15GB).

---

## Summary

1. Use dictionary attacks for quick, effective password cracking against weak passwords.
2. Adjust hash mode (`-m`) and attack type (`-a`) to match the target hash.
3. Monitor cracking progress using status commands.
4. Choose wordlists based on hardware and hash complexity.

Dictionary attacks are efficient but depend heavily on the strength of the target hash and password complexity. Advanced hardware significantly reduces cracking time for complex hashes.
