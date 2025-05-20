# Ransomware Script (Educational Use Only)

## ⚠️ Disclaimer

> This tool is designed **strictly for educational, research, and cybersecurity training purposes**. Unauthorized deployment on systems you do not own or operate with explicit consent is illegal and unethical. Use in sandboxed environments only.

---

## Introduction

Ransomware continues to be one of the most dangerous cyber threats in the world. Understanding its core mechanisms file encryption, key concealment, and user coercion is essential for defenders, educators, and ethical hackers.


---


## Architecture & Design

### System Overview

The system consists of the following core modules:

1. **Encryption Module**  
   - Encrypts files in **1MB chunks**
   - Applies **PKCS#7** padding
   - Outputs to `.encblob` encrypted files

2. **Key Management**  
   - Generates 32-byte AES key + 16-byte IV  
   - Obfuscates using **rotating XOR**
   - Stores inside a **10MB dummy file** at 1MB offset

3. **Integrity Verification**  
   - Computes and stores **SHA-256** hash of entire folder  
   - Verifies hash post-decryption to ensure **zero data loss**

4. **GUI Interaction**  
   - **Ransom Note GUI** for simulated decryption workflow  
   - **Post-Encryption Warning GUI** with countdown and alerts

5. **Packaging & Execution**  
   - Built into a **standalone executable** using PyInstaller  
   - Hidden console, bundled assets, and clean UI

---

## Encryption Details

- **Algorithm**: AES-256 in CBC mode using `cryptography` library
- **Key**: 32 bytes (256 bits)
- **IV**: 16 bytes (128 bits)
- **Chunk size**: 1MB
- **Padding**: PKCS#7 (streamed and finalized at EOF)

---

## Key and Hash Obfuscation

- Keys are stored in `.hidden_ransom_key.txt`, padded to 10MB  
- Obfuscated via **rotating XOR byte mask**
- SHA-256 hash of original content stored in `.hidden_ransom_hash.txt` at 2MB offset
- Both hidden files simulate stealthy key retention

---

## Payment Simulation (GUI-based)

The decryption workflow simulates a ransomware attack:
- A **ransom note GUI** pops up for `.encblob` files
- Options include:  
  - Enter a decryption key manually  
  - Click **“Check Payment”** to simulate a ransom payment
- Upon successful simulation:
  - Decryption process starts

---

## Packaging with PyInstaller

The script is bundled into a single `.exe` using the following command:

```bash
pyinstaller --onefile --name "FileSecurityDemo" --distpath "../dist" --workpath "../build" \
  --add-data "../assets/images.ico;assets" --hidden-import cryptography --noconsole --clean main.py
