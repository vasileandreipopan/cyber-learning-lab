# RSA — Complete Theory

## Contents

1. Introduction 
2. How RSA works (Conceptual Overview)
3. Mathematical Foundations
4. Key Generation Explained
5. Encryption Process
6. RSA in Real-World Security
7. Common Vulnerabilities and Attacks
8. Glossary of Terms

---

## Introduction 

RSA (Rivest–Shamir–Adleman) is one of the most widely used **public-key cryptosystems** in modern cryptography.  
It provides **confidentiality (via encryption), and integrity/authentication (via digital signatures)** depending on how it's used.

Unlike symmetric encryption (where the same key is used for both encryption and decryption), RSA is **asymmetric**, it uses a **public key** for encryption and a **private key** for decryption.

RSA is essential for:
- Secure data transmission (e.g., HTTPS, email encryption)
- Digital signatures and authentication
- Cryptographic challenges and CTFs
- Understanding modern security protocols and PKI (Public Key Infrastructure)

In this document, you’ll explore:
- The mathematics and logic behind RSA  
- How keys are generated, used, and attacked  
- Common vulnerabilities and exploitation techniques   

---

## How RSA Works (Conceptual Overview)

RSA is an **asymmetric cryptographic algorithm** — it uses **two related keys**:
- A **public key** for encryption (shared openly)
- A **private key** for decryption (kept secret)

This design allows secure communication **without exchanging any secret key in advance**, 
making RSA one of the foundations of modern cryptography.

### The Core Idea

RSA’s security relies on a one-way mathematical problem:

> It’s easy to multiply two large prime numbers, but extremely difficult to factor their product back into those primes.

This difficulty is known as the **integer factorization problem**, which forms the mathematical backbone of RSA’s security.  
(Think of it as mixing two paint colors, easy to mix, nearly impossible to separate back into the originals.)

If we take two large prime numbers \( p \) and \( q \) and compute:

$$
n = p \times q
$$

we can safely publish \( n \) as part of the public key — since finding \( p \) and \( q \) from \( n \) is computationally infeasible.

### The Key Relationship

The **public** and **private** keys are mathematically connected through modular arithmetic.  
Their relationship is defined by the fundamental equation:

$$
(e \times d) \bmod \varphi(n) = 1
$$

Where:

- \( e \) → public exponent  
- \( d \) → private exponent  
-  φ(n) — Euler’s Totient function, calculated as φ(n) = (p - 1)(q - 1)


This means that \( d \) is the **modular inverse** of \( e \) modulo φ(n),  
ensuring that decryption perfectly reverses encryption.

---

## Mathematical Foundations

Before understanding RSA’s algorithm and security, we need to understand the number theory concepts it relies on:
**modular arithmetic**, **prime numbers**, and **Euler’s Totient Function**.

At the heart of RSA lies **modular arithmetic** — a system that enables mathematical operations to be performed efficiently even with extremely large numbers.

### Modular Arithmetic

RSA operates entirely in the realm of **modular arithmetic** — arithmetic where numbers “wrap around” after reaching a certain value \( n \).  
This is often described as **clock arithmetic**.

#### Definition

Two numbers \( a \) and \( b \) are **congruent modulo n** if they leave the same remainder when divided by \( n \):

$$
a \equiv b \pmod{n} \quad \text{if and only if} \quad n \mid (a - b)
$$

This means \( a \) and \( b \) differ by a multiple of \( n \).

#### Operations in Modular Arithmetic

For any modulus \( n \), the following properties hold:

$$
(a + b) \bmod n = ((a \bmod n) + (b \bmod n)) \bmod n
$$

$$
(a \times b) \bmod n = ((a \bmod n) \times (b \bmod n)) \bmod n
$$

$$
(a^k) \bmod n = ((a \bmod n)^k) \bmod n
$$

These properties allow RSA to perform encryption and decryption efficiently using **modular exponentiation**.

#### Modular Inverse

The **modular inverse** of \( a \) (mod \( n \)) is a number \( a^{-1} \) such that:

$$
a \times a^{-1} \equiv 1 \pmod{n}
$$

This inverse exists **if and only if** \( \gcd(a, n) = 1 \).  
In RSA, the private exponent \( d \) is the modular inverse of \( e \) modulo \( \varphi(n) \).

**Example:**

