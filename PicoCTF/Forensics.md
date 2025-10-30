# 1. trivial flag transfer protocol

Figure out how they moved the flag.

## Solution:

- First, I opened the file using wireshark to analyse it. We can see TFTP traffic in the file, so I exported TFTP objects from the file and saved them.
- These files consist of 3 images, 1 txt file called plan, another called instructions, and a program.deb file.
- The contents of these files appear to be encrypted, so I used a decrypter to try and make some sense of the text, it turned out to be a ROT-13 cipher.
- The text from the plan files reads out the following after decryption: "I USED THE PROGRAM AND HID IT WITH-DUE DILIGENCE. CHECK OUT THE PHOTOS"
- The text from the instructions file reads out the following after decryption: "TFTP DOESNT ENCRYPT OUR TRAFFIC SO WE MUST DISGUISE OUR FLAG TRANSFER. FIGURE OUT A WAY TO HIDE THE FLAG AND I WILL CHECK BACK FOR THE PLAN"
- So now we know we should check out the photos, and also we still have the program.deb file, which gives us the hint that we need to use a tool called steghide.
- So, I installed steghide, and ran it for all the images using the passkey as "DUEDILIGENCE" from the hint in the plan file.
- The third image gives us the flag.

  ```
  PS C:\Users\Aaditya\Desktop\picoCTF> steghide extract -sf ./picture3.bmp -p “DUEDILIGENCE”
  wrote extracted data to "flag.txt".
  ```

## Flag:

```
picoCTF{h1dd3n_1n_pLa1n_51GHT_18375919}
```

## Concepts learnt:

-  I learnt how to extract objects from a file using wireshark by filtering based on type of data. 
-  I learned what a ROT-13 cipher is.
-  I learnt how to use steghide to decode text from images using steganography.

## Notes:

- Nil.

## Resources:

- [Decrypting the cipher](https://www.dcode.fr/cipher-identifier)
  

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
