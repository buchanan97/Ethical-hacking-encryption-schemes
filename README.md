# Ethical Hacking Encryption Schemes

A Python demonstration of a **brute-force hash-reversal attack** applied to SHA-256 hashed licence plates. The project was built as an educational exercise to illustrate how weak, predictable input spaces can be reversed even with a cryptographically strong hash function like SHA-256.

---

## Table of Contents

1. [Project Overview](#project-overview)
2. [How It Works](#how-it-works)
3. [Code Walkthrough](#code-walkthrough)
4. [Requirements](#requirements)
5. [Running the Script](#running-the-script)
6. [Example Output](#example-output)
7. [Demo Video](#demo-video)
8. [Disclaimer](#disclaimer)

---

## Project Overview

The script in `Brute force hash attack` attempts to recover the original licence-plate strings behind a set of known SHA-256 hashes. Because licence plates follow a rigid, predictable format (`LLL-NNNN` – three uppercase letters followed by four digits), the total search space is small enough (26³ × 10⁴ = **175,760,000** combinations) to be exhausted in a reasonable amount of time on a standard machine.

---

## How It Works

1. **Target hashes are loaded** – six SHA-256 digests (stored as uppercase hex strings prefixed with `0x`) represent the "unknown" licence plates that need to be recovered.
2. **Every possible plate is generated** – nested `for` loops iterate over all combinations of three uppercase letters (`A–Z`) and four digits (`0–9`), producing strings in the form `ABC-1234`.
3. **Each plate is hashed** – Python's built-in `hashlib.sha256()` converts the plate string to its SHA-256 digest.
4. **The digest is compared** – if the computed hash matches any of the target hashes the plate is printed as a match.

This is a classic **dictionary / exhaustive search** technique. It succeeds here purely because the input space is tiny and fully enumerable; SHA-256 itself is not broken.

---

## Code Walkthrough

```python
import hashlib

def generate_hashes():
    # The six SHA-256 hashes we want to reverse
    target_hashes = {
        "0x31E6FA527BC58FDF...",
        ...
    }

    letters = "ABCDEFGHIJKLMNOPQRSTUVWXYZ"
    digits  = "0123456789"

    # Seven nested loops → every LLL-NNNN combination
    for l1 in letters:
      for l2 in letters:
        for l3 in letters:
          for d1 in digits:
            for d2 in digits:
              for d3 in digits:
                for d4 in digits:
                    plate = f"{l1}{l2}{l3}-{d1}{d2}{d3}{d4}"

                    # Compute SHA-256 and format identically to the target hashes
                    hash_object = hashlib.sha256(plate.encode())
                    hash_hex    = "0x" + hash_object.hexdigest().upper()

                    # Print if this plate produced one of the target hashes
                    if hash_hex in target_hashes:
                        print(f"Match found: {plate} -> {hash_hex}")

if __name__ == "__main__":
    generate_hashes()
```

### Key details

| Step | Detail |
|------|--------|
| Encoding | `plate.encode()` converts the string to bytes (UTF-8) before hashing |
| Case normalisation | `.upper()` ensures the computed hex string matches the uppercase targets |
| `0x` prefix | Added to both the target hashes and the computed hash for a consistent comparison |
| Data structure | `target_hashes` is a Python `set`, so each lookup is **O(1)** |

---

## Requirements

- Python **3.6+** (f-strings are used)
- No third-party libraries – only the standard-library `hashlib` module is needed

---

## Running the Script

```bash
# Navigate to the project folder
cd "Brute force hash attack"

# Run the script
python3 "Brute force hash attack"
```

> **Note:** The script iterates over ~175 million combinations. Depending on your hardware this may take several minutes. You will see output printed to the terminal as each match is found.

---

## Example Output

```
Match found: XYZ-1234 -> 0xD317C47574BBC4474DAF8AC434441124AFD91787F97E675489E19576CEE120C3
Match found: ...
```

---

## Demo Video

[Watch the demo video](https://www.loom.com/share/73477dc7e3f04dd1878a0962ea895bc6)

---

## Disclaimer

This project is intended **solely for educational purposes**. The techniques demonstrated here are used in legitimate security research and penetration testing courses to show why predictable input spaces must be avoided even when a strong hash algorithm is in use. Do not use this knowledge to access systems or data without explicit authorisation.