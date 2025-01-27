# Reverse Engineering Challenges

## 1. [String Cipher](https://defhawk.com/battleground/practice-lab/string-cipher)

### Description
Dive into the basics of reverse engineering by unraveling a simple yet intriguing puzzle. Analyze the provided binary or script to reveal a hidden string. This challenge tests your ability to deobfuscate and understand basic reversing concepts.  
**Click on the [Play Challenge](#) link to download the file.**

### Steps to Solve

<img width="914" alt="image" src="https://github.com/user-attachments/assets/aab907a7-98b1-41d8-8f1c-8e20cc876e99" />

1. Use `objdump` or `gdb` on Kali Linux to analyze the binary:
   objdump -s -j .rodata rev
   <img width="1179" alt="image" src="https://github.com/user-attachments/assets/bca1128e-b381-49ea-b2d7-593e79ca7cf1" />

2. Exract the ASCII strings.
3. Analyze and decode to get flag.

## 2. [Obfuscation Maze](https://defhawk.com/battleground/practice-lab/Obfuscation-Maze)

### Description
Step into a complex world of obfuscation and clever disguises. Your task is to reverse-engineer a file that combines obfuscation techniques with subtle tricks. Unravel the layers and decode the hidden logic to find the solution. Click on Play challenge to find the binary.

### Steps to Solve
1. Use IDA/Ghidra/radare2 to disassemble the ELF file (rev2).
2. Extract the Obfuscated string.
";\\n!'04& '0\\n'0#04901("![image](https://github.com/user-attachments/assets/31941173-7bd1-40dd-8fa9-78f1613675a8)
3. Analyze and reverse the code to confirm the XOR operation with key = 85

   <img width="782" alt="image" src="https://github.com/user-attachments/assets/d04e9a9a-29f8-4ccc-92e5-bdd0d0bcee35" />

5. Decode the encoded string and find the flag

```
v1 = hex(64058364944535178947612915434603811869)[2:]  # Convert to hex and remove 0x
v2 = ";\\n!'04& '0\\n'0#04901("
key = 85
tmp = []

# XOR the hex values from v1
for i in range(0, len(v1), 2):
    tmp.append(chr(int(v1[i:i+2], 16) ^ key))  # Extract each hex char, convert to int, and XOR

tmp = tmp[::-1]  # Reverse the list

# XOR the characters from v2
for i in v2:
    tmp.append(chr(ord(i) ^ key))  # XOR each character with the key

# Join the result into a single string
result = "".join(tmp)
result
 ```


