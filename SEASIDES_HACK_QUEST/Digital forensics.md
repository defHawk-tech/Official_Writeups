# [Follow the Traces](https://defhawk.com/battleground/practice-lab/follow-the-traces)

## Challenge Overview

Dive into the world of log analysis and uncover hidden threats! In this challenge, participants will analyze real-world system logs to identify suspicious activities, trace potential breaches, and unravel attack vectors. The goal is to reconstruct the flag hidden within the logs.

### Flag Format: 
```
HACK_QUEST{...}
```

## Observation and Analysis

The challenge provided a file containing sensitive information split into three parts. Upon careful analysis, it was determined that the split data could be reconstructed to form the complete flag. This suggested that sensitive information was unintentionally exposed within the file, making it susceptible to attack.

## Approach Taken

### 1. File Analysis:
- Thoroughly examined the provided file to identify any hidden or exposed information.
- Discover that the flag was divided into three parts, spread across different sections of the file.

<img width="358" alt="image" src="https://github.com/user-attachments/assets/e91a234b-99fa-4277-aa46-cf4400a5bd38" />

### 2. Reconstructing the Flag:
- By analyzing the file and extracting the fragments, the three parts of the flag were identified and combined to form the complete flag.

### 3. Conclusion

The challenge highlighted the importance of log analysis and data reconstruction in identifying security vulnerabilities. By examining real-world system logs and uncovering hidden fragments of sensitive information, participants were able to trace the attack vector and recover the flag. This exercise emphasized how unintentional data exposure could lead to potential threats, such as ransomware attacks.
