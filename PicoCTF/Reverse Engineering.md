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

# 2. ARMssembly 1

For what argument does this program print `win` with variables 58, 2 and 3? File: chall_1.S Flag format: picoCTF{XXXXXXXX} -> (hex, lowercase, no 0x, and 32 bits. ex. 5614267 would be picoCTF{0055aabb})

## Solution:

First, I opened up the assembly code in notepad and analysed it, trying to locate the variables 58,2 and 3, since those are the variables we are looking for.

The first three lines I identified were of the format `mov w0, 58` , which basically move the values 58, 2 and 3 into the w0 registers.

The second line in this function is responsible for taking our input.

The next key line is `lsl w0, w1, w0` which stands for logical shift left. It shifts the bits of the binary representation of the stored number by 2 places to the left.
58's binary representation shifted left by 2 places gives us 232 in decimal.

The next instruction of use, is `sdiv w0, w1, w0`  which does w1/w0 and stores it in w0. Since the last number to be loaded into w0 was 3, this gives us 232/3 which has an integer value of 77.

<img width="407" height="733" alt="image" src="https://github.com/user-attachments/assets/7e151743-5cd4-4c97-878a-aa430f83a718" />

Next, `sub w0, w1, w0` does w1-w0 and stores the result in w0. This is basically 77- our input, and returns it to the main function.

<img width="395" height="394" alt="image" src="https://github.com/user-attachments/assets/49d15ba7-2873-40e1-8d44-a7153a6a9118" />

The condition for win to be printed is for the 'func' function to return 0 as it's value.

Hence, we can deduce that the input we need to provide is 77.
77 in hex is 0000004D. Wrapping this in flag format gives us the correct flag.

## Flag:

```
picoCTF{0000004d}
```

## Concepts learnt:

- I learned how to analyse assembly code and discard it's unnecessary parts, only focusing on what's useful to us.
- Since I already had some knowledge of assembly, it was a good revision of reading assembly code.

## Notes:

- At first, I tried entering just `4D` inside the flag format, not realising that I have to add 0s at the start since it is in 32bit.
  
## Resources:

- Nil.


***


# 3. Valut-door-3

This vault uses for-loops and byte arrays. The source code for this vault is here: VaultDoor3.java

## Solution:

I opened the given script in VSCode to analyse it, and found that the program is basically taking an input, and then using 4 for loops to scramble it, and checking it against a given string.
Our task is to find out what should be the input, knowing what the scrambled string looks like.

We can see in the code that it expects us to give an input in the picoCTF{} form and ignores that part while scrambling.

The program needs the vault password to be 32 charecters long.

In the first for loop, the first 8(0-7) charecters are copied as is, without any changes.

In the second for loop, the charecters from 8-15, are copied in the reverse order, which basically means that we need to replace the 8 with 15, 9 with 14...and so on.

The third for loop is for scrambling the even positions between 16-31, this basically swaps the even positions in a manner similar to the second loop, i.e. 16 gets replaced with 30, 18 with 28.. and so on, for all even positions in the given range.

The fourth for loop is for copying the odd positions between 16-31 as is, without any changes.


<img width="854" height="998" alt="image" src="https://github.com/user-attachments/assets/265a1874-e0c7-4f72-80cd-ec17b527c8e1" />


If we apply all of the above operations to the scrambled string `jU5t_a_sna_3lpm18g947_u_4_m9r54f` given in the code, we get a string `jU5t_a_s1mpl3_an4gr4m_4_u_79958f`
Wrapping this in the picoCTF{} format, we get our flag.


## Flag:

```
picoCTF{jU5t_a_s1mpl3_an4gr4m_4_u_79958f}
```

## Concepts learnt:

- I learnt how to reverse engineer the logic behind a for loop, where given the end result, we need to go backwards and deconstruct the process.

## Notes:

-I didn't go on any tangents per say, but initially I thought we have to get the input by changing the parameters of the for loop so that we could do it purely using the code, but then I realised that since it is only a 32 charecter string, in 4 chunks of 8 characters, understanding the logic and doing it manually will be faster.

## Resources:

- Nil.
