# Number Theory — The Math Behind Cryptography

---

## 🧠 Why Learn This?

Modern cryptography is built on **number theory**, the branch of mathematics that studies how integers behave — especially when divided or multiplied under constraints.

RSA, Diffie–Hellman, ECC, and even hashing depend on these principles.

If you understand modular arithmetic, primes, GCD, and modular inverses, you’ll understand how real encryption works.

---

## ⚙️ 1. Modular Arithmetic — Math That Wraps Around

### The Intuition

Think of a **12-hour clock**.  
If it’s 10 o’clock and you add 5 hours, it becomes 3 o’clock.

That’s **modular arithmetic** — math that loops back to 0 after reaching a fixed limit.

If we say “mod 12,” it means:

```
10 + 5 = 3  (because 15 wraps around after 12)
```

We write this as:

```
(10 + 5) mod 12 = 3
```

### 📘 Formal Definition

`a mod n` = the **remainder** when dividing `a` by `n`.

Examples:

```
13 mod 5 = 3   → because 13 = 2×5 + 3
7 mod 3 = 1    → because 7 = 2×3 + 1
```

If we are “working mod 5,” then all possible results are in the range **0–4**.

---

### Properties of Modular Arithmetic

|   Operation    |                     Rule                      |      Example      |
|----------------|-----------------------------------------------|-------------------|
| Addition       | (a + b) mod n = ((a mod n) + (b mod n)) mod n | (7 + 8) mod 5 = 0 |
| Multiplication | (a × b) mod n = ((a mod n) × (b mod n)) mod n | (3 × 4) mod 5 = 2 |
| Subtraction    | (a − b) mod n = ((a mod n) − (b mod n)) mod n | (2 − 5) mod 7 = 4 |

These rules keep crypto math consistent — results always stay inside a limited “field.”

---

## ⚙️ 2. Modular Addition, Multiplication, and Exponentiation

### Modular Addition

Add the numbers, then take the remainder:

```
(7 + 11) mod 5 = 18 mod 5 = 3
```

### Modular Multiplication

Multiply, then take the remainder:

```
(4 × 9) mod 7 = 36 mod 7 = 1
```

### Modular Exponentiation

Raise to a power, then take the remainder:

```
(3^4) mod 5 = 81 mod 5 = 1
```

Modular exponentiation is the basis of **RSA** and **Diffie–Hellman**.  
Example:

```
Cipher = (Message^e) mod n
```

This is literally how encryption is performed in RSA.

---

## ⚙️ 3. Modular Inverses — The “Undo” Operation

In normal math:

```
3 × (1/3) = 1
```

So `1/3` is the inverse of 3.

But modular arithmetic doesn’t allow fractions, so we find the number that “undoes” multiplication under a modulus.

### 📘 Definition

The modular inverse of `a` (mod n) is a number `x` such that:

```
(a × x) mod n = 1
```

### Example

Find the inverse of 3 mod 5:

```
3×1 mod 5 = 3
3×2 mod 5 = 1 ✅
```

So the modular inverse of 3 mod 5 is **2**.

It means `(3 × 2) mod 5 = 1`.

---

### ⚠️ When Does an Inverse Exist?

A modular inverse exists **only if** the two numbers are **coprime** —  
i.e., their greatest common divisor (GCD) equals 1.

| a | n  | gcd(a, n) | Inverse Exists? |    Example      |
|---|----|-----------|-----------------|-----------------|
| 3 | 5  |    1      |    ✅ Yes       | 3⁻¹ mod 5 = 2   |
| 4 | 8  |    4      |    ❌ No        | None            |
| 7 | 26 |    1      |    ✅ Yes       | 7⁻¹ mod 26 = 15 |

If the numbers aren’t coprime, no modular inverse exists.

This concept is **critical for RSA key generation**, because private keys are modular inverses.

---

## ⚙️ 4. GCD — Greatest Common Divisor

### What Is It?

The **Greatest Common Divisor (GCD)** of two integers is the largest number that divides both without leaving a remainder.

Examples:

```
gcd(12, 18) = 6
gcd(15, 28) = 1  → they are coprime
```

When gcd(a, n) = 1 → a and n share no common factors → a has an inverse mod n.

---

### Euclidean Algorithm (Finding GCD)# Number Theory — The Math of Cryptography

## What & Why
Cryptography lives in modular arithmetic.  
RSA, Diffie-Hellman, and ECC all rely on math that happens “mod n”.

## Key Concepts