Find the modular inverse of \( a = 3 \) modulo \( n = 11 \).

We seek \( x \) such that:

$$
3x \equiv 1 \pmod{11}
$$

The solution is \( x = 4 \), since \( 3 \times 4 = 12 \equiv 1 \pmod{11} \).

Hence, \( 3^{-1} \equiv 4 \pmod{11} \).

### Prime Numbers

Primes are the **foundation of RSA’s security**.  
A **prime number** is an integer greater than 1 that has no positive divisors other than 1 and itself.

#### Why Primes Matter

RSA relies on the product of two large primes:

$$
n = p \times q
$$

This number \( n \) is easy to compute but extremely hard to factor back into its original primes.

Factoring \( n \) into \( p \) and \( q \) is the **RSA problem** — the core security assumption on which RSA is built.

#### Generating Secure Primes

To ensure security:
- \( p \) and \( q \) must be **large** (at least 1024 bits each for a 2048-bit RSA key).  
- They must be **randomly generated** and **independent**.  
- They must pass **probabilistic primality tests** (like Miller–Rabin).

If primes are poorly chosen or reused, the RSA system can be broken.

### Euler’s Totient Function

Euler’s Totient Function, denoted as \( \varphi(n) \), counts the number of integers less than \( n \) that are **coprime** to \( n \) (i.e., share no common divisor other than 1).

#### Definition

For a prime number \( p \):

$$
\varphi(p) = p - 1
$$

For a product of **two distinct primes** \( p \) and \( q \) (as in RSA):

$$
\varphi(n) = \varphi(p \times q) = (p - 1)(q - 1)
$$

#### Example

If  
\( p = 7 \) and \( q = 13 \), then  

$$
n = 7 \times 13 = 91
$$  

and  

$$
\varphi(n) = (7 - 1)(13 - 1) = 6 \times 12 = 72
$$

So there are 72 integers between 1 and 91 that are coprime with 91.

#### Connection to RSA

Euler’s theorem states that if \( a \) and \( n \) are coprime:

$$
a^{\varphi(n)} \equiv 1 \pmod{n}
$$

Euler’s theorem generalizes Fermat’s Little Theorem and ensures that exponentiation in modular arithmetic behaves predictably — a property RSA exploits directly.

Because the public and private exponents satisfy:

$$
(e \times d) \bmod \varphi(n) = 1
$$

it follows that:

$$
(M^e)^d \equiv M^{e \times d} \pmod{n}
$$

and therefore:

$$
M^{e \times d} \equiv M \pmod{n}
$$

This guarantees that encryption followed by decryption (or vice versa) always recovers the original message.

### Summary of Key Relationships

| Concept            | Formula                                  | Meaning                                |
|:-------------------|:-----------------------------------------|:---------------------------------------|
| Modular congruence | \( a \equiv b \pmod{n} \)                | a and b have the same remainder mod n  |
| Modular inverse    | \( a \times a^{-1} \equiv 1 \pmod{n} \)  | defines inverse relationship           |
| RSA modulus        | \( n = p \times q \)                     | public modulus                         |
| Totient            | \( \varphi(n) = (p - 1)(q - 1) \)        | key to private exponent                |
| Key relation       | \( (e \times d) \bmod \varphi(n) = 1 \)  | ensures decryption reverses encryption |
| Encryption         | \( C = M^e \bmod n \)                    | public-key encryption                  |
| Decryption         | \( M = C^d \bmod n \)                    | private-key decryption                 |

---

## Key Generation Explained

With the mathematical principles established, we can now see how RSA turns these concepts into real cryptographic keys.  
It creates a **public key** for encryption and a **private key** for decryption, both mathematically linked but computationally infeasible to derive from one another.

---

### Step 1 — Choose Two Large Prime Numbers

Select two large, random, and independent prime numbers:

$$
p \quad \text{and} \quad q
$$

Example (for illustration only, not secure):

$$
p = 61, \quad q = 53
$$

Then compute the **modulus**:

$$
n = p \times q = 61 \times 53 = 3233
$$

This \( n \) will be part of both the **public** and **private** keys.

---

### Step 2 — Compute Euler’s Totient Function

For distinct primes \( p \) and \( q \):

$$
\varphi(n) = (p - 1)(q - 1)
$$

Example:

$$
\varphi(3233) = (61 - 1)(53 - 1) = 60 \times 52 = 3120
$$

