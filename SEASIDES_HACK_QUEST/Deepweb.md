# [Deepweb](https://defhawk.com/battleground/raid/seasides25/deepweb)

[Author: @frey] CipherShade, a notorious dark web figure operating under the alias **zday0x01**, runs a hidden service called *The Whisper Vault*. While claiming anonymity, CipherShade’s arrogance leads to breadcrumbs being scattered across the internet. Participants must uncover CipherShade's real identity through OSINT techniques and link their activities across Twitter, GitHub, and HackerOne.

---

## Storyline and Challenge Breakdown

### **Step 1: The Entry Point – The .onion Site**
#### **Clues on the Site**:
- The homepage introduces *The Whisper Vault*, a platform offering “data recovery” and cryptocurrency laundering services.  
- Breadcrumbs include:
  - A hidden comment in the HTML source:
    ```html
    <!-- Project update: Check zday0x01 repo for WhisperVault_module -->
    ```
  - A blog post mentioning recent updates tweeted by `@_zday0x01_Dark`.
  - A Bitcoin testnet wallet ID displayed for payments (participants can trace it later).

---

### **Step 2: The Twitter Account (@_zday0x01_Dark)**
#### **Clues on Twitter**:
The Twitter profile is cryptic and taunting, featuring posts like:
- “Only shadows can whisper. Follow the trail if you dare. #Zday.”
- A pinned tweet with a blurred image of a cryptocurrency transaction receipt, containing subtle metadata pointing to the GitHub repo.
- Posts referring to "test modules" and linking back to the .onion site or GitHub.

#### **Hidden Clues**:
- The photo metadata includes GPS coordinates of a coffee shop in a specific city.  
- A post cryptically mentioning "James" or "JGatter," hinting at the HackerOne profile.

---

### **Step 3: The GitHub Account (zday0x01)**
#### **Clues in the Repo**:
- A public repository titled **"WhisperVault_module"**, claiming to be a privacy-enhancing tool.
- Commit messages like:
  ```
  Fixed login module - zday0x01 @protonmail.com
  ```
- An issue logged by a user suggesting:
  > “Why not submit this on HackerOne? You already have creds there, James.”
- A README file linking back to the .onion service and containing a testnet Bitcoin wallet address.

---

### **Step 4: The HackerOne Account (james-gatter)**
#### **Clues**:
- CipherShade's HackerOne profile includes a trail of ego-driven submissions.
- A disclosed bug titled *"VaultSec Exploit"* lists the .onion URL as the vulnerable target.
- The bio contains the phrase:
  > "Always testing boundaries, just like zday0x01."

---

## Steps to Solve the Challenge

### **Step 1: Analyze the .onion Site**
- Participants visit the site and uncover:
  - The HTML comment pointing to the GitHub repository.
  - Mentions of a Twitter account and a Bitcoin address.

### **Step 2: Investigate Twitter (@_zday0x01_Dark)**
- Participants:
  - Extract metadata from the blurred image, finding GPS coordinates.
  - Correlate the username `_zday0x01_Dark` with the GitHub handle, leading to the next step.

### **Step 3: Explore GitHub (zday0x01)**
- Participants analyze:
  - The commit history to uncover the email `zday0x01@protonmail.com`.
  - A suggestion in the issues log pointing to HackerOne.

### **Step 4: Examine HackerOne (james-gatter)**
- Participants link:
  - The bio and disclosed reports to the .onion site.
  - The username "James Gatter" to a real-world identity, completing the challenge.

---

## Challenge Complexity

### **Red Herrings**:
- Include false Bitcoin wallet IDs on the .onion site.
- Use unrelated commits in the GitHub repository to mislead participants.

### **Metadata Layers**:
- Embed GPS coordinates or timestamps in tweets and images.
- Use EXIF metadata to lead participants to a city or location.

### **Blockchain Puzzle**:
- Simulate transactions using testnet wallets that participants must trace for connections.

---

## Victory Conditions
- CipherShade's real identity is revealed as **James Gatter**, confirmed by cross-referencing GitHub, HackerOne, and Twitter profiles.
