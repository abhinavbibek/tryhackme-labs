# CRYPTOGRAPHY – FUNDAMENTALS & CORE CONCEPTS  

## 1. Importance of Cryptography

The main goal of cryptography is **secure communication in the presence of attackers**.

Secure communication mainly means:

- **Confidentiality** → data should not be readable by unauthorized people  
- **Integrity** → data should not be altered in transit  
- **Authenticity** → verifying who you are communicating with  

In simple terms, cryptography ensures that attackers **cannot read, modify, or fake messages**.

Cryptography is everywhere in daily digital life, even though users rarely notice it directly.

### Real-world examples

- When logging into websites (like TryHackMe), credentials are encrypted before being sent  
- SSH creates an encrypted tunnel so commands cannot be sniffed  
- Online banking verifies server certificates to prevent man-in-the-middle attacks  
- File downloads use cryptographic hashes to verify file integrity  
- Most modern internet communication happens over encrypted connections (HTTPS, TLS, SSH)

---

## 2. Cryptography and Legal Requirements

Cryptography is not just optional; it is often **legally required**.

Examples:

- **PCI DSS** → Required for companies handling credit card data  
  - Data must be encrypted at rest and in transit  

- **Healthcare data regulations**:
  - USA → HIPAA, HITECH  
  - EU → GDPR  
  - UK → DPA  

These regulations show that cryptography is a **mandatory security layer**, usually working silently in the background.

---

## 3. Historical Ciphers

Cryptography has existed for **thousands of years**.

### Caesar Cipher (1st century BCE)

One of the simplest encryption techniques:

- Each letter is shifted by a fixed number

**Example**

- Plaintext: `TRYHACKME`  
- Key: `3` (right shift)  
- Ciphertext: `WUBKDFNPH`

**Encryption mapping**
- T → W  
- R → U  
- Y → B (wraps around after Z)

**Decryption**
- Shift letters back by the same key

**Problems**
- Only 25 possible keys  
- Easy to brute force  
- Completely insecure by modern standards  

This shows why **modern cryptography assumes attackers know the algorithm, not the key**.

---

## 4. Other Historical Ciphers

Some well-known historical cryptographic systems:

- Vigenère Cipher (16th century)  
- Enigma Machine (World War II)  
- One-Time Pad (Cold War era)  

Many of these are interesting historically but are **no longer secure**.

---

## 5. Types of Encryption

Encryption is mainly divided into two categories:

- **Symmetric encryption**
- **Asymmetric encryption**

---

## 6. Symmetric Encryption

Symmetric encryption uses **one shared secret key** for:

- Encryption  
- Decryption  

### Key points

- Same key on both sides  
- Key must remain secret  
- Secure key exchange is difficult  
- Also called **private key cryptography**

**Real-world analogy**  
Sending a password-protected file:
- File is easy to send  
- Password needs a secure channel  

### Common symmetric algorithms

- DES (deprecated)  
- 3DES (deprecated)  
- AES (current standard)  

#### Algorithm notes

**DES**
- 56-bit key  
- Broken in under 24 hours (1999)

**3DES**
- DES applied three times  
- Effective security ≈ 112 bits  
- Deprecated in 2019

**AES**
- Standard since 2001  
- Key sizes: 128, 192, 256 bits  
- Widely used today

---

## 7. Asymmetric Encryption

Asymmetric encryption uses **two different keys**:

- **Public key** → encryption  
- **Private key** → decryption  

### Key points

- Public key can be shared openly  
- Private key must be kept secret  
- Also called **public key cryptography**  
- Slower than symmetric encryption  

### Common asymmetric algorithms

- RSA  
- Diffie-Hellman  
- Elliptic Curve Cryptography (ECC)  

### Key size comparison

- RSA: 2048+ bits recommended  
- Diffie-Hellman: 2048–4096 bits  
- ECC: 256-bit key ≈ 3072-bit RSA security  

Asymmetric encryption relies on math problems that are **easy to compute but practically impossible to reverse**.

---

## 8. Why Both Are Used Together

In real systems:

- Asymmetric encryption → used to **exchange keys**
- Symmetric encryption → used for **bulk data**

This combines:
- Secure key exchange  
- High performance  

---

## 9. Mathematical Foundations of Cryptography

Modern cryptography heavily depends on mathematics.

Two important operations:

- **XOR**
- **Modulo**

---

## 10. XOR Operation

XOR (exclusive OR) compares two bits:

- Same bits → 0  
- Different bits → 1  

### Truth table

| A | B | A ⊕ B |
|---|---|-------|
| 0 | 0 | 0 |
| 0 | 1 | 1 |
| 1 | 0 | 1 |
| 1 | 1 | 0 |

### Example

```
1010
1100
----
0110
```

### Important XOR properties

- A ⊕ A = 0  
- A ⊕ 0 = A  
- Commutative: A ⊕ B = B ⊕ A  
- Associative: (A ⊕ B) ⊕ C = A ⊕ (B ⊕ C)

---

## 11. XOR in Cryptography

XOR can act as a **basic symmetric encryption method**.

Let:
- P = plaintext  
- K = secret key  
- C = ciphertext  

### Encryption
```
C = P ⊕ K
```

### Decryption
```
P = C ⊕ K
```

Because:
```
(P ⊕ K) ⊕ K = P
```

**Important notes**
- Key must be as long as plaintext  
- Simple XOR alone is not secure  
- Concept is used inside modern ciphers  

---

## 12. Modulo Operation

Modulo gives the **remainder after division**.

### Examples

- 25 % 5 = 0  
- 23 % 6 = 5  
- 23 % 7 = 2  

### Important properties

- Result is always between `0` and `(divisor − 1)`  
- Not reversible  
- Infinite values can satisfy the same modulo equation  

**Example**
```
x % 5 = 4
```
Many values of `x` satisfy this.

---

## 13. Modulo in Cryptography

Modulo is heavily used in:

- RSA  
- Diffie-Hellman  
- ECC  

It allows:

- Working with very large numbers  
- Controlled numerical ranges  
- One-way mathematical behavior  

Programming languages like **Python** are commonly used because they support **large integers natively**.
