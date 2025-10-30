# 1. trivial flag transfer protocol

Figure out how they moved the flag.

## Solution:

- 



## Flag:

```

```

## Concepts learnt:

-  

## Notes:

- Nil.

## Resources:

- 
  

***


# 2. tunn3l v1s10n

We found this file. Recover the flag.

## Solution:

- First, I opened the image in a hex editor and noticed that it is a bitmap file. But, saving it with a .bmp extension doesn't get us anywhere.

- So, I tried comparing it's hex with that of how a typical bitmap image should look like, and found multiple discrepancies in it.
- First, at an offset of 14 bytes, where the first 4 bytes which display the size of the infoheader should be 28 00 00 00.
  <img width="785" height="200" alt="image" src="https://github.com/user-attachments/assets/0a84636d-9c34-4c98-aaf1-b1473ce8f95c" />

- After making this change, I saved it as a bmp and got the following image:
  <img width="1126" height="299" alt="image" src="https://github.com/user-attachments/assets/27a8e6fe-fc14-4ddc-9780-5fdf83da58b0" />

- So, we know that we're doing something right.
- Next, I obsereved that just before this, where it should be 36 00 00 00, there was BA D0 00 00. So I changed this too.
- Next, I tried adjusting the dimensions of the image, and played around with the height and width. Changing the width didn't help much, but changing the width from 32 01 00 00 to 32 03 00  00. I saved the image again as a bmp, and got the following image that contains the flag.
<img width="1411" height="697" alt="image" src="https://github.com/user-attachments/assets/30fcad57-0add-48f8-8a7c-0795ac776ae4" />

## Flag:

```
picoCTF{qu1t3_a_v13w_2020}
```

## Concepts learnt:

- I learned how to edit the hex code of a file, and I learnt how to compare the hex of a given file with a standard that it should follow, and how to utilise this knowledge to make files viewable.

##Notes:

- Nil.

## Resources:

- [For general format of BMP](https://www.ece.ualberta.ca/~elliott/ee552/studentAppNotes/2003_w/misc/bmp_file_format/bmp_file_format.htm)


***


# 3. m00nwalk

Decode this message from the moon.

## Solution:

- I used the hint to find out which signal was used to transfer the pictures from the moon back to earth, which I found out to be slow-scan television signals(SSTV).

- After this, I simply used a SSTV decoder to decode the .wav file into an image, which looked like this:

  <img width="421" height="333" alt="image" src="https://github.com/user-attachments/assets/95e92c1e-cf7d-4608-9ca2-69cb91835229" />

We can flip the image and get the flag from here itself.

## Flag:

```
picoCTF{beep_boop_im_in_space}
```

## Concepts learnt:

- I learnt what SSTV signals are, and that they can encode images inside audio signals.
  
## Notes:

-Nil.

## Resources:

- [Dedoder](https://sstv-decoder.mathieurenaud.fr/)
