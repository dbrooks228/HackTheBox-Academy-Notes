# Cracking Miscellaneous Files & Hashes

This guide provides comprehensive instructions on cracking password-protected files and hashes encountered during penetration tests and security assessments. The document highlights various tools, techniques, and examples to crack password-protected documents such as Microsoft Office files, zip archives, KeePass databases, and PDFs using Hashcat.

---

## Tools

A variety of tools can be used to extract password hashes from files and convert them into formats compatible with **Hashcat**:

1. **JohnTheRipper**: Provides C-based tools to extract hashes. Available [here](https://github.com/magnumripper/JohnTheRipper).
   - **Installation**:
     ```bash
     sudo git clone https://github.com/magnumripper/JohnTheRipper.git
     cd JohnTheRipper/src
     sudo ./configure && sudo make
     ```

2. **Python Ports**: Easier to work with and available in the JohnTheRipper jumbo GitHub repo. 
   - Examples include `office2john.py` and `keepass2john.py`.

---

## Examples of Cracking File Formats

### Example 1: Cracking Password-Protected Microsoft Office Documents

**Tools Used**: `office2john.py` to extract hashes.  
Hashcat supports the following modes:
- **9400**: MS Office 2007
- **9500**: MS Office 2010
- **9600**: MS Office 2013

**Steps**:
1. Extract the hash:
    ```bash
    python office2john.py hashcat_Word_example.docx
    ```
    Example output:
    ```
    $office$*2013*100000*256*16*6e059661c3ed733f5730eaabb41da13a*...
    ```

2. Crack the hash with Hashcat:
    ```bash
    hashcat -m 9600 office_hash /opt/useful/seclists/Passwords/Leaked-Databases/rockyou.txt
    ```

---

### Example 2: Cracking Password-Protected Zip Files

**Tools Used**: `zip2john` to extract hashes.  
Hashcat supports the following modes for compressed files:
- **11600**: 7-Zip
- **13600**: WinZip
- **17200**: PKZIP (Compressed)

**Steps**:
1. Create a password-protected zip file:
    ```bash
    zip --password zippyzippy blueprints.zip dummy.pdf
    ```

2. Extract the hash:
    ```bash
    zip2john blueprints.zip
    ```

3. Crack the hash:
    ```bash
    hashcat -a 0 -m 17200 zip_hash /opt/useful/seclists/Passwords/Leaked-Databases/rockyou.txt
    ```

---

### Example 3: Cracking KeePass Files

**Tools Used**: `keepass2john.py` to extract hashes.  
Hashcat supports KeePass hashes using mode **13400**.

**Steps**:
1. Extract the hash:
    ```bash
    python keepass2john.py Master.kdbx
    ```

2. Crack the hash with Hashcat:
    ```bash
    hashcat -a 0 -m 13400 keepass_hash /opt/useful/seclists/Passwords/Leaked-Databases/rockyou.txt
    ```

---

### Example 4: Cracking Password-Protected PDF Files

**Tools Used**: `pdf2john.py` to extract hashes.  
Hashcat supports the following modes:
- **10400**: PDF 1.1 - 1.3 (Acrobat 2 - 4)
- **10500**: PDF 1.4 - 1.6 (Acrobat 5 - 8)

**Steps**:
1. Extract the hash:
    ```bash
    python pdf2john.py inventory.pdf | awk -F":" '{ print $2 }'
    ```

2. Crack the hash with Hashcat:
    ```bash
    hashcat -a 0 -m 10500 pdf_hash /opt/useful/seclists/Passwords/Leaked-Databases/rockyou.txt
    ```

---

## Key Takeaways

- **Speed**: Hashcat is faster on GPUs compared to CPUs, especially for computationally heavy hash types like KeePass and PDF.
- **Weak Passwords**: Many users choose weak passwords, making password-protected files susceptible to dictionary attacks with wordlists like `rockyou.txt`.

---

## Connect to Pwnbox

Pwnbox provides a Parrot Linux instance to execute these techniques. Visit your lab environment and follow the step-by-step instructions for practical applications.

```bash
# Example: Extracting a 7-Zip file hash
7z2john example.7z > hashfile
hashcat -a 0 -m 11600 hashfile rockyou.txt
```
## Additional Questions

1. **Task**: Extract the hash from the attached 7-Zip file, crack the hash, and submit the value of the `flag.txt` file inside the archive.
   - Follow the steps provided in **Example 2** above.

---

Stay sharp, and remember: Always abide by ethical and legal standards in penetration testing engagements.
