## Cracking an LCG
It is a PRNG which is defined by the equation : **State<sub>i+1</sub> = (a.State<sub>i</sub> + b) mod M** <br>
Commonly, `a` is known as the `multiplier`and `b` is known as the `increment`. <br>
<br>
Let's say we have access to an LCG but the only value we know of is M. Then we can easily predict all future outputs of the LCG using basic modular arithmetic.<br>
<br>


We can obtain the values of:<br>
<br>
State<sub>1</sub> = S<sub>1</sub><br>
State<sub>2</sub> = S<sub>2</sub><br>
State<sub>3</sub> = S<sub>3</sub><br>
<br>
So three equations and three unknowns, namely the multiplier, the increment and State<sub>0</sub> (commonly called the seed).This can easily be solved as shown: <br>
```import math
import random
from Cryptodome.Util.number import inverse
class PRNG:
    a = random.randint(0,1000)
    b = random.randint(0,1000)
    M = 2305843009213693951  # 9th mersenne prime

    def __init__(self, seed):
        self.state = seed

    def next(self):
        self.state = (self.a * self.state + self.b) % self.M
        return self.state

LCG = PRNG(random.randint(0,1000))
S1 = LCG.next()
S2 = LCG.next()
S3 = LCG.next()
M = 2305843009213693951
Multiplier = ((S2 - S3)*inverse(S1 - S2, M)) % M
Increment = (S2 - Multiplier*S1) % M
Seed = ((S1 - Increment) * inverse(Multiplier, M)) % M
My_S4 = (Multiplier*S3 + Increment) % M
S4 = LCG.next()
assert My_S4 == S4
print("LCG Cracked!")```



