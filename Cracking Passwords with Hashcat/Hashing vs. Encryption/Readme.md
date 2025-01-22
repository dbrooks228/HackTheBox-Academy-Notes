# Hashing vs. Encryption

This documentation outlines the key differences, use cases, and examples of hashing and encryption for secure data handling.

---

## Hashing

Hashing is a one-way process that converts plaintext into a unique fixed-length string (hash). Hashes cannot be reversed to obtain the original plaintext. Common uses include password storage and file integrity verification.

### Key Features:
- **One-way process**: Original data cannot be reconstructed.
- **Fixed-length output**: Independent of input size.
- **Examples**: MD5, SHA-256, BCrypt, Argon2.

### Protection Mechanisms:
- **Salting**: Adding random data to plaintext before hashing increases computation time and prevents rainbow table attacks.
- **Keyed Hashing**: Using a secret key with hash functions (e.g., HMAC) to ensure data integrity.

### Example:
```bash
# Hashing plaintext with MD5
echo -n "p@ssw0rd" | md5sum
0f359740bd1cda994f8b55330c86d845

# Hashing with a salt
echo -n "p@ssw0rd123456" | md5sum
f64c413ca36f5cfe643ddbec4f7d92d0
```

### Notable Algorithms:
- **SHA-512**: Fast but susceptible to rainbow table attacks.
- **BCrypt & Argon2**: Slow and resource-intensive to resist brute-forcing.

---

## Encryption

Encryption is a reversible process that converts plaintext into ciphertext, which can be decrypted using a key. It ensures data confidentiality during transmission or storage.

### Types of Encryption:
1. **Symmetric Encryption**: Uses the same key for encryption and decryption.
   - **Example**: XOR
   ```python
   from pwn import xor
   xor("p@ssw0rd", "secret")  # Encrypt
   b'\x03%\x10\x01\x12D\x01\x01'
   xor(b'\x03%\x10\x01\x12D\x01\x01', "secret")  # Decrypt
   b'p@ssw0rd'
   ```
   - Other examples: AES, DES, 3DES, Blowfish.

2. **Asymmetric Encryption**: Uses a public key for encryption and a private key for decryption.
   - **Examples**: RSA, ECDSA, Diffie-Hellman.
   - **Use case**: HTTPS (SSL/TLS) for secure web communication.

---

## Summary

### Hashing:
- One-way process.
- Ensures data integrity.
- Commonly used for password storage.
- Example: SHA-256, Argon2.

### Encryption:
- Reversible process.
- Ensures data confidentiality.
- Symmetric (single key) and asymmetric (public/private keys).
- Example: AES, RSA.

Use the appropriate method based on your security needs: hashing for integrity, encryption for confidentiality.
