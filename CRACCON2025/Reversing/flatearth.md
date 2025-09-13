# flatearth

The challenge is a crackme that uses control flow flattening for obfuscation. Here's a portion of the pseudocode for the relevant function, extracted from IDA Pro:

```c
_BOOL8 __fastcall check_flag(char *a1)
{
  char v2; // [rsp+1Bh] [rbp-5h]
  int v3; // [rsp+1Ch] [rbp-4h]

  if ( strlen(a1) != 31 )
    return 0;
  v2 = 66;
  v3 = 9493;
  while ( v3 && v3 != -1 )
  {
    switch ( v3 )
    {
```

## Control Flow Flattening

Control flow flattening is an obfuscation technique where the natural control of a program (if/else, loops) are turned a 'state machine' defined with a large switch statement inside a loop. This loop is controlled by a state variable that defines which case to execute next. While this is semantically equivalent to using loops or nested if/else statements, it becomes harder to analyze.

In this challenge, the `check_flag` the variable `v3` is the state variable, initially set to `9493`. The loop continues until `v3` is indicating success, or -1 indicating failure. Each `case` block contains a condition that checks a character of the flag. If the condition is true, `v3` is updated to the next state. If it's false, `v3` is set to -1, and the loop terminates.

## Deobfuscation

To solve this, we need to reconstruct the original control flow. We can do this by following the state transitions until we reach the success state. This script parses the decompiler output (with a few edge cases handled), finds the next state for each state, and prints them in the correct order.

Here is the script:

```python
import re


def parse_case_blocks(code: str):
    case_pattern = re.compile(r'(case (\d+)|default):\s*(.*?)break;', re.DOTALL)
    if_pattern = re.compile(r'if\s*\((.*?)\)\s*{.*?v3\s*=\s*(-?\d+);', re.DOTALL)

    case_dict = {}
    default_value = None

    for match in case_pattern.finditer(code):
        label_number = match.group(2)
        block = match.group(3).strip()

        if_match = if_pattern.search(block)
        condition = if_match.group(1).strip()
        v3_value = int(if_match.group(2))
        if label_number is not None:
            case_label = int(label_number)
            case_dict[case_label] = (condition, v3_value if case_label != 30958 else 31615) # weird edge case in the decompilation
        else:
            default_value = (condition, v3_value)
            
    return case_dict, default_value


with open('pseudocode.c', 'r') as f:
    code = f.read()


case_dict, default = parse_case_blocks(code)
print(len(case_dict))
print(default)


v3 = 9493

while v3 != 0:
    if v3 not in case_dict:
        condition, next_v3 = default
    else:
        condition, next_v3 = case_dict[v3]        
    print(f"{v3}: {condition}")
    v3 = next_v3
```

The script starts with `v3 = 9493` and follows the chain of `v3` values, printing the condition at each step. The output of this script is a sequence of conditions on the flag characters.

## Solving

The conditions obtained from the deobfuscation script are a system of equations. Each condition involves one character of the flag and the value of `v2` (initially `0x42`) from the previous step. Since the conditions are sequential, we must solve them one by one. We can use a solver like Z3 to find the flag that satisfies all the conditions.
