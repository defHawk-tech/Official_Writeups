**Chall Name** :- Easy RASP

**Solution**

There are some checks :- `root check`, `emulator check`, `frida check`
If any of the check fails, the app will crash when the submit button is pressed. 
The `confuse_and_map` function in the native side is well obfuascated and is basically performing `b64 decode`. 

Players can either guess that the algorithm is b64 and can find the hardcoded b64 encoded password in the binary or they have to write a frida script to bypass all the checks and get the password by hooking onto the function.

Password :- `letmein..`
