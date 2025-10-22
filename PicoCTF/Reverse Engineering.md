# 1. GDB baby step 1

Can you figure out what is in the eax register at the end of the main function? Put your answer in the picoCTF flag format: picoCTF{n} where n is the contents of the eax register in the decimal number base. If the answer was 0x11 your flag would be picoCTF{17}. Disassemble this.> Put in the challenge's description here

## Solution:

First, since we are told to disassemble the file given, I used dogbolt to decompile it.

Next, we are told to look for the eax register at the end of the main function, so I first located main.

Next, I looked up what exactly an eax register is, and found out that it is a 32 bit register which is used as the accumulator for arithmetic and logical operations, so it also returns a value.

<img width="584" height="672" alt="image" src="https://github.com/user-attachments/assets/cdd1fea8-9776-4cad-a8c7-e848b8856103" />

I took the value given in the return function, which is easily identifiable as a hex value, and converted it to decimal.

Finally, I wrapped the answer we get on conversion that is '549698' into t he picoctf form to get our flag.

## Flag:

```
picoCTF{549698}
```

## Concepts learnt:

- I learned how to use a decompiler to get the code of an executable file using dogbolt, which employees various decompilation softwares which we can choose from.
- I also learned how to analyse the code to extract specific information that may be useful to us.

## Notes:

- Didn't really go on any tangents on this one since it was fairly simple
- The closest to an 'alternate' solution I found was to use angr instead of BinaryNinja, which directly gives us th e decimal value, saving us the effort of converting it ourselves.

## Resources:

- Nil.


***

# 2. Challenge name

> Description

.
.
.

