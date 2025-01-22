# Creating Custom Wordlists

Custom wordlists are essential for cracking hashes that cannot be solved using common dictionaries. This document outlines tools and techniques for creating targeted and efficient wordlists.

---

## Key Tools for Wordlist Creation

### Crunch
Generates wordlists based on defined parameters such as length, character sets, and patterns.

**Syntax:**
```bash
crunch <min_length> <max_length> <charset> -t <pattern> -o <output_file>
```

**Examples:**
1. Generate words with 4-8 characters:
   ```bash
   crunch 4 8 -o wordlist
   ```
2. Create passwords in the format `ILFREIGHT201XXXX`:
   ```bash
   crunch 17 17 -t ILFREIGHT201%@@@@ -o wordlist
   ```

---

### CUPP (Common User Password Profiler)
Creates wordlists based on personal details from OSINT or social engineering.

**Interactive Mode Example:**
```bash
python3 cupp.py -i
```
CUPP prompts for target details like:
- Name, birthdate, partner, pet names.
- Additional keywords, leet mode, and special characters.

**Example Output:**
Generates `roger.txt` with 2419 passwords.

---

### CeWL
Scrapes websites to generate wordlists based on page content.

**Syntax:**
```bash
cewl -d <spider_depth> -m <min_word_length> -w <output_wordlist> <url>
```

**Example:**
```bash
cewl -d 5 -m 8 -e http://inlanefreight.com/blog -w wordlist.txt
```
Extracts words (≥8 characters) and emails from the specified URL.

---

### Princeprocessor
Generates word permutations using the PRINCE algorithm for advanced password cracking.

**Syntax:**
```bash
./pp64.bin -o <output_file> < input_file
```

**Examples:**
1. Create words up to 25 characters long:
   ```bash
   ./pp64.bin --pw-max=25 -o wordlist.txt < words
   ```
2. Limit words to ≥3 elements:
   ```bash
   ./pp64.bin --elem-cnt-min=3 -o wordlist.txt < words
   ```

---

### Kwprocessor
Creates wordlists based on keyboard patterns (e.g., `qwerty`).

**Installation:**
```bash
git clone https://github.com/hashcat/kwprocessor
cd kwprocessor
make
```

**Example:**
```bash
kwp -s 1 basechars/full.base keymaps/en-us.keymap routes/2-to-10-max-3-direction-changes.route
```
Generates words based on specified patterns and keyboard layout.

---

### Hashcat Utilities
Hashcat stores cracked passwords in `hashcat.potfile`. Extracting this file can help generate new wordlists for future attacks.

**Example:**
```bash
cut -d: -f 2- ~/hashcat.potfile > new_wordlist.txt
```

---

## Best Practices
1. **Targeted Wordlists**:
   - Focus on formats and patterns relevant to the target (e.g., known password policies or naming conventions).
2. **Use Rules**:
   - Combine custom wordlists with rules to expand possibilities.
3. **Analyze Results**:
   - Reuse cracked passwords to refine future attacks.

Custom wordlists are critical for advanced password cracking. Tools like Crunch, CUPP, CeWL, Princeprocessor, and Kwprocessor enable tailored dictionary creation, significantly increasing success rates.