\( \varphi(n) \) defines the size of the multiplicative group — the number of integers that are coprime to \( n \).

---

### Step 3 — Choose a Public Exponent (e)

Select an integer \( e \) that satisfies:

$$
1 < e < \varphi(n)
$$

and

$$
\gcd(e, \varphi(n)) = 1
$$

That means \( e \) and \( \varphi(n) \) share no common factors.

Common choice in practice:

$$
e = 65537
$$

(\(65537 = 2^{16} + 1\)) — a prime number that makes encryption efficient and avoids certain attacks.

---

### Step 4 — Compute the Private Exponent (d)

Compute \( d \), the **modular multiplicative inverse** of \( e \) modulo \( \varphi(n) \):

$$
(e \times d) \bmod \varphi(n) = 1
$$

This means \( d \) is chosen such that:

$$
d = e^{-1} \pmod{\varphi(n)}
$$

In practice, \( d \) is computed efficiently using the **Extended Euclidean Algorithm**, which finds the modular inverse even for very large integers.

Example (continuing from above):

$$
e = 17, \quad \varphi(n) = 3120
$$

We find:

$$
d = 2753
$$

Check:

$$
(17 \times 2753) \bmod 3120 = 1
$$

Verified — \( d \) is the correct modular inverse of \( e \).

---

### Step 5 — Form the Key Pairs

Now we can construct the keys:

| Key Type | Components | Purpose |
|:----------|:------------|:---------|
| **Public Key** | \( (n, e) \) | Shared openly — used to encrypt messages |
| **Private Key** | \( (n, d) \) | Kept secret — used to decrypt messages |

Using our example:

| Key | Values |
|:----|:--------|
| Public Key | \( (3233, 17) \) |
| Private Key | \( (3233, 2753) \) |

---

### Step 6 — Verify the Relationship

To confirm that encryption and decryption are inverses, check that:

$$
(M^e)^d \equiv M^{e \times d} \pmod{n}
$$

and since \( e \times d \equiv 1 \pmod{\varphi(n)} \), it follows that:

$$
M^{e \times d} \equiv M \pmod{n}
$$

For any plaintext \( M \) such that \( 0 < M < n \) and \( \gcd(M, n) = 1 \).

This works because of **Euler’s theorem** and the way we defined \( d \) as the modular inverse of \( e \).

---

### Step 7 — Security Notes

To ensure cryptographic strength in real-world RSA:

- Use **at least 2048-bit keys** (\( p \) and \( q \approx 1024 \) bits each).  
- **Never reuse primes** across different keys.  
- Ensure **random, independent** prime generation.  
- **Discard \( p \) and \( q \)** after key generation to prevent private key reconstruction.  
- Protect the **private key (\( d \))** — exposure compromises all encryption.  
- Use **padding schemes** like **OAEP** to prevent deterministic attacks.

---

### Summary of the RSA Key Generation Process

| Step  | Formula                                                           | Description                      |
|:------|:------------------------------------------------------------------|:---------------------------------|
| 1     | \( n = p \times q \)                                              | Compute modulus from two primes  |
| 2     | \( \varphi(n) = (p - 1)(q - 1) \)                                 | Compute totient                  |
| 3     | \( \text{Choose } e \text{ such that } \gcd(e, \varphi(n)) = 1 \) | Select public exponent           |
| 4     | \( d = e^{-1} \pmod{\varphi(n)} \)                                | Compute private exponent         |
| 5     | \( (n, e) \) → Public Key                                         | Used for encryption              |
| 6     | \( (n, d) \) → Private Key                                        | Used for decryption              |
| 7     | \( (M^e)^d \equiv M \pmod{n} \)                                   | Encryption/decryption validation |

---

## Encryption Process

Once the key pair is generated, the public key \((n, e)\) can be used by anyone to encrypt messages securely.  
This section describes how a sender encrypts a message using the recipient’s **public key**.  
It outlines the formal mathematical process, conversion steps, and a small example to solidify understanding.

---

### Overview

The encryption step **locks the message mathematically** — only the corresponding private key can unlock it.  
Encryption in RSA transforms a plaintext message \( M \) into a ciphertext \( C \) using the **public key**.  
The operation is based on modular exponentiation:

$$
C \equiv M^e \pmod{n}
$$

where:

