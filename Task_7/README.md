# Task 7 – Password Security & Cryptography Concepts

This document covers symmetric vs asymmetric encryption, hashing algorithms, salting, password cracking, SSL/TLS, and a password security best practices guide for CoreTech Innovation.  

---

## 1. Symmetric vs Asymmetric Encryption

### Symmetric Encryption
- Uses one **shared secret key** for both encryption and decryption.
- Fast; ideal for bulk data encryption.
- Examples: AES, DES, ChaCha20.
- Used in: disk encryption, VPN tunnels.

Conceptual example (Python – pycryptodome):

    from Crypto.Cipher import AES
    cipher = AES.new(key, AES.MODE_EAX)
    ciphertext, tag = cipher.encrypt_and_digest(data)

### Asymmetric Encryption
- Uses a **key pair**: public key (shared) and private key (kept secret).
- Slower; used for key exchange and digital signatures.
- Examples: RSA, ECC, Ed25519.
- Used in: HTTPS handshake, email signing.

Conceptual example (Python):

    from Crypto.PublicKey import RSA
    key = RSA.generate(2048)
    public_key = key.publickey().export_key()
    private_key = key.export_key()

| Feature          | Symmetric                | Asymmetric                  |
|------------------|--------------------------|-----------------------------|
| Keys             | One shared key           | Key pair (public + private) |
| Speed            | Fast                     | Slow                        |
| Use case         | Bulk encryption          | Key exchange, signatures    |
| Key distribution | Difficult                | Easier (public key is open) |

---

## 2. Hashing Algorithms (MD5, SHA-1, SHA-256)

A hash is a one-way function converting input into a fixed-size digest.

| Algorithm | Digest size | Status  |
|-----------|-------------|---------|
| MD5       | 128-bit     | Broken (collisions, fast) |
| SHA-1     | 160-bit     | Broken (SHAttered 2017)   |
| SHA-256   | 256-bit     | Currently secure          |

### Why MD5 is Insecure
- **Collision attacks** – two different inputs can produce the same hash.
- **Speed** – extremely fast on GPUs/ASICs; billions of guesses per second.
- **No salt** – makes rainbow table attacks trivial.

Hashes of the word "password":

    MD5:    5f4dcc3b5aa765d61d8327deb882cf99
    SHA-1:  5baa61e4c9b93f3f0682250b6cf8331b7ee68fd8
    SHA-256:5e884898da28047151d0e56f8dc6292773603d0d6aabbdd62a11ef721d1542d8

**Modern secure alternatives:** bcrypt, scrypt, Argon2id (slow, salted, memory-hard).

---

## 3. Password Hashing in Python (hashlib)

This script demonstrates **insecure MD5** vs **salted SHA-256**.

    import hashlib
    import os

    def hash_password_md5_unsafe(password: str) -> str:
        return hashlib.md5(password.encode()).hexdigest()

    def hash_password_salted_sha256(password: str, salt: bytes = None) -> tuple:
        if salt is None:
            salt = os.urandom(16)
        salted = salt + password.encode()
        hash_val = hashlib.sha256(salted).hexdigest()
        return salt.hex(), hash_val

    def verify_password_salted(password: str, salt_hex: str, stored_hash: str) -> bool:
        salt = bytes.fromhex(salt_hex)
        _, computed_hash = hash_password_salted_sha256(password, salt)
        return computed_hash == stored_hash

    if __name__ == "__main__":
        pwd = "SecurePass123"
        print("Insecure MD5:", hash_password_md5_unsafe(pwd))
        salt_hex, sha_hash = hash_password_salted_sha256(pwd)
        print("Salt (hex):", salt_hex)
        print("Salted SHA-256:", sha_hash)
        print("Verify correct:", verify_password_salted(pwd, salt_hex, sha_hash))
        print("Verify wrong  :", verify_password_salted("Wrong", salt_hex, sha_hash))

