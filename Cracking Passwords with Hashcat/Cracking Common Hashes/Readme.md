# Cracking Common Hashes with Hashcat

This document provides an overview of common hash types, examples of cracking them using Hashcat, and scenarios where they are frequently encountered during penetration tests.

---

## Table of Contents
1. [Introduction](#introduction)
2. [Common Hash Types](#common-hash-types)
3. [Examples of Cracking Hashes](#examples-of-cracking-hashes)
   - [Example 1: Database Dumps](#example-1-database-dumps)
   - [Example 2: Linux Shadow File](#example-2-linux-shadow-file)
   - [Example 3: Active Directory Hashes](#example-3-active-directory-hashes)
4. [Resources](#resources)

---

## Introduction

During penetration testing engagements, we often encounter various types of password hashes. These hashes can be cracked offline to reveal cleartext passwords, which can then be used for further network access, privilege escalation, or lateral movement. 

**Hashcat** is a versatile tool that supports a wide range of hash types and cracking techniques.

---

## Common Hash Types

The table below lists some of the most commonly encountered hash types during assessments:

| **Hash Mode** | **Hash Name**                       | **Example Hash**                                                                                  |
|---------------|-------------------------------------|--------------------------------------------------------------------------------------------------|
| `0`           | MD5                                 | `8743b52063cd84097a65d1633f5c74f5`                                                               |
| `100`         | SHA1                                | `b89eaac7e61417341b710b727768294d0e6a277b`                                                       |
| `1000`        | NTLM                                | `b4b9b02e6f09a9bd760f388b67351e2b`                                                               |
| `1800`        | sha512crypt (SHA512 for Unix)       | `$6$52450745$k5ka2p8bFuSmoVT1tzOyyuaREkkKBcCNqoDKzYiJL9RaE8yMnPgh2XzzF0NDrUhgrcLwg78xs1w5pJiypEdFX/` |
| `3200`        | bcrypt                              | `$2a$05$LhayLxezLhK1LhWvKxCyLOj0j1u.Kj0jZ0pEmm134uzrQlFvQJLF6`                                   |
| `5600`        | NetNTLMv2                           | `admin::N46iSNekpT:08ca45b7d7ea58ee:88dcbe4446168966a153a0064958dac6:5c7830315c783031000000...` |

---

## Examples of Cracking Hashes

### Example 1: Database Dumps

**Scenario**: MD5 and SHA1 hashes are often found in database dumps from SQL injection attacks or breached password databases.

#### Steps:

1. Create SHA1 hashes for a list of passwords:
    ```bash
    for i in $(cat words); do echo -n $i | sha1sum | tr -d ' -'; done
    ```
    Example output:
    ```
    fa3c9ecfc251824df74026b4f40e4b373fd4fc46 (SHA1 hash for "winter!")
    ```

2. Use Hashcat to crack the hashes:
    ```bash
    hashcat -m 100 SHA1_hashes /usr/share/wordlists/rockyou.txt
    ```

---

### Example 2: Linux Shadow File

**Scenario**: The `/etc/shadow` file on Linux contains password hashes for user accounts.

#### Example Hash:
root:$6$tOA0cyybhb/Hr7DN$htr2vffCWiPGnyFOicJiXJVMbk1muPORR.eRGYfBYUnNPUjWABGPFiphjIjJC5xPfFUASIbVKDAHS3vTW1qU.1


#### Steps:

1. Recognize the structure of the hash:
   - `$6$` → Indicates SHA512crypt.
   - Next 16 characters → Salt.
   - Remaining part → Hash.

2. Crack the hash:
    ```bash
    hashcat -m 1800 nix_hash /usr/share/wordlists/rockyou.txt
    ```

---

### Example 3: Active Directory Hashes

#### NTLM

**Scenario**: NTLM hashes are retrieved during network enumeration or via tools like Mimikatz.

**Example Hash**:
7100a909c7ff05b266af3c42ec058c33


#### Steps:

1. Save the hash to a file:
    ```bash
    echo "7100a909c7ff05b266af3c42ec058c33" > ntlm_example
    ```

2. Crack the hash:
    ```bash
    hashcat -a 0 -m 1000 ntlm_example /usr/share/wordlists/rockyou.txt
    ```

3. Example output:
    ```
    7100a909c7ff05b266af3c42ec058c33:Password01
    ```

---

## Resources

- [Hashcat Official Website](https://hashcat.net)
- [Hashcat Example Hashes](https://hashcat.net/wiki/doku.php?id=example_hashes)
- [RockYou Wordlist](https://github.com/brannondorsey/naive-hashcat/releases)
