# [NotthatEasy](https://defhawk.com/battleground/raid/seasides25/notthateasy) : Steganography Challenge

[Author: @rcspell] This write-up explains the step-by-step process to solve the steganography challenge and retrieve the hidden flag from two images.

<img width="976" alt="image" src="https://github.com/user-attachments/assets/c2cee155-f6e3-4dc8-ba70-16c9e74cb61c" />

---

## **Step 1: Extract Metadata from Image 1**

1. Use `exiftool` to extract metadata from **IMG_CRAC.JPG**:
   ```bash
   exiftool IMG_CRAC.JPG
   ```
2. Look for any comments or metadata fields containing encoded data. In this case, you’ll find:
   ```
   Comment: MTAxaGFja2luZw==
   ```
3. Decode the base64 string using any decoding tool or command:
   ```bash
   echo "MTAxaGFja2luZw==" | base64 --decode
   ```
   Result:
   ```
   101hacking
   ```
   This is the password needed for the next step.

---

## **Step 2: Extract Hidden Data from Image 1**

1. Use the password `101hacking` with **steghide** to extract the embedded data from **IMG_CRAC.JPG**:
   ```bash
   steghide extract -sf IMG_CRAC.JPG -p 101hacking
   ```
2. If successful, you’ll retrieve a partial flag from `flag.txt`.

---

## **Step 3: Analyze Image 2**

1. Start by inspecting the metadata of **IMG2.JPG**:
   ```bash
   exiftool IMG2.JPG
   ```
   You’ll find a hint in the `Comment` field:
   ```
   Comment: NOT_THAT_DIRECT_try_some_other_ways_too
   ```
   This suggests there’s more hidden in the file.

2. Check if the image contains an appended file using **7-Zip** or the `file` command:
   ```bash
   file IMG2.JPG
   ```
   If the file contains a zip archive, proceed to the next step.

---

## **Step 4: Extract Hidden Data from Image 2**

1. Extract the embedded zip file from **IMG2.JPG** using the `copy` command:
   ```bash
   copy /b IMG2.JPG extracted.zip
   ```
2. Open the extracted zip file and retrieve `flag.txt`.

---

## **Step 5: Combine Partial Flags**

1. Open both `flag.txt` files retrieved from **IMG_CRAC.JPG** and **IMG2.JPG**.
2. Combine the partial flags to reconstruct the full flag.

---

## **Final Flag**

After following these steps, you’ll have successfully uncovered the complete flag. Congratulations!
