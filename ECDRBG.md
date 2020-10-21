## Elliptic Curve Deterministic Random Bit Generator
It's a notorious PRNG which received widespread criticism, due to a suspicious backdoor implemented by the NSA. <br>
[From wikipedia](https://en.wikipedia.org/wiki/Dual_EC_DRBG): `Sometime before its first known publication in 2004, a possible kleptographic backdoor was discovered with the Dual_EC_DRBG's design, with the design of Dual_EC_DRBG having the unusual property that it was theoretically impossible for anyone but Dual_EC_DRBG's designers (NSA) to confirm the backdoor's existence.`<br><br>

Before reading further, if you aren't familiar with elliptic curve cryptography then [I suggest going through this first.](https://github.com/Nightshade999/Elliptic-Curve-Cryptography)<br><br>

### Working
1) Take a 256-bit seed.<br>
2) Multiply the seed with a point P, which is usually the generator point of the NIST-P256 curve. <br>
3) The x-coordinate of the resulting point is the seed for the next output. <br>
4) Multiply this second seed with a point Q of our choice. <br>
5) Take the x-coordinate of the resulting point, discard the high 16 bits, and output the value. <br>
<br>
So we have:<br><br>
Seed = S<sub>0</sub> <br>
S<sub>1</sub> = (S<sub>0</sub>.P).x <br>
return (S<sub>1</sub>.Q).x >> 240 <br>

### Backdoor
Suppose we choose Q such that **Q = (d.P)**, where `d` is an integer known to us.<br>
This is what happens:<br>
S<sub>1</sub> = (S<sub>0</sub>.P).x
**// Now Q = d.P**
return ((S<sub>1</sub>d).P).x >> 240 <br>
<br>
So now we can predict all future outputs. The only obstacle is retrieving the discarded 16 bits, which can easily be achieved using a bruteforce of 2<sup>16</sup> = 65536 tries.





