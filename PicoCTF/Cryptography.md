# 2. Custom Encryption

Can you get sense of this code file and write the function that will decode the given encrypted file content. Find the encrypted file here flag_info and code file might be good to analyze and get the flag. 

## Solution:

- First, I analysed the script we were given, and realised that it does 3 main things: generate a sharedkey, XOR scramble it, and then it into it's ASCII value and multiplies it with the shared key and the constant 311.

- To decrypt this, we need to perform all of the operations in the reverse order.
  
- First, we must divide all the ciphers by the shared key and by 311.
  
- Since we are given the formula to calculate the shared key, we can use some modular arithmetic to calculate the share key, which gives us the value 22.
  
- So, we know that all the ciphers are to be divided by the value 22*311, that is 6842.
  
- After dividing all the ciphers by the value we obtained, we convert those into charecters using ASCII.
  
- We need to reverse the XOR scramble on the resulting string of charecters with the argument "trudeau" as given in the script.
  
- After this, we get the flag, but reversed. So we simply need to reverse it again to obtain the flag.
  
```
cipher_list = [
    61578, 109472, 437888, 6842, 0, 20526, 129998, 526834, 478940, 287364,
    0, 567886, 143682, 34210, 465256, 0, 150524, 588412, 6842, 424204,
    164208, 184734, 41052, 41052, 116314, 41052, 177892, 348942, 218944,
    335258, 177892, 47894, 82104, 116314
]
key = "trudeau"
K = 6842
semi_cipher = ""
for num in cipher_list:
    ord_value = num // K
    semi_cipher += chr(ord_value)
reversed_flag = ""
key_length = len(key)
for i, char in enumerate(semi_cipher):
    key_char = key[i % key_length]
    decrypted_char = chr(ord(char) ^ ord(key_char))
    reversed_flag += decrypted_char
flag = reversed_flag[::-1]
print(f"Flag: {flag}")
```

## Flag:

```
picoCTF{custom_d2cr0pt6d_49fbee5b}
```

## Concepts learnt:

- I learnt how to backtrace complicated encryption, by analysing a script given to us.
- I also learnt how to perform XOR operation to encrypt or decrypt a given string.

## Notes:

- An alternate "method" is that we don't need to have a line to explicitly calculate the XOR in the script, we can simply use the 'dynamic_xor_encrypt' function from the script given to us in the challenge description.


## Resources:

- Nil. 


***


# 3. mini RSA

What happens if you have a small exponent? There is a twist though, we padded the plaintext so that (M ** e) is just barely larger than N. Let's decrypt this: ciphertext

## Solution:

- We can tell by the title that this is a challenge based on RSA encryption, so I looked it up on wikipedia and found a formula that can be used to compute the m value using the given N,C and e values.

- The formula used to find m is:   c is congruent to m^e(mod N)

- We need to rearrange the formula a little to compute m. We can write it as:    m^3=c+ k*N    where k is some constant that we are multiplying with N.

- To find k, and then use that to convert our m value into a readable flag, we can use a python script to iterate over all values upto 10000.
  
```
import gmpy2
N = 1615765684321463054078226051959887884233678317734892901740763321135213636796075462401950274602405095138589898087428337758445013281488966866073355710771864671726991918706558071231266976427184673800225254531695928541272546385146495736420261815693810544589811104967829354461491178200126099661909654163542661541699404839644035177445092988952614918424317082380174383819025585076206641993479326576180793544321194357018916215113009742654408597083724508169216182008449693917227497813165444372201517541788989925461711067825681947947471001390843774746442699739386923285801022685451221261010798837646928092277556198145662924691803032880040492762442561497760689933601781401617086600593482127465655390841361154025890679757514060456103104199255917164678161972735858939464790960448345988941481499050248673128656508055285037090026439683847266536283160142071643015434813473463469733112182328678706702116054036618277506997666534567846763938692335069955755244438415377933440029498378955355877502743215305768814857864433151287
e = 3
c = 1220012318588871886132524757898884422174534558055593713309088304910273991073554732659977133980685370899257850121970812405700793710546674062154237544840177616746805668666317481140872605653768484867292138139949076102907399831998827567645230986345455915692863094364797526497302082734955903755050638155202890599808146919581675891411119628108546342758721287307471723093546788074479139848242227243523617899178070097350912870635303707113283010669418774091018728233471491573736725568575532635111164176010070788796616348740261987121152288917179932230769893513971774137615028741237163693178359120276497700812698199245070488892892209716639870702721110338285426338729911942926177029934906215716407021792856449586278849142522957603215285531263079546937443583905937777298337318454706096366106704204777777913076793265584075700215822263709126228246232640662350759018119501368721990988895700497330256765579153834824063344973587990533626156498797388821484630786016515988383280196865544019939739447062641481267899176504155482
found_flag = False
for k in range(10000): 
    m_cubed = (k * N) + c
    m, is_perfect = gmpy2.iroot(m_cubed, e)
    if is_perfect:
        byte_length = (m.bit_length() + 7) // 8
        byte_string = m.to_bytes(byte_length, byteorder='big')
        flag = byte_string.decode('utf-8')
        if 'picoCTF' in flag:
            print(f"Found k = {k}")
            print(f"{flag}")
            found_flag = True
            break
```

## Flag:

```
picoCTF{e_sh0u1d_b3_lArg3r_60ef2420}
```

## Concepts learnt:

- I learned how RSA encryption works, including the basic modular arithmetic behind it.
- I learned that RSA encoded messages can be transformed to a readable form using something called big-endian byte representation.
- I also learned how to use the gmpy2 library to compute the cube root of a large number.

## Notes:

- At first, after figuring out the script, I was iterating over only 1000 values since I didn't expect the k value to be very large. When nothing else was working, I changed the value to 10000.


## Resources:

- [wikipedia](https://en.wikipedia.org/wiki/RSA_cryptosystem)
