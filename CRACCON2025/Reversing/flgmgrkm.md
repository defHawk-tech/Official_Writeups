# FlgMgrKm

The challenge provides a live kernel dump file from a Windows system, along with a PDB file. According to the description, the flag is likely stored within a kernel-mode driver.

To solve the challenge, we start by opening the dump file in WinDbg and loading the provided PDB. Running the `lm` command lists all loaded drivers, where we can identify one named `FlgMgrKm`. Using the command `x FlgMgrKm!*`, we list all symbols exported by the driver. Among them, two stand out: an array named `flag` and a function called `DecryptFlag`.

Inspecting the disassembly of `DecryptFlag`, we find that it performs a simple XOR-based decryption. We can reverse this logic in Python using the following snippet:

```python
''.join([chr((a ^ 0x69) + i) for i, a in enumerate(flag_bytes)])
```

Running this script on the contents of the `flag` array yields the final flag:

```
craccon{fl46_dr1v3r_k3rn3l_m0d3}
```
