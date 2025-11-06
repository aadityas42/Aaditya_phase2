# 1. JoyDivision

## Solution:

- I put the elf file into dogbolt to analyse it and found that the following things are happening:
- There are only 2 function that modify the flag buffer which are called in main.
- The "flipBits" function is called.
- The "expand" function is being called 3 times.
- So, if we want the flag, we need to perform these operations in the reverse order on the flag.txt file.
- So, we can write a python script to carry these out.
  
The script is
```
from pathlib import Path
def inv_expand(data):
    out = bytearray(len(data)//2)
    toggle = False
    for i in range(len(out)):
        o0, o1 = data[2*i], data[2*i+1]
        if not toggle:
            out[i] = (o1 & 0xF0) | (o0 & 0x0F)
        else:
            out[i] = (o0 & 0xF0) | (o1 & 0x0F)
        toggle = not toggle
    return bytes(out)

def inv_flip(data):
    out = bytearray(len(data))
    kcount = 0
    for i, v in enumerate(data):
        if i % 2 == 0:
            out[i] = (~v) & 0xFF
        else:
            key = (0x69 + 0x20*kcount) & 0xFF
            out[i] = v ^ key
            kcount += 1
    return bytes(out)

b = Path("flag.txt").read_bytes()

x = inv_expand(b)
x = inv_expand(x)
x = inv_expand(x)
plain = inv_flip(x)
print(plain.decode(errors="replace"))
```
from this, we get the output:
```
PS C:\Users\Aaditya> python joyD.py
sunshine{C3A5ER_CR055ED_TH3_RUB1C0N}
```

## Flag:

```
sunshine{C3A5ER_CR055ED_TH3_RUB1C0N}
```

## Concepts learnt:

- I learnt how to analyse elf files in dogbolt
- I also learnt how to write a script to reverse operations including those like XOR.

## Notes:

- Nil.

## Resources:

- Nil.
  

