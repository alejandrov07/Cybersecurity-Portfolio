# Cryptographic Analysis, Data Recovery, and File Integrity Verification

## Security Insight
From a defensive and incident response perspective, evaluating the strength of cryptographic controls and maintaining data integrity are foundational to securing enterprise infrastructures. Cyber adversaries frequently exploit weak, legacy encryption mechanisms or attempt to masquerade malicious payloads by minorly altering source code (e.g., injecting non-printable characters or null bytes into standard binaries like the EICAR anti-virus test string) to bypass static signature-based detection systems. 

Relying solely on visual inspection utilities like cat or strings creates a critical operational blind spot, as altered binaries can appear completely identical to legitimate ones. Because cryptographic hash functions possess the avalanche effect—where a single-bit modification yields a completely unpredictable digest—manually calculating SHA-256 values ensures absolute data validation. Furthermore, understanding legacy cipher vulnerabilities (such as frequency analysis or brute-forcing) allows analysts to perform reverse engineering during data recovery, while enforcing robust, industry-standard protocols like AES-256-CBC with PBKDF2 guarantees that exfiltrated assets remain computationally infeasible to decrypt without authorized keys.

## Technical Execution: Data Recovery & Decryption

### Phase 1: Environment Exploration and Hidden Artifact Discovery
The initial assessment of the compromised directory structure requires identifying both visible configuration files and hidden operational scripts or metadata.

# List directory contents to identify encrypted files and subdirectories
analyst@sec-ops-lab:~$ ls -l

# Navigate to the target subdirectory and expose hidden administrative files
analyst@sec-ops-lab:~$ cd caesar
analyst@sec-ops-lab:~/caesar$ ls -a
.  ..  .leftShift3  Q1.encrypted

### Phase 2: Decrypting Legacy Substitutions (Caesar Cipher)
The hidden file .leftShift3 utilizes a monoalphabetic substitution cipher with a fixed shift of 3 positions. To bypass this control and pipe the plaintext instructions, a character-set translation is executed via standard Linux streams.

# Translate the cipher text by mapping the shifted alphabet back to standard positions
analyst@sec-ops-lab:~/caesar$ cat .leftShift3 | tr "d-za-cD-ZA-C" "a-zA-Z"

### Phase 3: Symmetric Decryption via OpenSSL (AES-256-CBC)
Using the key recovered from the baseline decryption phase (ettubrute), the primary encrypted data payload (Q1.encrypted) is processed using the Advanced Encryption Standard (AES) in Cipher Block Chaining (CBC) mode, utilizing a Password-Based Key Derivation Function 2 (PBKDF2) to mitigate credential-stuffing vulnerabilities.

# Decrypt the primary payload and output the verified plaintext structure
analyst@sec-ops-lab:~/caesar$ openssl aes-256-cbc -pbkdf2 -a -d -in Q1.encrypted -out Q1.recovered -k ettubrute

# Verify the integrity and read the contents of the recovered asset
analyst@sec-ops-lab:~/caesar$ cat Q1.recovered

## Technical Execution: File Integrity Verification (Hashing)

### Phase 4: Environment Verification and Visual Telemetry
Moving to a separate directory sector to investigate two configuration payloads (file1.txt and file2.txt) flagged by security alerts. Visual inspection through the terminal interface shows identical outputs, demonstrating how easily a malicious file can mimic a secure one.

# List target files to check metadata constraints
analyst@sec-ops-lab:~$ ls -la
-rw-r--r-- 1 analyst analyst   69 May 18 11:00 file1.txt
-rw-r--r-- 1 analyst analyst   70 May 18 11:00 file2.txt

# Inspect file contents using standard output utilities
analyst@sec-ops-lab:~$ cat file1.txt
X5O!P%@AP[4\PZX54(P^)7CC)7}$EICAR-STANDARD-ANTIVIRUS-TEST-FILE!$H+H*

analyst@sec-ops-lab:~$ cat file2.txt
X5O!P%@AP[4\PZX54(P^)7CC)7}$EICAR-STANDARD-ANTIVIRUS-TEST-FILE!$H+H*

### Phase 5: Cryptographic Hash Generation and Comparison
Computing SHA-256 digests reveals the underlying structural divergence of the assets. Even though they look identical to the naked eye, their Unique Cryptographic IDs (hashes) prove they are completely different files.

# Compute SHA-256 hashes to expose actual file signatures
analyst@sec-ops-lab:~$ sha256sum file1.txt
131f95c51cc819465fa1797f6ccacf9d494aaaff46fa3eac73ae63ffbdfd8267  file1.txt

analyst@sec-ops-lab:~$ sha256sum file2.txt
2558ba9a4cad1e69804ce03aa2a029526179a91a5e38cb723320e83af9ca017b  file2.txt

# Serialize hashes to separate files and perform a byte-level differential analysis
analyst@sec-ops-lab:~$ sha256sum file1.txt >> file1hash
analyst@sec-ops-lab:~$ sha256sum file2.txt >> file2hash
analyst@sec-ops-lab:~$ cmp file1hash file2hash
file1hash file2hash differ: char 1, line 1

## Analysis Conclusion
The system successfully isolated structural modification at the binary layer. Relying on visual tools like cat is unsafe for verification; mathematical confirmation through cryptographic hashing (sha256sum) and automation remains the only reliable methodology to prove file tampering in production systems.

---
*This project is part of my cybersecurity portfolio [Blue Team Operations & Infrastructure Defense]*