<img width="1252" height="783" alt="image" src="https://github.com/user-attachments/assets/1a23dacf-fa37-40ef-8f18-8a02fa069b29" />
<img width="945" height="134" alt="image" src="https://github.com/user-attachments/assets/6b226d2f-5f91-4e45-a7c2-41fdf0bf9f87" />


---

## 4. Salting and Its Importance

- A **salt** is a random value added to the password before hashing.
- It ensures identical passwords produce different hashes.
- It defeats **rainbow tables** (precomputed hash databases).
- Attackers must crack each password individually.

**Best practice:** Use a unique, cryptographically random salt (≥ 16 bytes) for every user.

---

## 5. Password Cracking Lab (Hashcat / John the Ripper)

### Sample hashes (file: `target_hashes.txt`)

    5f4dcc3b5aa765d61d8327deb882cf99   # password
    e10adc3949ba59abbe56e057f20f883e   # 123456
    1c8bfe8f801d79745c4631d09fff36c82aa37fc4cce4fc946683d7b336b63032   # letmein (SHA-256)

### Crack with John the Ripper

    john --format=raw-md5 --wordlist=/usr/share/wordlists/rockyou.txt md5_hashes.txt
    john --show md5_hashes.txt
<img width="812" height="473" alt="image" src="https://github.com/user-attachments/assets/6097b351-b722-48d4-8cf9-2908ce0a846d" />



### Crack with Hashcat

    hashcat -m 0 -a 0 md5_hashes.txt /usr/share/wordlists/rockyou.txt
    hashcat -m 1400 -a 0 sha256_hashes.txt /usr/share/wordlists/rockyou.txt
<img width="704" height="383" alt="image" src="https://github.com/user-attachments/assets/c3d9f549-9200-4fa0-b63f-5116b545e227" />



> ⚠️ Only use on your own test hashes in an isolated lab.

---

## 6. SSL/TLS and How Certificates Work

### SSL/TLS
- Provides **encryption, authentication, and integrity** for internet communications (HTTPS).
- **TLS 1.3 Handshake (simplified):**
  1. Client/Server exchange hello messages and random numbers.
  2. Server sends its **certificate** (contains public key).
  3. Client validates the certificate chain (Root CA → Intermediate → Server).
  4. Key exchange (ECDHE) derives symmetric session keys.
  5. Encrypted session begins.

### Digital Certificates (X.509)
- Bind a domain name to a public key.
- Issued and digitally signed by a **Certificate Authority (CA)**.
- Trust chain: Root CA → Intermediate CA → Server certificate.
- Browsers/OS trust root CAs; they verify signatures all the way up.

<img width="798" height="857" alt="image" src="https://github.com/user-attachments/assets/33ae11c2-4810-4b7e-8652-af04d6cbac64" />


---

## 7. Password Security Best Practices Guide – CoreTech Innovation

### Storage (Developers)
- **Never** store plaintext passwords.
- Use **slow, memory-hard algorithms**: Argon2id (1st choice), bcrypt (cost ≥ 12), scrypt.
- Add a **unique salt** (≥ 16 bytes) per password.
- Consider a **pepper** (secret outside the database).

### Policies (End‑Users)
- Minimum length: **12 characters** (16+ for privileged accounts).
- Encourage **passphrases**.
- Check passwords against **breach databases** (e.g., Have I Been Pwned API).
- No forced periodic changes unless compromise is suspected.

### Multi‑Factor Authentication (MFA)
- **Mandatory** for remote access, admins, and privileged accounts.
- Use TOTP (app‑based) or FIDO2 security keys.
- Avoid SMS where possible.

### Secure Transmission
- Enforce **HTTPS** with valid TLS certificates on all login pages.
- Never email passwords; use time‑limited, single‑use reset links.

### Monitoring & Response
- Implement **rate limiting** and **account lockout**.
- Log authentication events without passwords.
- Have an incident response plan for breaches.

### Education
- Train employees on phishing, password reuse, and password managers.
- Provide an approved password manager (e.g., Bitwarden, 1Password).
