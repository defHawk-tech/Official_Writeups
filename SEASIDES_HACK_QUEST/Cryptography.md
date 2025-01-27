# [Bruteone](https://defhawk.com/battleground/practice-lab/bruteone)
[Author: Pratham]

This write-up provides a step-by-step guide to solve the **Bruteone** challenge hosted at [DefHawk's Battleground](https://defhawk.com/battleground/practice-lab/bruteone). The goal is to decrypt a 41-character flag.

---

## **Challenge Description**

The challenge revolves around decrypting a file named `output`, which contains a SHA-256 hash for each letter of a plaintext string. Each line in the file corresponds to the hash of one character. Your task is to reverse the process and retrieve the original plaintext (flag).

---

## **Solution**

The following Python code is provided to solve the challenge:

### Code Breakdown:
```python
# Encrypt the output letter by letter
import hashlib

# Function to simulate the encryption process
def encrypt(plaintext: str):
    return "\n".join(hashlib.sha256(plaintext[i].encode()).hexdigest() for i in range(len(plaintext)))

if __name__ == "__main__":
    message = ""
    # Open the output file containing the hashes
    with open('output', 'r') as f:
        cipher = f.read().split("\n")
        # Iterate through each hash in the file
        for c in cipher:
            # Brute force all possible ASCII characters (0-127)
            for i in range(128):
                if hashlib.sha256(chr(i).encode()).hexdigest() == c:
                    message += chr(i)
                    break
    print(message)
```

# [Cycle](https://defhawk.com/battleground/practice-lab/cycle)
[Author: Pratham]

## Description

A mysterious file named `output.txt` has surfaced, containing random binary strings paired with encoded values. Cryptographic experts suspect these values were generated using a Cyclic Check (CRC) operation. Your objective is to crack the encoding scheme and extract the hidden flag embedded in the CRC results. The flag follows the standard CTF format: `HACK_QUEST{...}`.

#### Solution Approach:

1. **Cyclic Redundancy Check (CRC)**:
   The CRC operation works by appending a series of zeros to the data and then applying a polynomial, which is a series of binary values. This process is repeated until the remainder is computed, which forms the CRC value.

2. **Reversing the CRC**:
   The goal is to identify the polynomial (in the range of printable ASCII characters) used to generate the CRC for each binary string. Once you identify the polynomial, you can use it to build the flag.

#### Solution Code:

The provided Python solution uses the following approach to crack the encoding:

```python
def calculate_crc(data: str, polynomial: chr) -> str:
    polynomial = list(bin(ord(polynomial))[2:])
    data = list(data)
    polynomial = list(polynomial)
    # Append 0s to the binary string
    data += ['0'] * (len(polynomial) - 1)
    # Perform XOR operation
    for i in range(len(data) - len(polynomial) + 1):
        if data[i] == '1':
            for j in range(len(polynomial)):
                data[i + j] = str(int(data[i + j]) ^ int(polynomial[j]))
    return ''.join(data[-len(polynomial) + 1:])

f = open("output.txt", "r")
lines = f.readlines()
f.close()
flag = ""
for line in lines:
    binary_string, crc = line.split()
    for i in range(65, 128):
        if calculate_crc(binary_string, chr(i)) == crc:
            flag += chr(i)
            break
print(flag)
```
