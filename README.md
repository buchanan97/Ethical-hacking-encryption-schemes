# Ethical Hacking – Encryption Schemes

A Python project that demonstrates **brute-force hash-cracking** techniques used in ethical hacking. The program systematically generates every possible licence-plate string in the format `LLL-NNNN` (three uppercase letters followed by four digits), hashes each candidate with **SHA-256**, and checks whether the resulting hash matches one of a set of pre-defined target hashes.

---

## Demo Video

[![Watch the demo](https://cdn.loom.com/sessions/thumbnails/3c2c1aed52174a94989c37ec599f0bc3-with-play.gif)](https://www.loom.com/share/3c2c1aed52174a94989c37ec599f0bc3)

> Click the thumbnail above to watch a full walkthrough of the tool in action.

---

## How It Works

1. **Target hashes** – A set of known SHA-256 hashes (stored in `Brute force hash attack`) represents the "unknown" licence plates we want to recover.
2. **Candidate generation** – Nested loops iterate over every combination of 26 letters × 26 letters × 26 letters × 10 digits × 10 digits × 10 digits × 10 digits, producing strings such as `ABC-1234`.
3. **Hashing** – Each candidate string is encoded to bytes and hashed with Python's built-in `hashlib.sha256`.
4. **Matching** – If the uppercase hex digest (prefixed with `0x`) matches any target hash, the original plate is printed to the console.

This mirrors the real-world rainbow-table / brute-force approach attackers use against weakly hashed data, and shows why salting and strong key-derivation functions (bcrypt, Argon2, etc.) are essential.

---

## Requirements

- Python 3.x (no third-party libraries required – only the standard `hashlib` module is used)

---

## Usage

```bash
# Clone the repository
git clone https://github.com/buchanan97/Ethical-hacking-encryption-schemes.git
cd Ethical-hacking-encryption-schemes

# Run the brute-force hash attack
python3 "Brute force hash attack"
```

When a candidate plate whose SHA-256 hash matches a target is found, it is printed:

```
Match found: ABC-1234 -> 0x<SHA256_HASH>
```

> **Note:** The search space is 26³ × 10⁴ = **175,760,000** combinations. Depending on your hardware this may take several minutes to complete.

---

## Project Structure

```
Ethical-hacking-encryption-schemes/
├── Brute force hash attack   # Main Python script
└── README.md
```

---

## Disclaimer

This project is intended purely for **educational purposes** within an ethical hacking / cybersecurity curriculum. Do not use these techniques against systems or data you do not own or have explicit permission to test.