- \( M \) — plaintext message represented as an integer  
- \( e \) — public exponent  
- \( n \) — modulus (product of two large primes)

The result \( C \) can only be decrypted using the private exponent \( d \).

---

### Step 1 — Convert Plaintext to an Integer

Before encryption, the plaintext (byte string) must be converted into an integer smaller than \( n \).  
This is done using **OS2IP (Octet String to Integer Primitive)**:

$$
m = \textit{OS2IP}(M)
$$

where \( M \) is the padded message in bytes and \( m \) is its integer representation.

You must ensure:

$$
0 \le m < n
$$

If \( m \ge n \), the message must be re-padded or split into smaller blocks.

---

### Step 2 — Encryption Formula

Compute the ciphertext integer \( C \) using modular exponentiation:

$$
C \equiv m^e \pmod{n}
$$

This is the **core encryption equation** of RSA.

---

### Step 3 — Convert Ciphertext to Bytes

Once \( C \) is computed, it’s converted back into a byte string using **I2OSP (Integer to Octet String Primitive)**:

$$
C_{\text{bytes}} = \textit{I2OSP}(C, k)
$$

where \( k = \left\lceil \frac{\text{bitlen}(n)}{8} \right\rceil \)  
is the length in bytes of the modulus \( n \).

---

### End-to-End Encryption Chain

The full encryption pipeline can be summarized as:

$$
M \xrightarrow[\text{encode + pad}]{} m 
\xrightarrow[\text{public key }(n,e)]{C = m^e \bmod n} 
C \xrightarrow[\text{convert}]{} C_{\text{bytes}}
$$

or equivalently:

$$
C_{\text{bytes}} = \textit{I2OSP}\big((\textit{OS2IP}(M))^e \bmod n,\, k\big)
$$

---

### Example (Toy Numbers — Not Secure)

Using the example key:

$$
p = 61, \quad q = 53, \quad n = 3233, \quad e = 17
$$

and plaintext integer:

$$
m = 123
$$

Encryption:

$$
C = 123^{17} \bmod 3233 = 855
$$

Result:  
Ciphertext integer \( C = 855 \)

In real RSA, the plaintext is padded and converted to/from bytes, but the mathematical principle is identical.

---

### Padding and Practical Security

**Never** use textbook RSA (raw \( m^e \bmod n \)) — it’s insecure.  
Always use **padding schemes** that add randomness and structure:

- **OAEP (Optimal Asymmetric Encryption Padding)** → modern standard (RSAES-OAEP).  
- **PKCS#1 v1.5** → older but still seen in legacy systems.

These prevent deterministic output and chosen-ciphertext attacks.

---

### Key Equations (Visible)

$$
\begin{aligned}
m &= \textit{OS2IP}(M) \\\\
C &\equiv m^e \pmod{n} \\\\
C_{\text{bytes}} &= \textit{I2OSP}(C, k)
\end{aligned}
$$

---

### Important Notes

1. **Message size limit**

The plaintext (after padding) must fit into \( k \) bytes, where:

$$
k = \left\lceil \frac{\text{bitlen}(n)}{8} \right\rceil
$$

For example, with a 2048-bit modulus, \( k = 256 \) bytes.

2. **Hybrid encryption (real-world use)**

RSA is slow and not designed for large data.  
Instead, it encrypts a **random symmetric key** (used for AES, ChaCha20, etc.):

$$
C_K = K^e \bmod n
$$

The receiver decrypts the random key with RSA and then decrypts the data with AES.

3. **Coprimality requirement**

Mathematically, the proofs assume \( \gcd(m, n) = 1 \).  
In practice, secure padding ensures this is almost always true.

4. **Determinism**

Textbook RSA is deterministic (same plaintext → same ciphertext).  
Padding (OAEP) introduces randomness, making it **semantically secure**.

---

### Summary Table

| Step                  | Formula                                                                   | Description                   |
|:----------------------|:--------------------------------------------------------------------------|:------------------------------|
| Convert message       | \( m = \textit{OS2IP}(M) \)                                               | Byte string → integer         |
| Encrypt               | \( C \equiv m^e \pmod{n} \)                                               | Modular exponentiation        |
| Convert ciphertext    | \( C_{\text{bytes}} = \textit{I2OSP}(C, k) \)                             | Integer → byte string         |
| Full process          | \( C_{\text{bytes}} = \textit{I2OSP}((\textit{OS2IP}(M))^e \bmod n, k) \) | Combined encryption pipeline  |

