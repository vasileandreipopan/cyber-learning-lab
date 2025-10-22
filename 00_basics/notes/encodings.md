# Encodings — How Data Becomes Bytes

## What Is Encoding?

Encoding is the process of converting data from one form into another so computers can store, transmit, and process it.

Every encryption algorithm operates on **binary data** (0s and 1s). Understanding encodings ensures that when you encrypt or hash something, you know *exactly* what bytes you’re transforming.

---

## Common Encodings

|       Encoding        |     Base      |               Description                     |       Example         |
|-----------------------|---------------|-----------------------------------------------|-----------------------|
| **Binary**            | 2             | Represents data using only 0 and 1.           | 01001000 01101001     |
| **Hexadecimal (Hex)** | 16            | Compact way to show bytes (2 hex = 1 byte).   | `0x48 0x69`           |
| **ASCII / UTF-8**     | Text → bytes  | Maps letters to numbers.                      | “Hi” → `[0x48, 0x69]` |
| **Base64**            | 64            | Converts binary data to text-safe format.     | “Hi” → `SGk=`         |

---

## 📘 Key Concepts

- **Binary** is the native “language” of computers.
- **Hexadecimal** is used to represent bytes in a readable way.
- **Base64** is often used to embed binary data in text formats (like emails, JSON, or certificates).
- **Encoding ≠ Encryption:**  
  Encoding is reversible; encryption hides information.

---

## 🧠 Example

Take the word **"Hi"**:

**"H"** → ASCII: 72, Binary: 01001000, Hex: 0x48
**"i"** → ASCII: 105, Binary: 01101001, Hex: 0x69
