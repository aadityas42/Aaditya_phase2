# Hash Vegas

## Solution

- Our goal is to maintain a balance of 1,000,000,000$ starting from 100$
- There are 2 main flaws in this: 
  PRNG Leak: The games leak the internal state of Python’s random module (MT19937).
  Weak Hashing: The lottery uses a secret-prefix hash (H(secret∣∣data)). One of the allowed hashes is SHA-1, which is vulnerable to length-extension attacks.

- Python uses the MT19937 algorithm. If you can observe 624 consecutive 32-bit outputs, you can "untemper" them to recover the entire internal state and predict every future "random" number.
  Roulette: Each round leaks 256 bits (8 words). We play 64 times → 512 words.
  lots: Each spin leaks two 32-bit values via fruit symbols. We spin 56 times → 112 words.
  Total: 512+112=624 words. We now "own" the RNG.

- Predicting the "Golden" Ticket
- The lottery shuffles a list of hash functions. Only one of them is SHA-1. Since we know the PRNG state, we can simulate the lottery locally until we find the exact turn where the service will pick SHA-1. We skip all other turns by placing "0" bets.

- SHA-1 Length Extension
- When the SHA-1 turn arrives, we buy a ticket. The server gives us a voucher: SHA1(secret∣∣username∣1). Because SHA-1 processes data in blocks, we can take that hash and use it as a starting state to append our own data (∣1000000000) without knowing the secret.
- The server truncates SHA-1 to 20 bytes, and since SHA-1's full digest is exactly 20 bytes, the truncation does nothing to stop our forged hash from being accepted.


```
import socket, struct, hashlib, random, re

def untemper(y):
    def un_xor_r(y, s):
        x = 0
        for i in range(32):
            bit = (y >> (31-i)) & 1
            if (31-i) + s < 32: bit ^= (x >> (31-i+s)) & 1
            x |= (bit << (31-i))
        return x
    def un_xor_l(y, s, m):
        x = 0
        for i in range(32):
            bit = (y >> i) & 1
            if i - s >= 0: bit ^= ((x >> (i-s)) & 1) & (m >> i)
            x |= (bit << i)
        return x
    y = un_xor_r(y, 18)
    y = un_xor_l(y, 15, 0xefc60000)
    y = un_xor_l(y, 7, 0x9d2c5680)
    return un_xor_r(y, 11)

def get_sha1_pad(msg_len):
    pad = b'\x80' + b'\x00' * ((56 - (msg_len + 1) % 64) % 64)
    return pad + struct.pack('>Q', msg_len * 8)

def solve():
    # 1. Collect 624 outputs from Roulette (8/rd) and Slots (2/rd)
    # 2. Rebuild state
    state = [untemper(x) for x in collected_outputs]
    rng = random.Random()
    rng.setstate((3, tuple(state + [624]), None))

    # 3. Predict the SHA-1 ticket
    funcs = [hashlib.sha256]*1024 + [hashlib.sha3_224]*1023 + [hashlib.sha1]
    rng.shuffle(funcs)
```

## Flag
```
nite{9ty%_0f_g4mbler5_qu17_b3f0re_th3y_mak3_1t_big}
```