---

## RSA in Real-World Security

Now that we understand how RSA encrypts and decrypts messages, let’s explore how it’s actually used in real systems and protocols.  
RSA is not just a theoretical system — it underpins decades of secure communication on the Internet.  
However, it is rarely used in its raw mathematical form.  
Modern systems use RSA for **key exchange**, **digital signatures**, and **hybrid encryption**, combined with secure padding and symmetric ciphers.

---

### 1. RSA for Key Exchange

RSA enables two parties to establish a **shared secret** (session key) over an insecure channel.

Typical workflow (as used in classic TLS versions):

$$
\text{EncryptedKey} = k^e \bmod n
$$

$$
k = (\text{EncryptedKey})^d \bmod n
$$

Then all communication is encrypted symmetrically:

$$
\text{Ciphertext} = \mathrm{AES\_Encrypt}(k,\, \text{Plaintext})
$$

---

#### Real-World Example: TLS Handshake (Pre-TLS 1.3)

| Step  | Description                                                   |
|:----: |:--------------------------------------------------------------|
| 1     | Client fetches server’s certificate (contains RSA public key) |
| 2     | Client generates random pre-master secret (session key)       |
| 3     | Encrypts it using server’s RSA public key                     |
| 4     | Server decrypts with private key                              |
| 5     | Both derive symmetric keys (AES) for the session              |

In **TLS 1.3**, RSA key exchange was replaced by **Ephemeral Diffie-Hellman (ECDHE)** for forward secrecy, but RSA still appears in **digital signatures**.

---

### 2. RSA for Digital Signatures

In this mode, RSA operates in the **reverse direction** — using the private key to sign data and the public key to verify it.

#### Signing
1. Compute message hash \( h = H(M) \).  
2. Apply the RSA private key:

$$
S \equiv h^d \pmod{n}
$$

#### Verification
The verifier computes:

$$
h' \equiv S^e \pmod{n}
$$

and checks:

$$
h' \stackrel{?}{=} H(M)
$$

If equal → signature is valid.

---

#### Signature Schemes

RSA signatures require padding schemes to ensure security and interoperability:

| Scheme                 | Purpose                                                  | Status                              |
|:-----------------------|:---------------------------------------------------------|:------------------------------------|
| **RSASSA-PKCS#1 v1.5** | Legacy padding, simple but deterministic                 | Still supported (e.g., X.509 certs) |
| **RSASSA-PSS**         | Probabilistic padding (recommended by NIST, FIPS 186-5)  | Modern standard for signatures      |

Equation for RSA-PSS verification:

$$
(S^e \bmod n) \rightarrow \text{verify padded hash}
$$

---

### 3. RSA in Email & File Encryption

RSA is also the core of **PGP/GPG** and **S/MIME** systems.

| Component              | What RSA Does                             | What Symmetric Algorithm Does |
|:-----------------------|:------------------------------------------|:------------------------------|
| **PGP / GPG**          | Encrypts the random session key           | Encrypts the actual message   |
| **S/MIME**             | Wraps a symmetric content-encryption key  | Protects the email body       |
| **Encrypted archives** | Encrypts the AES key stored with the file | Encrypts file contents        |

Symbolically:

$$
\text{Ciphertext} = \text{Encrypt}_{\text{AES}}(k, M)
$$

---

### 4. RSA in Authentication and Identity

RSA keys are also used for **identity verification** — not only encryption.

| Use Case               | Description                                                          |
|:-----------------------|:---------------------------------------------------------------------|
| **SSH authentication** | Clients prove identity by signing a challenge with their private key |
| **X.509 certificates** | RSA public key identifies the owner of a domain or organization      |
| **JWT / OAuth tokens** | RSA public/private key pairs sign and verify JSON Web Tokens         |

In these systems, the **private key never leaves the owner’s device**, while the **public key** is embedded in certificates or metadata.

---

### 5. Modern Trends — Beyond RSA

While RSA remains widespread, new cryptographic standards are emerging due to **performance** and **post-quantum** concerns.

#### ⚙️ Performance & Scalability

RSA requires large keys for strong security:

