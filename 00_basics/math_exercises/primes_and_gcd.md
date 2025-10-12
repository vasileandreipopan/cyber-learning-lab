# Prime Numbers & GCD Exercises

## Goal
Recognize primes, compute GCDs, and understand why they matter for crypto.

---

### Problems
1. List all primes below 20.  
2. Compute gcd(12, 18).  
3. Which pairs are coprime?  
   - a) (9, 28)  
   - b) (12, 27)  
   - c) (7, 20)  
4. Why are primes crucial in RSA?  
5. Why must the encryption exponent e be coprime with φ(n)?

---

### Answers
1. 2, 3, 5, 7, 11, 13, 17, 19  
2. 6  
3. a) yes b) no c) yes  
4. Because RSA’s strength depends on how hard it is to factor n = p × q.  
5. Because otherwise the modular inverse (private key) wouldn’t exist.
