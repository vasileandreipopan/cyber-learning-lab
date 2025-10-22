# Cryptography Glossary — Basic Terms Explained

## 🔐 Core Terms

### Algorithm
A step-by-step mathematical procedure used to secure or transform data.  
Examples: AES, RSA, SHA-256.

### Cipher
Another word for “encryption method.”  
A cipher transforms readable text (plaintext) into unreadable data (ciphertext).

### Key
A secret value used by an algorithm to encrypt or decrypt data.  
Without the right key, decryption should be practically impossible.

### Symmetric Encryption
Same key used for both encryption and decryption.  
Fast, used for large data. Example: **AES**.

### Asymmetric Encryption
Two keys: one public, one private.  
Used for secure communication and digital signatures. Example: **RSA**, **ECC**.

---

## ⚙️ Important Algorithms

### RSA (Rivest–Shamir–Adleman)
- A **public-key** system based on prime numbers and modular exponentiation.
- Security comes from how hard it is to factor large numbers.
- Used for encryption, signatures, and key exchange.

### ECC (Elliptic Curve Cryptography)
- Like RSA, but built on points of a mathematical curve instead of primes.
- Offers same security with smaller keys (faster, more efficient).

### AES (Advanced Encryption Standard)
- A **symmetric cipher** used worldwide (e.g., HTTPS, disk encryption).
- Works on fixed-size blocks (128 bits) with keys of 128, 192, or 256 bits.

### Hash Function
Transforms any input into a fixed-size “digest.”  
Used to verify integrity and store passwords. Example: **SHA-256**.

### Digital Signature
Proof that data came from a specific sender and wasn’t modified.  
Created with a private key, verified with a public key.

### Public Key Infrastructure (PKI)
System that manages digital certificates and trusted authorities (used in HTTPS).

---

## Math Concepts

| Term                      |                  Meaning                  |      Example      |
|---------------------------|-------------------------------------------|-------------------|
| **Modulus (mod)**         | “Remainder” operation                     | 13 mod 5 = 3      |
| **Modular Inverse**       | Undo multiplication under a modulus       | 3⁻¹ mod 5 = 2     |
| **GCD**                   | Greatest common divisor                   | gcd(8, 12) = 4    |
| **Prime Number**          | Only divisible by 1 and itself            | 2, 3, 5, 7, 11…   |
| **Coprime**               | Two numbers with gcd = 1                  | (9, 28)           |
| **XOR (Exclusive OR)**    | Bitwise operation used in stream ciphers  | 1⊕0=1, 1⊕1=0      |

---
