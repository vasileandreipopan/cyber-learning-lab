# XOR Logic Exercises

## Goal
Understand how XOR works and why it’s crucial in encryption and hashing.

---

### ⚙️ Key Concept
| A | B | A XOR B |
|:-:|:-:|   :-:   |
| 0 | 0 |    0    |
| 0 | 1 |    1    |
| 1 | 0 |    1    |
| 1 | 1 |    0    |

**Reversible property:**  
`A XOR B XOR B = A`

---

### Problems
1. What is 1010 XOR 1100 ?  
2. If `Key = 01101100` and `Plaintext = 10011010`, what is Ciphertext?  
3. Explain why XOR is used in the One-Time Pad (OTP).  
4. What happens if you reuse the same OTP key?

---

### Answers
1. 0110  
2. 11110110  
3. Because XOR perfectly hides data when the key is random and unique.  
4. Reusing the same key leaks information — attackers can recover plaintexts.