| Security Level | Minimum RSA Key Size | ECC Equivalent |
|:---------------|:---------------------|:---------------|
| 112-bit        | 2048-bit RSA         | 224-bit ECC    |
| 128-bit        | 3072-bit RSA         | 256-bit ECC    |
| 192-bit        | 7680-bit RSA         | 384-bit ECC    |
| 256-bit        | 15360-bit RSA        | 512-bit ECC    |

Because ECC (Elliptic Curve Cryptography) offers the same security with smaller keys, most modern systems now use:

- **ECDSA** → replaces RSA-PSS for signatures  
- **ECDHE** → replaces RSA key exchange

---

#### ⚛️ Post-Quantum Cryptography (PQC)

Quantum algorithms like **Shor’s Algorithm** can break RSA by factoring \( n = p \times q \) efficiently.  
Hence, research and deployment are moving toward **quantum-resistant** schemes:

| Family        | Example Algorithms | Role                    |
|:--------------|:-------------------|:------------------------|
| Lattice-based | Kyber, Dilithium   | Encryption & Signatures |
| Hash-based    | SPHINCS+           | Signatures              |
| Code-based    | Classic McEliece   | Encryption              |
| Multivariate  | Rainbow            | (Legacy research)       |

As of **NIST’s 2025 standardization**, **Kyber** (for encryption/key exchange) and **Dilithium** (for signatures) are the primary successors to RSA and ECC.

---

## Common Vulnerabilities and Attacks

Although RSA is mathematically elegant and secure when implemented correctly, **most real-world vulnerabilities arise from incorrect parameter choices, bad randomness, or insecure padding** — not from breaking the core math.

This section covers the **most common attacks and weaknesses** against RSA, both theoretical and practical.

---

### 1. Small Public Exponent Attack (Low-\( e \) Attack)

Using a small public exponent (like \( e = 3 \)) can make RSA encryption faster — but **unsafe** if padding or message structure is weak.

#### Vulnerable Case

If the plaintext \( m \) is small enough that:

$$
m^e < n
$$

then modular reduction does not occur, and the ciphertext is simply:

$$
C = m^e
$$

Hence, the plaintext can be recovered directly:

$$
m = \sqrt[e]{C}
$$

#### Example
For \( e = 3 \) and a small message,  
\( C = m^3 \) ⇒ attacker can compute the integer cube root of \( C \) to reveal \( m \).

**Fix:** Always use **padding schemes** like **OAEP**, which randomize \( m \) before encryption.

---

### 2. Common Modulus Attack

If **two users share the same modulus \( n \)** but have **different public exponents** \( e_1, e_2 \), an attacker can recover the plaintext if the same message \( m \) is encrypted for both keys.

#### Given:

$$
C_1 = m^{e_1} \bmod n, \quad C_2 = m^{e_2} \bmod n
$$

and \( \gcd(e_1, e_2) = 1 \).

#### Then:

The attacker can compute integers \( a, b \) such that:

$$
a e_1 + b e_2 = 1
$$

(using the **Extended Euclidean Algorithm**) and derive:

$$
m \equiv C_1^a \times C_2^b \pmod{n}
$$

**Fix:** Never reuse the same modulus across key pairs.

---

### 3. Low Private Exponent (Wiener’s Attack)

If the private exponent \( d \) is too small (relative to \( n \)), an attacker can recover \( d \) using **continued fractions**.

#### Vulnerability Condition

If:

$$
d < \frac{1}{3} n^{0.25}
$$

then Wiener’s attack can efficiently recover \( d \) from \( (n, e) \).

**Fix:** Generate keys such that \( d \) is large enough — standard libraries enforce this.

---

### 4. Faulty Random Prime Generation

If the primes \( p \) and \( q \) used to generate \( n \) are not truly random — for example, if they share bits or are reused — an attacker can factor \( n \).

#### Example — Shared Prime Factor

If two RSA keys share one prime (e.g., \( p_1 = p_2 \)):

$$
n_1 = p_1 q_1, \quad n_2 = p_1 q_2
$$

then:

$$
\gcd(n_1, n_2) = p_1
$$

reveals the shared factor immediately.

**Fix:** Always use **cryptographically secure random number generators (CSPRNGs)** when generating primes.

---

### 5. Partial Key Exposure (CRT Fault or Side-Channel Attack)

