# Working with Rules in Hashcat

The rule-based attack is one of the most advanced password-cracking techniques in Hashcat. Rules enable you to transform base wordlists by applying modifications such as prefixing, suffixing, character replacement, and case toggling. This method increases cracking success rates without requiring larger wordlists.

---

## Key Functions for Rules

| Function | Description                           | Example Input           | Example Output           |
|----------|---------------------------------------|-------------------------|--------------------------|
| `l`      | Convert all to lowercase             | `InlaneFreight2020`     | `inlanefreight2020`      |
| `u`      | Convert all to uppercase             | `InlaneFreight2020`     | `INLANEFREIGHT2020`      |
| `c`      | Capitalize first letter              | `inlaneFreight2020`     | `Inlanefreight2020`      |
| `t`      | Toggle case                          | `InlaneFreight2020`     | `iNLANEfREIGHT2020`      |
| `^X`     | Prepend character `X`                | `InlaneFreight2020`     | `!InlaneFreight2020`     |
| `$X`     | Append character `X`                 | `InlaneFreight2020`     | `InlaneFreight2020!`     |
| `r`      | Reverse word                         | `InlaneFreight2020`     | `0202thgierFenalnI`      |

A full list of functions can be found in Hashcat's [documentation](https://hashcat.net/wiki/).

---

## Reject Rules
Reject rules exclude words from processing based on their length:
- Words shorter than `N`: `>N`
- Words longer than `N`: `<N`

---

## Example: Creating and Applying a Rule

### Step 1: Create a Custom Rule
This rule capitalizes the first letter, substitutes characters for leetspeak, and appends `2020`:
```bash
echo 'c so0 si1 se3 ss5 sa@ $2 $0 $2 $0' > rule.txt
```

### Step 2: Prepare a Wordlist
Store a sample password in `test.txt`:
```bash
echo 'password_ilfreight' > test.txt
```

### Step 3: Debug the Rule
Use the `--stdout` flag to view the transformations applied by the rule:
```bash
hashcat -r rule.txt test.txt --stdout
```

**Output Example:**
```
P@55w0rd_1lfr31ght2020
```

---

## Cracking Passwords with Rules

### Example Scenario
Crack the SHA1 hash `08004e35561328e357e34d07c53c7e4f41944e28` using a custom rule.

#### Step 1: Generate the Hash
```bash
echo -n 'St@r5h1p2020' | sha1sum | awk '{print $1}' > hash
```

#### Step 2: Run Hashcat
Use the custom rule with a wordlist to crack the hash:
```bash
hashcat -a 0 -m 100 hash /opt/useful/seclists/Passwords/Leaked-Databases/rockyou.txt -r rule.txt
```

**Output Example:**
```
08004e35561328e357e34d07c53c7e4f41944e28:St@r5h1p2020
```

---

## Default and Publicly Available Rules

### Hashcat Default Rules
Hashcat comes with pre-installed rules located in the `/usr/share/hashcat/rules/` directory. Examples include:
- `best64.rule`
- `rockyou-30000.rule`
- `d3ad0ne.rule`

### Random Rule Generation
Generate and apply random rules dynamically:
```bash
hashcat -a 0 -m 100 -g 1000 hash /opt/useful/seclists/Passwords/Leaked-Databases/rockyou.txt
```

### Popular Public Rules
- **NSA Rules**: Tailored for corporate policies.
- **Hob0Rules**: Focused on general password patterns.
- **Corporate.rule**: Designed for enterprise-level engagements.

---

## Best Practices for Rule-Based Attacks
1. **Start Small**: Use targeted wordlists and rules before scaling up.
2. **Debug Rules**: Validate transformations with `--stdout`.
3. **Leverage Defaults**: Begin with pre-installed rules for quick results.
4. **Optimize Resources**: Match wordlists and rules to hardware capabilities.

Rule-based attacks are powerful tools for cracking complex passwords efficiently. Mastering rule creation and usage enables effective password cracking while saving time and computational resources.
