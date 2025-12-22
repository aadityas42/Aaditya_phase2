# Stronk Rabin

## Solution

- If N is unknown, query the ENC oracle with a random value m:
    m2−ENC(m)≡0(modN)
- Calculate N=gcd(m12​−ENC(m1​),m22​−ENC(m2​)).
- Next is factoring N
- The DEC(1) oracle returns a sum of a shuffled subset of the 16 roots of unity.
- Different calls return different combinations of roots.
- We query DEC(1) twice to get sums S1​ and S2​.
- Calculate gcd(S1​−S2​,N) to extract a non-trivial factor. This is recursively repeated until all four primes (p,q,r,s) are found.
- Lastly to extract the flag 
- Because the oracle returns a sum of roots, a specific combination will yield a value of the form 2m(modN) (where m is the original plaintext).
- Query DEC(C) multiple times to gather unique sum combinations.
- Multiply each gathered sum by 2−1(modN).
- Check each result for the flag prefix nite{ to obtain the flag.


Script:
```
import json, random
from math import gcd
from Crypto.Util.number import long_to_bytes, inverse
from pwn import remote
from sympy import isprime, crt

HOST, PORT = "stronk.chals.nitectf25.live", 1337

def call(io, func, arg):
    io.sendline(json.dumps({"func": func, "args": [int(arg)]}).encode())
    while True:
        line = io.recvline().decode().strip()
        if line.startswith('{'): return json.loads(line)["retn"]

def recover_modulus(io):
    g = 0
    for _ in range(5):
        m = random.getrandbits(1024)
        g = gcd(g, abs(m**2 - int(call(io, "ENC", m))))
    return g

def factorize(io, n):
    factors = [n]
    res = []
    while factors:
        f = factors.pop()
        if isprime(f):
            res.append(f)
            continue
       
        while True:
            g = gcd(abs(int(call(io, "DEC", 1)) - int(call(io, "DEC", 1))), f)
            if 1 < g < f:
                factors.extend([g, f // g])
                break
    return res

def recover_flag(C, primes):

    roots = [[pow(C, (p + 1) // 4, p), p - pow(C, (p + 1) // 4, p)] for p in primes]
    N = 1
    for p in primes: N *= p
    

    for i in range(16):
        res = [roots[j][(i >> j) & 1] for j in range(4)]
        x = crt(primes, res)[0]
        for candidate in [x, N - x]:
            b = long_to_bytes(int(candidate))
            if b"nite{" in b: return b
    return b"Flag not found"

def main():
    io = remote(HOST, PORT, ssl=True)
    line = io.recvline().decode()
    C = json.loads(line if "{" in line else io.recvline())["C"]
    
    N = recover_modulus(io)
    primes = factorize(io, N)
    print(f"[*] Flag: {recover_flag(C, primes).decode(errors='ignore')}")
    io.close()

if __name__ == "__main__":
    main()
```

## Flag
```
nite{rabin_stronk?_no_r4bin_brok3n}
```