When RSA is optimized with the **Chinese Remainder Theorem (CRT)** for faster decryption, hardware faults or timing leaks can expose key fragments.

#### Example

If a computation fault reveals incorrect signatures \( S' \),  
and you know the correct signature \( S \), you can compute:

$$
\gcd(S - S', n)
$$

to recover one of the primes.

**Fixes:**
- Implement **RSA blinding** to randomize inputs.
- Perform **fault detection** (re-encrypt and verify signature).
- Use **constant-time operations** to prevent timing side-channels.

---

### 6. Padding Oracle Attacks (Bleichenbacher’s Attack)

A famous attack against **RSA PKCS#1 v1.5** encryption (discovered in 1998).

If a server reveals whether a ciphertext has “valid padding” or not, the attacker can iteratively query the oracle and recover the plaintext.

#### Attack Outline

Given \( C \), the attacker sends modified ciphertexts:

$$
C' = (C \times s^e) \bmod n
$$

and observes the server’s response to deduce intervals where the plaintext lies — eventually recovering \( m \).

**Fixes:**
- Use **RSAES-OAEP** (probabilistic padding).
- **Never** leak padding validation errors.
- Implement **uniform error messages**.

---

### 7. Timing and Side-Channel Attacks

Even if the math is perfect, side channels (like timing, power, or EM emissions) can leak secrets.

Examples:
- **Timing attacks** (Kocher, 1996) — measure decryption time variations to recover \( d \).  
- **Cache attacks** — infer secret key operations from cache access patterns.  
- **Power analysis** — extract key bits from power consumption traces.

**Fixes:**
- Use **constant-time arithmetic** and **RSA blinding**.
- Run on hardware with **side-channel protections** (e.g., HSMs).

---

### 8. Chosen Ciphertext and CCA2 Attacks

Plain RSA is **deterministic** and **malleable** — attackers can modify ciphertexts to produce predictable changes in plaintext.

For example:

$$
C' = (C \times r^e) \bmod n
$$

decrypts to:

$$
M' = M \times r \bmod n
$$

**Fix:** Always use **OAEP** (Optimal Asymmetric Encryption Padding) or **hybrid encryption** with symmetric ciphers.

---

### 9. Weak Key Parameters and Common Factors

If RSA keys are generated poorly — small \( n \), weak \( p, q \), or predictable structure — tools like **RsaCtfTool** or **factordb.com** can instantly factor them.

Example:  
A 512-bit RSA modulus can be factored in seconds using modern CPUs.

**Fixes:**
- Minimum recommended key size: **2048 bits**.  
- Use trusted crypto libraries (OpenSSL, GnuPG, PyCryptodome).

---




## Glossary of Terms

A quick reference to the key mathematical, cryptographic, and implementation terms used throughout this RSA section.

---

### A

**Asymmetric Cryptography** —  
A cryptographic system that uses *two different keys*: a public key for encryption or verification, and a private key for decryption or signing.

**Algorithm** —  
A finite sequence of instructions to solve a specific problem. RSA is an algorithm for public-key encryption and digital signatures.

---

### C

**Ciphertext (C)** —  
The encrypted form of a plaintext message.  
Computed in RSA as:  
$$
C \equiv M^e \pmod{n}
$$

**Chinese Remainder Theorem (CRT)** —  
A theorem in modular arithmetic that allows faster RSA decryption by splitting calculations into smaller moduli (\( p \) and \( q \)).  
Used to optimize performance but introduces fault-attack risks.

**Common Modulus Attack** —  
An attack that recovers plaintext when the same modulus \( n \) is reused across different RSA keys.

**CRT Fault Attack** —  
A hardware or timing fault during RSA-CRT computation that leaks one prime factor of \( n \).

---

### D

**Decryption** —  
The process of transforming ciphertext \( C \) back into plaintext \( M \) using the private key:  
$$
M \equiv C^d \pmod{n}
$$

**Digital Signature** —  
A cryptographic value generated with a private key to prove authenticity and integrity of a message.  
In RSA: \( S \equiv H(M)^d \pmod{n} \).

---

### E

**Encryption** —  
The process of encoding plaintext into ciphertext using a public key:  
$$
C \equiv M^e \pmod{n}
$$

**Euler’s Totient Function (\( \varphi(n) \))** —  
Counts the positive integers up to \( n \) that are coprime with \( n \).  
For RSA:  
$$
\varphi(n) = (p - 1)(q - 1)
$$

**Extended Euclidean Algorithm** —  
An algorithm to find integers \( x, y \) such that \( ax + by = \gcd(a, b) \).  
Used to compute the modular inverse \( d = e^{-1} \pmod{\varphi(n)} \).

---

### F

**Factoring Problem** —  
The hard mathematical problem underlying RSA security: finding the prime factors \( p \) and \( q \) of a large composite \( n = p \times q \).

**Fermat’s Little Theorem** —  
If \( p \) is prime and \( a \) is not divisible by \( p \):  
$$
a^{p-1} \equiv 1 \pmod{p}
$$
Euler’s theorem generalizes this for any modulus \( n \).

---

### G

**GCD (Greatest Common Divisor)** —  
The largest integer dividing two numbers \( a \) and \( b \).  
RSA requires \( \gcd(e, \varphi(n)) = 1 \) to ensure the existence of \( d \).

---

### H

**Hybrid Encryption** —  
A system combining asymmetric and symmetric cryptography:  
RSA encrypts a random symmetric key (for AES/ChaCha20), and that key encrypts the bulk data.

**Hash Function (H)** —  
A deterministic function that maps input data to a fixed-length digest.  
Used in RSA signatures to sign the hash instead of the raw message.

---

### I

**I2OSP (Integer-to-Octet-String Primitive)** —  
Converts an integer into a fixed-length byte string.  
Defined in PKCS#1 (RFC 8017) and used in RSA encryption/decryption steps.

---

### K

**Key Pair** —  
The public and private RSA keys generated from the same two primes \( p, q \).  
Public key = \( (n, e) \); Private key = \( (n, d) \).

---

### M

**Message (M)** —  
The plaintext (numerical representation) to be encrypted or signed.

**Modular Arithmetic** —  
Arithmetic performed with respect to a modulus \( n \).  
Numbers “wrap around” after reaching \( n \).

**Modular Exponentiation** —  
The operation \( a^b \bmod n \), central to RSA encryption and decryption.

**Modular Inverse** —  
For integers \( a, n \), the modular inverse \( a^{-1} \) satisfies:  
$$
a \times a^{-1} \equiv 1 \pmod{n}
$$

---

### O

**OAEP (Optimal Asymmetric Encryption Padding)** —  
A probabilistic padding scheme that adds randomness and structure before RSA encryption to ensure semantic security (used in RSAES-OAEP).

**OS2IP (Octet-String-to-Integer Primitive)** —  
Converts a byte string into an integer representation before encryption.

---

### P

**Plaintext (M)** —  
The original message before encryption.

**Prime Number** —  
A number greater than 1 divisible only by 1 and itself.  
RSA relies on large random primes \( p, q \).

**PKCS#1** —  
Public-Key Cryptography Standard defining RSA padding schemes and algorithms (OAEP, PSS).

---

### R

**RSA (Rivest–Shamir–Adleman)** —  
An asymmetric cryptographic algorithm invented in 1977 based on the difficulty of factoring large numbers.

---

### S

**Session Key (k)** —  
A temporary symmetric key used to encrypt a single communication session.  
Often exchanged using RSA key encapsulation.

**Signature (S)** —  
The RSA output representing a signed message digest.

---

### T

**Totient (\( \varphi(n) \))** —  
Another name for Euler’s Totient Function — the count of integers coprime to \( n \).

**TLS (Transport Layer Security)** —  
Protocol that uses RSA (or ECC) for key exchange and authentication in HTTPS.

---

### W

**Wiener’s Attack** —  
An attack exploiting small private exponents \( d \) using continued fractions to recover \( d \) when \( d < \frac{1}{3} n^{0.25} \).

---

### Symbols & Notation Quick Reference

| Symbol            | Meaning                                 |
|:------------------|:----------------------------------------|
| \( n \)           | RSA modulus = \( p \times q \)          |
| \( p, q \)        | Distinct prime factors of \( n \)       |
| \( e \)           | Public exponent                         |
| \( d \)           | Private exponent                        |
| \( \varphi(n) \)  | Euler’s Totient Function                |
| \( M \)           | Message (plaintext)                     |   
| \( C \)           | Ciphertext                              |
| \( S \)           | Digital signature                       |
| \( k \)           | Session key (for symmetric encryption)  |

---