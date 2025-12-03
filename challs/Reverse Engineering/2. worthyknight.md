# 2. worthy.knight

## Solution:

- I put the in dogbolt and used ghidra to analyse it.
- I started by going to the conditions for the inputs of the 5 sets of 2 pair of charecters to try and decode what the input should be.
- The first part we have is
  
```
if ((byte)(local_c8[1] ^ local_c8[0]) == 0x24) {
        if (local_c8[1] == 0x6a) {
```
Since XOR is reversible, this is basically 0x6a^0x24 which gives us 0x4e, or 'N' which is the 0th char.
The 1st char is stated to be 0x6a or 'j'.
This makes our first pair 'Nj'

-The second pair

```
if ((local_c8[2] ^ local_c8[3]) == 0x38) {
            if (local_c8[3] == 0x53) {
```
again, this is 0x53^0x38 which is 0x6b or 'k' which is char 2
char 3 is 0x53 or 'S'
the pair is hence 'kS'

- The third pair
  for this we are given an MD5 hash `33a3192ba92b5a4803c9a9ed70ea5a9c` which translates to 'Tf'

- The fourth pair
  ```
   if ((local_c8[6] ^ local_c8[7]) == 0x38) {
                  if (local_c8[7] == 0x61) {
  ```
  this is 0x61^0x38 which is 0x59 or 'Y'
  0x61 is 'a'
  so the pair is 'Ya'

- The fifth pair
  ```
  if ((byte)(local_c8[9] ^ local_c8[8]) == 0x20) {
                      if (local_c8[9] == 0x69) {
  ```
  this is 0x69^ox20 which is 0x49 or 'I' which is the 8th char.
  the 9th char is 0x69 or 'i'
  so the pair is 'Ii'.

  so, putting these together, and the flag format 'KCTF{}' indicated by the decompiled code, we get our final flag as `KCTF{NjkSTfYaIi}`
## Flag:

```
KCTF{NjkSTfYaIi}
```

## Concepts learnt:

- I got better at analysing code in ghidra
- Some practice with XOR

## Notes:

- At first since it was a knight type file, I looked up what that type of file is and what it does, and thought we had to go down that route by decrypting the file since a knight would typically encrypt it and hide files from us.

## Resources:

- [what is knight](https://www.fortinet.com/blog/threat-research/ransomware-roundup-knight)
- [dogbolt](https://dogbolt.org)  