### Modular Arithmetic
Think of it like a clock:  
`a mod n` = remainder when `a` is divided by `n`.

Examples:
- `13 mod 5 = 3`
- `(7 + 8) mod 5 = 0`
- `(3 × 4) mod 5 = 2`

### Modular Inverse
The number `x` such that `(a × x) mod n = 1`.

Example:  
`3 × 2 mod 5 = 6 mod 5 = 1`, so the inverse of 3 mod 5 is 2.

**Use in RSA:** calculating the private exponent `d` from `e`.

### GCD (Greatest Common Divisor)
Used to check if two numbers are coprime.  
If `gcd(a, n) = 1`, `a` has an inverse mod `n`.

### Prime Numbers
- Prime numbers have no divisors except 1 and themselves.
- Used to build `n = p × q` in RSA, making factoring hard.

### Modular Exponentiation
Efficient way to compute `(a^b) mod n`.  
Used in encryption, decryption, key exchange.

## Real-world Crypto Links
|      Concept      |           Used In             |
|-------------------|-------------------------------|
| Mod arithmetic    | AES S-boxes, RSA, DH          |
| Modular inverse   | RSA private key calculation   |
| Primes            | RSA key generation            |
| Exponentiation    | Encryption/decryption         |   

## Takeaways
1. Modular arithmetic defines how crypto math “wraps around.”
2. Inverses only exist for coprime numbers.
3. Efficient exponentiation keeps crypto fast and secure.


The **Euclidean Algorithm** efficiently finds the GCD of two numbers.

Example: gcd(56, 15)

```
56 ÷ 15 = 3 remainder 11
15 ÷ 11 = 1 remainder 4
11 ÷ 4  = 2 remainder 3
4 ÷ 3   = 1 remainder 1
3 ÷ 1   = 3 remainder 0
→ GCD = 1
```

Since we ended with remainder 1, 56 and 15 are **coprime**.

Crypto software (like OpenSSL) uses this same logic internally to validate key pairs.

---

## ⚙️ 5. Prime Numbers — The Atoms of Cryptography

### Definition

A **prime number** is greater than 1 and divisible only by 1 and itself.

Examples:

```
2, 3, 5, 7, 11, 13, 17, 19...
```

### Why Primes Matter

Public-key encryption (like RSA) relies entirely on prime numbers.

In RSA, you pick two large primes, `p` and `q`, and multiply them to create:

```
n = p × q
```

It’s **easy** to multiply primes, but **extremely hard** to reverse the process (factoring n back into p and q).  
That one-way property gives RSA its security.

---

### Example: Small RSA-Style Setup

Let’s see how number theory builds an encryption system.

1.- Choose primes:

```
p = 17
q = 19
```

2.- Compute modulus and Euler’s totient:

```
n = p × q = 323
φ(n) = (p−1)(q−1) = 16×18 = 288
```

3.- Choose encryption exponent `e` (coprime with φ(n)):

```
e = 5
```

4.- Compute the modular inverse of e mod φ(n):

```
d = e⁻¹ mod 288 = 173
```

Now `(e, n)` is the **public key**, and `(d, n)` is the **private key**.

Encryption and decryption:

```
Ciphertext = (Message^e) mod n
Message = (Ciphertext^d) mod n
```

Every step uses modular arithmetic, primes, and inverses — nothing else.

---

## ⚙️ 6. Modular Exponentiation — Making Huge Powers Possible

In encryption, we often compute:

```
C = (M^e) mod n
```

But `M^e` could be astronomically large, so computers use an efficient trick:
they reduce the result **at every step** using modular arithmetic.

Example:

```
(7^4) mod 11
= (7×7×7×7) mod 11
= 2401 mod 11
= 3
```

This technique (called *modular exponentiation by squaring*) allows RSA and Diffie–Hellman to be fast even with 2048-bit keys.

---

## ⚙️ 7. Putting It All Together

|      Concept      |       What It Does        |    Example    |     Used In       |
|-------------------|---------------------------|---------------|-------------------|
| Modulo            | Keeps numbers in range    | 13 mod 5 = 3  | All crypto        |
| GCD               | Finds shared factors      | gcd(15, 28)=1 | Key validation    |
| Coprime           | Allows inverses           | (7, 26)       | RSA               |
| Modular inverse   | Undoes multiplication     | 3⁻¹ mod 5=2   | RSA private key   |
| Primes            | Hard to factor            | 17, 19        | RSA, ECC          |
| Exponentiation    | Encrypts/decrypts         | (m^e) mod n   | RSA, DH           |

---