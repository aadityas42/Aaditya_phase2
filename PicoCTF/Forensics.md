# 1. trivial flag transfer protocol

Figure out how they moved the flag.

## Solution:



## Flag:

```

```

## Concepts learnt:

- 

## Notes:

- 

## Resources:

- Nil.


***

# 2. tunn3l v1s10n

We found this file. Recover the flag.

## Solution:


## Flag:

```

```

## Concepts learnt:

- 

##Notes:

- 

## Resources:

- Nil.


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
