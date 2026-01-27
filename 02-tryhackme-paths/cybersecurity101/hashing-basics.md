# What Hashing REALLY Is (and What It Is NOT)

## Hashing ≠ Encryption

This is the most important distinction.

| Encryption | Hashing |
|----------|--------|
| Reversible | Irreversible |
| Uses a key | No key |
| Used to hide data | Used to verify data |
| You decrypt | You compare |

Key facts:
- You **cannot decrypt a hash**
- If someone says “decrypt this hash”, they are wrong

A hash function:
- Takes input of any size  
- Produces a fixed-size output  
- Same input → same output  
- Tiny input change → completely different output  

---

## 2. Avalanche Effect (Why Hashes Look Random)

A tiny change in input causes a massive change in output.

Example (1-bit difference):

```
T → 01010100
U → 01010101
```

Yet:
- MD5 → completely different
- SHA1 → completely different
- SHA256 → completely different

This property is called the **avalanche effect**.

Good hash functions destroy patterns completely.

---

## 3. Why Hashes Are Encoded (Hex / Base64)

Internally:
- Hash output = raw bytes

Humans do not read raw bytes, so encoding is used:

- **Hex** → 2 hex characters per byte
- **Base64** → shorter but less readable

That is why:
```
sha256sum file
```
looks long and random.  
It is simply bytes printed in a readable format.

---

## 4. Why Hashing Is Critical in Real Life

Two major real-world uses:
1. Password storage  
2. Integrity checking  

You use hashing every day without seeing it.

---

## 5. Passwords: The Right Way vs The Wrong Way

### Worst Practices (Real Breaches)

#### 1. Plaintext passwords
Example: RockYou
- 14 million passwords leaked
- Enabled direct login to other services
- Still used in cracking today

#### 2. Encryption instead of hashing
Example: Adobe
- Used reversible encryption
- Stored the encryption key
- Once key leaked → all passwords exposed

#### 3. Weak hashing (no salt)
Example: LinkedIn
- Used SHA1
- No salt
- Instantly rainbow-table cracked

---

## 6. Hash Collisions (Why MD5 & SHA1 Are Dead)

### What is a collision?
Two different inputs producing the same hash.

Collisions are:
- Mathematically unavoidable
- Should be computationally infeasible

MD5 and SHA1:
- Collisions can be deliberately generated
- Cryptographically broken

Rule:
- MD5 / SHA1 → not safe for passwords or integrity
- SHA256+ → acceptable for integrity
- Passwords → use slow hashing only

---

## 7. Rainbow Tables (Why Salts Exist)

Without salt:
```
password → hash
password → same hash
```

Attackers can:
- Precompute millions of hashes
- Do fast lookups instead of cracking

Websites like CrackStation work this way.

---

## 8. Salting (The Most Important Defense)

What salting does:
```
password + random_salt → hash
```

Effects:
- Same password → different hashes
- Salt is not secret
- Salt is stored in the database
- Prevents rainbow tables
- Forces attackers to crack each hash individually

---

## 9. Proper Password Hashing (Best Practice)

Recommended modern algorithms:
- Argon2 (best)
- bcrypt
- scrypt
- PBKDF2

Properties:
- Slow by design
- Memory-hard
- GPU-unfriendly

Speed is bad for password hashing.

---

## Why Not Encrypt Passwords?

Encryption:
- Requires storing a key
- Key compromise = total failure

Hashing:
- No key
- One-way
- Much safer

---

## 10. Recognising Hash Types (Offensive Skill)

Tools like `hashid` help, but:
- They guess
- Context matters more

Example:
- 32 hex characters → MD5 or NTLM
- Context determines which one

---

## 11. Linux Password Hashes (`/etc/shadow`)

Format:
```
$username:$prefix$options$salt$hash:...
```

Example:
```
$y$j9T$salt$hash
```

The prefix identifies the algorithm.

### Common Prefixes (Important to Memorise)

| Prefix | Algorithm |
|------|----------|
| $y$ | yescrypt (modern default) |
| $7$ | scrypt |
| $2b$ | bcrypt |
| $6$ | sha512crypt |
| $1$ | md5crypt |

---

## 12. Windows Password Hashes

Stored in:
- SAM database

Algorithm:
- NTLM (MD4-based)
- No salt
- Extremely fast to crack

Consequences:
- Pass-the-hash attacks exist
- NTLM is heavily discouraged

---

## 13. Cracking Hashes (Reality Check)

You do not decrypt hashes.

You:
1. Guess a password
2. Hash it
3. Compare results

Tools:
- Hashcat (GPU-focused)
- John the Ripper (CPU-friendly)

GPUs can compute millions or billions of hashes per second.  
This is why slow hashes are critical.

---

## 14. Hashing for Integrity (File Verification)

Rules:
- Same file → same hash
- Different file → different hash

Used for:
- ISO downloads
- Malware detection
- Software verification

If SHA256 matches:
- File not modified
- File authentic (if signed)

---

## 15. HMAC (Hash + Secret Key)

Hash alone provides:
- Integrity only

HMAC provides:
- Integrity
- Authenticity

Formula:
```
HMAC(K, M) = H((K ⊕ opad) || H((K ⊕ ipad) || M))
```

Used in:
- APIs
- JWT
- Secure communications
- Message authentication
