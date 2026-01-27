# Asymmetric (Public Key) Cryptography — Core Concepts & Practical Reality  

## 1. Why Public Key (Asymmetric) Cryptography Exists

### The core problem

Symmetric encryption is fast and secure, but it has a fatal flaw:

You must already share the secret key securely.

So the real question is:

How do two strangers agree on a secret key over the internet where attackers can listen?

This is exactly why asymmetric cryptography exists.

---

## 2. Lock-and-Box Analogy (Mental Model)

| Real Life | Cryptography |
|---------|-------------|
| Secret code | Symmetric key + algorithm |
| Lock | Public key |
| Lock’s key | Private key |

### Flow

1. Server gives you a lock (public key)  
2. You put your secret inside a box and lock it  
3. Only the server can open it (private key)  
4. After this, both sides switch to fast symmetric encryption  

Asymmetric cryptography is used only once, to safely exchange the symmetric key.

---

## 3. RSA — The Most Famous Asymmetric Algorithm

### What RSA is based on

RSA relies on a mathematical asymmetry:

- Easy: multiply two large prime numbers  
- Hard: factor their product  

Example:

Easy → `113 × 127 = 14351`  
Hard → Which two primes multiply to 14351?

For 600+ digit numbers, factoring is practically impossible with current computers.

---

## 4. RSA Variables (Important for CTFs)

| Symbol | Meaning |
|-----|-------|
| p, q | Large prime numbers |
| n | p × q |
| e | Public exponent |
| d | Private exponent |
| m | Message (plaintext) |
| c | Ciphertext |

Public key → `(n, e)`  
Private key → `(n, d)`

---

## 5. RSA Encryption & Decryption (Conceptual)

### Encryption
```
c = m^e mod n
```

### Decryption
```
m = c^d mod n
```

### Why it works
```
e × d ≡ 1 (mod φ(n))
```

This relies on modular arithmetic and number theory.

Important notes:
- In real life, `p` and `q` are hundreds of digits long  
- Never as small as demonstration examples  

---

## 6. RSA in CTFs (Practical Reality)

In CTF challenges, you are often given:
- `n`
- `e`
- `c`
- Sometimes `p` or `q`

Your task:
Recover `m` (usually the flag)

Tools you should know:
- RsaCtfTool
- rsatool
- Python:
```python
pow(c, d, n)
```

If `p` and `q` are leaked, RSA is immediately broken.

---

## 7. Diffie–Hellman (DH) — Key Exchange Without Encryption

RSA is not the only way to share keys.

Diffie–Hellman allows two parties to create a shared secret without ever sending the secret itself.

### Key idea

- Public values can be shared openly  
- Private values are never shared  
- Based on discrete logarithm problems  

---

## 8. Diffie–Hellman Step-by-Step

### Public values
```
p = large prime
g = generator
```

### Private values
```
Alice chooses a
Bob chooses b
```

### Public keys
```
A = g^a mod p
B = g^b mod p
```

### Shared secret
```
Alice: B^a mod p
Bob:   A^b mod p
```

### Result
```
g^(ab) mod p
```

Both sides derive the same secret, while an eavesdropper gains nothing useful.

---

## 9. RSA and Diffie–Hellman Together (Real World)

Modern secure protocols (TLS, SSH) combine both:

- Diffie–Hellman for shared key generation  
- RSA or Ed25519 for authentication and signatures  

Why this matters:
- Diffie–Hellman alone does not prove identity  
- RSA-based signatures prevent man-in-the-middle attacks  

---

## SSH — Where These Concepts Are Used

### Server Authentication

The SSH warning about an unknown host key means:

The client cannot verify the server’s identity.

If the server key changes later, it may indicate a man-in-the-middle attack.

---

### Client Authentication (SSH Keys)

Instead of passwords:
- Client proves identity using a private key  
- Server verifies using the corresponding public key  

Common algorithms:
- RSA  
- ECDSA  
- Ed25519  

---

## SSH Key Rules

- Private key is extremely sensitive  
- Never share it  
- File permissions must be set to `600`  
- Passphrase encrypts the key locally only  
- Passphrase is never sent to the server  

---

## Digital Signatures (Different from Encryption)

- Encryption ensures secrecy  
- Digital signatures ensure authenticity and integrity  

### How signing works

1. Hash the message  
2. Encrypt the hash using the private key  
3. Anyone can verify using the public key  

This proves:
- Who signed the message  
- That the message was not modified  

---

## Certificates and HTTPS

Browsers trust websites through:
- Certificate Authorities (CAs)
- A chain of trust
- Root certificates embedded in operating systems and browsers  

Notes:
- Let’s Encrypt provides free TLS certificates  
- Self-signed certificates encrypt traffic but do not verify identity  

---

## PGP / GPG (Email and File Security)

### What GPG provides

- File encryption  
- Email encryption  
- Digital signatures  

Rules:
- Public key can be shared  
- Private key must be protected carefully  

CTF relevance:
- Encrypted `.gpg` files are common  
- Tasks often involve importing keys and decrypting data  
