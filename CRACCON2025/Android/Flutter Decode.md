**Chall Name** :- Flutter Decode

**Solution**

The flag check is basically base64 decoding the hardcoded b64 encoded flag in the apk. Using `blutter`, and then reading the main.dart will give the candidates an idea that base64 decoding is occuring. Further based on the flag format, they can find the hardcoded string in the folder, specifically `pp.txt`.
