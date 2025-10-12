# Modular Arithmetic Exercises

## Goal
Practice the logic of modular arithmetic to build intuition for cryptographic math.

---

### Problems
1. Compute:
   - a) 15 mod 4  
   - b) (7 × 5) mod 6  
   - c) (5³) mod 13  
   - d) (9 + 8) mod 7  

2. Find modular inverses (if they exist):
   - a) 3 mod 10  
   - b) 7 mod 26  
   - c) 6 mod 9  

3. Conceptual:
   - Why does `(a × b) mod n = [(a mod n) × (b mod n)] mod n`?
   - In what situations would an inverse *not* exist?

---

### Answers
1. a) 3 b) 5 c) 8 d) 3  
2. a) 7 b) 15 c) none (not coprime)  
3. Because modular arithmetic distributes over multiplication; inverses only exist if gcd(a, n) = 1.
