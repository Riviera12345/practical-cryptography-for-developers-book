# Elliptic Curve Cryptography \(ECC\) - Concepts

The [**Elliptic Curve Cryptography \(ECC\)**](https://en.wikipedia.org/wiki/Elliptic-curve_cryptography) is modern **family of public-key cryptosystems**, which is based on the algebraic structures of the **elliptic curves over finite fields** and on the difficulty of the [**Elliptic Curve Discrete Logarithm Problem \(ECDLP\)**](https://en.wikipedia.org/wiki/Elliptic-curve_cryptography#Rationale).

The **ECC cryptography** is considered a natural modern **successor of the RSA** cryptosystem, because ECC uses **smaller keys**, signatures and ciphertexts than RSA for the same level of security and provides very fast key generation and fast signatures.

**ECC** implements all major capabilities of the asymmetric cryptosystems: **encryption**, **signatures** and **key exchange**.

## ECC Keys

The **private keys** in the ECC are integers \(in the range of the curve's field size, typically **256-bit** integers\). Example of 256-bit ECC private key \(hex encoded, 32 bytes, 64 hex digits\) is: `0x51897b64e85c3f714bba707e867914295a1377a7463a9dae8ea6a8b914246319`.

The **key generation** in the ECC cryptography is as simple as securely generating a **random integer** in certain range, so it is extremely fast. Any number within the range is valid ECC private key.

The **public keys** in the ECC are **EC points** - pairs of integer coordinates {**_x_**, **_y_**}, laying on the curve. Due to their special properties, **EC points** can be **compressed** to just one coordinate + 1 bit \(odd or even\). Thus the **compressed public key**, corresponding to a 256-bit ECC private key, is a **257-bit** integer. Example of ECC public key \(corresponding to the above private key, encoded in the Ethereum format, as hex with prefix `02` or `03`\) is: `0x02f54ba86dc1ccb5bed0224d23f01ed87e4a443c47fc690d7797a13d41d2340e1a`. In this format the public key actually takes 33 bytes \(66 hex digits\), which can be optimized to exactly 257 bits.

## Curves and Key Length

**ECC** crypto algorithms can use different underlying **elliptic curves**. Different curves provide different level of **security** \(cryptographic strength\), different **performance** \(speed\) and different **key length**, and also may involve different algorithms.

**ECC curves**, adopted in the popular cryptographic libraries and security standards, have **name** \(named curves, e.g. `secp256k1` or `curve25519`\), **field size** \(which defines the key length, e.g. **256-bit**\), security **strength** \(the fields size / 2\), **performance** \(operations/sec\) and many other parameters.

**ECC keys** have **length**, which directly depends on the underlying curve. In most applications \(like OpenSSL, OpenSSH and Bitcoin\) the default **key length** for the ECC private keys is **256 bits**, but depending on the curve many different ECC key sizes are possible: 192-bit \(curve `secp192r1`\), 233-bit \(curve `sect233k1`\), 224-bit \(curve `secp224k1`\), 256-bit \(curves `secp256k1` and `curve25519`\), 283-bit \(curve `sect283k1`\), 384-bit \(curves `p384` and `secp384r1`\), 409-bit \(curve `sect409r1`\), 414-bit \(curve `curve41417`\), 448-bit \(curve `Ed448-Goldilocks`\), 511-bit \(curve `M-511`\), 521-bit \(curve `p521`\), 571-bit \(curve `sect571k1`\) and many others.

## ECC Algorithms

**Elliptic-curve cryptography** (ECC) provides several groups of algorithms, based on the math of the elliptic curves over finite fields:

* ECC **digital signature** algorithms like [**ECDSA**](https://en.wikipedia.org/wiki/Elliptic_Curve_Digital_Signature_Algorithm) \(for classical curves\) and [**EdDSA**](https://en.wikipedia.org/wiki/EdDSA) \(for twisted Edwards curves\).
* ECC **encryption** algorithms and hybrid encryption schemes like the [**ECIES**](https://en.wikipedia.org/wiki/Integrated_Encryption_Scheme) integrated encryption scheme and [**EEECC**](https://cse.unl.edu/~ssamal/crypto/EEECC.pdf) \(EC-based ElGamal\).
* ECC **key agreement** algorithms like [**ECDH**](https://en.wikipedia.org/wiki/Elliptic-curve_Diffie–Hellman), [**X25519**](https://cryptography.io/en/latest/hazmat/primitives/asymmetric/x25519/) and [**FHMQV**](https://fastd.readthedocs.io/en/v18/crypto/fhmqvc.html).

All these algorithms use a **curve** behind (like `secp256k1`, `curve25519` or `p521`) for the calculations and rely of the difficulty of the **ECDLP** (elliptic curve discrete logarithm problem). All these algorithms use public / private key pairs, where the **private key** is an integer and the **public key** is a point on the elliptic curve (EC point). Let's get into details about the elliptic curves over finite fields.

## Elliptic Curves

In mathematics **elliptic curves** are plane algebraic curves, consisting of all points {**_x_**, **_y_**}, described by the equation:
 - x<sup>2</sup> = y<sup>3</sup> + **_a_**x + **_b_**

For example, the [NIST curve `secp256k1`](https://en.bitcoin.it/wiki/Secp256k1) (used in Bitcoin) is based on an elliptic curve in the form:
 - x<sup>2</sup> = y<sup>3</sup> + **_7_** (the general elliptic curve equation, where **_a_** = **0** and **_b_** = **7**)

This is a visualization of the above elliptic curve:

![](/assets/bitcoin-elliptic-curve.png)

To learn more about the equations of the elliptic curves and how they look like, play a bit with this **online elliptic curve visualization tool**: https://www.desmos.com/calculator/ialhd71we3.

![](/assets/ecc-visualization-tool.png)

### Elliptic Curves over Finite Fields

The **elliptic curve cryptography (ECC)** uses **elliptic curves over the [finite field](https://en.wikipedia.org/wiki/Finite_field) 𝔽<sub>p</sub>** (where **_p_** is prime and **_p_** > 3). This means that the field is a **square matrix** of size **_p_** x **_p_** and the points on the curve are limited to **integer coordinates** within the field only. All algebraic operations within the field (like point addition and multiplication) result in another point within the field. The elliptic curve equation over the finite field **𝔽<sub>p</sub>** takes the following modular form:
 - x<sup>2</sup> ≡ y<sup>3</sup> + **_a_**x + **_b_** (mod **_p_**)

Respectively, the "Bitcoin curve" `secp256k1` takes the form:
- x<sup>2</sup> ≡ y<sup>3</sup> + **_7_** (mod **_p_**)

Unlike **RSA**, which uses for its key space the **integers** in the range [0...**_p_**-1] (the field ℤ<sub>p</sub>), the **ECC** uses the **points** {**_x_**, **_y_**} within the Galois field **𝔽<sub>p</sub>** (where **_x_** and **_y_** are integers in the range [0...**_p_**-1]).

An **elliptic curve over the finite field** **𝔽<sub>p</sub>** consists of:
 - a set of integer coordinates {**_x_**, **_y_**}, such that **0** ≤ **_x_**, **_y_** < **_p_**
 - staying on the elliptic curve: **_y_**<sup>2</sup> ≡ x<sup>3</sup> + **_a_**x + **_b_** (mod **p**)

**Example** of elliptic curve over the finite field **𝔽<sub>17</sub>**:
 - y<sup>2</sup> ≡ x<sup>3</sup> + **7** (mod **17**)

This elliptic curve over **𝔽<sub>17</sub>** looks like this:

![](/assets/elliptic-curve-over-f17-example.png)

Note that the elliptic curve over finite field y<sup>2</sup> ≡ x<sup>3</sup> + **7** (mod **17**) consists of the **blue points** at the above figure, i.e. in practice the "_elliptic curves_" used in cryptography are "_sets of points in square matrix_", not classical "_curves_".

### Elliptic Curves over Finite Fields: Calculations

It is pretty easy to calculate whether **certain point belongs to certain elliptic curve** over a finite field. For example, a point {**_x_**, **_y_**} belongs to the curve y<sup>2</sup> ≡ x<sup>3</sup> + **7** (mod **17**) when and only when:
 - x<sup>3</sup> + **7** - y<sup>2</sup> ≡ 0 (mod **17**)

The point **P** {**5**, **8**} **belongs** to the curve, because `(5**3 + 7 - 8**2) % 17 == 0`. The point {**9**, **15**} **does not belong** to the curve, because `(9**3 + 7 - 15**2) % 17 != 0`. These calculations are in Python style. The above mentioned elliptic curve and the points {**5**, **8**} and {**9**, **15**} are visualized below:

![](/assets/points-on-elliptic-curve-over-finite-field.png)

### Multiplying ECC Point by Integer

A point **G** over an elliptic curve over finite field (EC point) can be **multiplied by an integer** **k** and the result is another EC point **P** on the same curve and this operation is **fast**:
 - **P** = **k** \* **G**

The above operation involves some formulas and transformations, but for simplicity, we shall skip them. The important thing to know is that multiplying EC point by integer returns another EC point on the same curve and this operation is **fast**. Everyone is free to [read more about EC point multiplication](https://en.wikipedia.org/wiki/Elliptic_curve_point_multiplication).

### Example: Multiply EC Point by Integer

For **example** let's take the EC point **G** = {**15**, **13**} on the elliptic curve over finite field y<sup>2</sup> ≡ x<sup>3</sup> + **7** (mod **17**) and multiply it by **k** = **6**. We shall obtain an EC point **P** = {**5**, **8**}:
  - **P** = **k** \* **G** = **6** \* {**15**, **13**} = {**5**, **8**}

The below figure visualizes this example of EC point multiplication:

![](/assets/ECC-multiply-point-example.png)

### The "Generator" Point in ECC

For the elliptic curves over finite fields ECC cryptosystems define a special pre-defined (constant) point called **[generator point G](https://en.wikipedia.org/wiki/Generating_set_of_a_group)**, which can **generate any other point** over the elliptic curve by multiplying **G** by some integer in the range [0...**n**]. The number **n** is called "**order**" of the cyclic group, defined by the EC.

In ECC cryptography the EC points form a **[cyclic group](https://en.wikipedia.org/wiki/Cyclic_group)**, which means that a number **n** exists (**n** > 1), such that **n** \* **G** = **_infinity_**. All possible EC points over the curve, that can be generated from **G** are **n** (including the special point _infinity_).

At the above example (the EC over finite field y<sup>2</sup> ≡ x<sup>3</sup> + **7**), if we take the point **G** = {**15**, **13**} as **generator**, any other point from the curve can be obtained by multiplying **G** by some integer in the range [1...18]. Thus the order of the EC is **n** = **18**.

### Private Key, Public Key and the Generator Point in ECC

In the **ECC**, when we multiply a fixed EC point **G** (the **generator** point) by certain **integer k** (**k** can be considered as **private key**), we obtain an EC point **P** (its corresponding **public key**).

Consequently, in ECC we have:
  - **elliptic curve** (EC) over finite field **𝔽<sub>p</sub>**
  - **G** == **generator point** (fixed constant, a starting point on the EC)
  - **k** == **private key** (integer)
  - **P** == **public key** (point)
 
It is **very fast** to calculate **P** = **k** \* **G**, using the well-known [ECC multiplication algorithms](https://en.wikipedia.org/wiki/Elliptic_curve_point_multiplication) in time _log_<sub>2</sub>(**_k_**), e.g. the "[double-and-add algorithm](https://en.wikipedia.org/wiki/Elliptic_curve_point_multiplication#Double-and-add)". For 256-bit curves, it will take just a few hundreds simple EC operations.

It is **extremely slow** (considered infeasible for large **k**) to calculate **k** = **P** / **G**.

This asymmetry (fast multiplication and infeasible slow opposite operation) is the basis of the security strength behind the ECC cryptography, also known as the **ECDLP problem**.

### Elliptic-Curve Discrete Logarithm Problem (ECDLP)

The **Elliptic Curve Discrete Logarithm Problem (ECDLP)** in computer science is defined as follows:

 - By given elliptic curve over finite field **𝔽<sub>p</sub>** and generator point **_G_** on the curve and point **_P_**  on the curve, find the integer **_k_** (if it exists), such that **_P_** = **_k_** \* **_G_**
 
For carefully chosen (by cryptographers) finite fields and elliptic curves, **the ECDLP problem has no efficient solution**.
 
The **multiplication** of elliptic curve points in the group **𝔽<sub>p</sub>** is similar to **exponentiation** of integers in the group **ℤ<sub>p</sub>** (this is known as _multiplicative notation_) and this is how the **ECDLP problem** is similar to the [**DLP problem**](../key-exchange/diffie-hellman-key-exchange.md#discrete-logarithm-problem-dlp) (discrete logarithm problem).

In the ECC cryptography, many algorithms rely on the **computational difficulty of the ECDLP problem** over carefully chosen field **𝔽<sub>p</sub>** and elliptic curve, for which **no efficient algorithm exists**.

### ECC and Curve Security Strength

Because the fastest known algorithm to solve the **ECDLP **for key of size **_p_** (EC over finite field of size **_p_** \* **_p_**) needs $$\sqrt{p}$$ steps, this means that to achieve a **_p_**-bit **security strength**, a **_2\*p_**-bit curve is needed. Thus **256-bit curves** provide **128-bit security strength**.

For example, the `secp256k1` (**_p_** = 256) curve provides 128-bit security and the `curve448` (**_p_** = 448) provides 224-bit security.

### Multiplication of EC Points - Example in Python

Now, after all the concepts, let's **write some code**. We shall use [the Python library `tinyec`](https://github.com/alexmgr/tinyec), which provides **ECC** primitives, such as **cyclic groups** (the `SubGroup` class), **elliptic curves** over finite fields (the `Curve` class) and EC **points** (the `Point` class). First, install the package `tinyec`:

```py
pip install tinyec
```

We shall play with the curve from our previous examples y<sup>2</sup> ≡ x<sup>3</sup> + **7** (mod **17**), with the generator point **G** = {15, 13}, which has order of **n** = **18**.

```py
from tinyec.ec import SubGroup, Curve

field = SubGroup(p=17, g=(15, 13), n=18, h=1)
curve = Curve(a=0, b=7, field=field, name='p1707')
print('curve:', curve)

for k in range(0, 25):
    p = k * curve.g
    print(f"{k} * G = ({p.x}, {p.y})")
```

The above code demonstrates the **EC multiplication**. It multiplies the generator point by 0, 1, 2, ..., 24. The output from the above program is as follows:

```
curve: "p1707" => y^2 = x^3 + 0x + 7 (mod 17)
0 * G = (None, None)
1 * G = (15, 13)
2 * G = (2, 10)
3 * G = (8, 3)
4 * G = (12, 1)
5 * G = (6, 6)
6 * G = (5, 8)
7 * G = (10, 15)
8 * G = (1, 12)
9 * G = (3, 0)
10 * G = (1, 5)
11 * G = (10, 2)
12 * G = (5, 9)
13 * G = (6, 11)
14 * G = (12, 16)
15 * G = (8, 14)
16 * G = (2, 7)
17 * G = (15, 4)
18 * G = (None, None)
19 * G = (15, 13)
20 * G = (2, 10)
21 * G = (8, 3)
22 * G = (12, 1)
23 * G = (6, 6)
24 * G = (5, 8)
```

It is visible that **0** \* **G** = **_infinity_**. It is also clearly visible, that the EC group is **cyclic** and the order of the EC group is **n** = **18**, because starting from **k** = **18**, the next points repeat the first ones:
 - **18** \* **G** = **0** \* **G** = _infinity_
 - **19** \* **G** = **1** \* **G** = {15, 13}
 - **20** \* **G** = **2** \* **G** = {2, 10}
 - **21** \* **G** = **3** \* **G** = {8, 3}
 - etc.

### Multiplication of EC Points - Real-World Example in Python

Now, let's write a **real-world example**. Instead of using our educational curve `p1707` (4-5-bit curve, p = 17), we shall use the cryptographic curve `secp192r1` (192-bit, p = 6277101735386680763835789423207666416083908700390324961279). The below example is similar to the previous:

```py
from tinyec import registry

curve = registry.get_curve('secp192r1')
print('curve:', curve)

for k in range(0, 10):
    p = k * curve.g
    print(f"{k} * G = ({p.x}, {p.y})")

print('Cyclic group order =', curve.field.n)

nG = curve.field.n * curve.g
print(f"n * G = ({nG.x}, {nG.y})")
```

The output is also similar to the previous example:

```
curve: "secp192r1" => y^2 = x^3 + 6277101735386680763835789423207666416083908700390324961276x + 2455155546008943817740293915197451784769108058161191238065 (mod 6277101735386680763835789423207666416083908700390324961279)
0 * G = (None, None)
1 * G = (602046282375688656758213480587526111916698976636884684818, 174050332293622031404857552280219410364023488927386650641)
2 * G = (5369744403678710563432458361254544170966096384586764429448, 5429234379789071039750654906915254128254326554272718558123)
3 * G = (2915109630280678890720206779706963455590627465886103135194, 2946626711558792003980654088990112021985937607003425539581)
4 * G = (1305994880430903997305943738697779408316929565234787837114, 3981863977451150342116987835776121688410789618551673306674)
5 * G = (410283251116784874018993562136566870110676706936762660240, 1206654674899825246688205669651974202006189255452737318561)
6 * G = (4008504146453526025173196900303594155799995627910231899946, 3263759301305176906990806636587838100022690095020155627760)
7 * G = (3473339081378406123852871299395262476289672479707038350589, 2152713176906603604200842901176476029776544337891569565621)
8 * G = (1167950611014894512313033362696697441497340081390841490910, 4002177906111215127148483369584652296488769677804145538752)
9 * G = (3176317450453705650283775811228493626776489433309636475023, 44601893774669384766793803854980115179612118075017062201)
Cyclic group order = 6277101735386680763835789423176059013767194773182842284081
n * G = (None, None)
```

We have a **cyclic group** of very large order **n** = 6277101735386680763835789423176059013767194773182842284081 and again **n** \* **G** = **_infinity_**, just like the previous example with our educational curve.

Now, let's generate a random **private key** `privKey` (integer in the range [0...n-1]) and its corresponding **public key** `pubKey = privKey * G`:

```py
from tinyec import registry
import secrets

curve = registry.get_curve('secp192r1')

privKey = secrets.randbelow(curve.field.n)
pubKey = privKey * curve.g
print("private key:", privKey)
print("public key:", pubKey)
```

The above code will produce output like this:

```
private key: 4225655318977962031264230130242180748818603147467615868902
public key: (5396030834456770190396776530938374882273836179487834152291, 3422160588166914010077732710830109086004758012634997793937) on "secp192r1" => y^2 = x^3 + 6277101735386680763835789423207666416083908700390324961276x + 2455155546008943817740293915197451784769108058161191238065 (mod 6277101735386680763835789423207666416083908700390324961279)
```

### Public Key Compression in the Elliptic Key Cryptosystems

...

### Compressing a Public Key in Python

...

### ECC Domain Parameters and the "secp256k1" Curve

In cryptography, elliptic curves over **_Fp_** are used, where the modulus **_p_** is very large prime, e.g. 256-bit number. The field of the curve is of size **p** x **p**, which is incredibly large.

...

### Safe ECC Curves

Currently cryptography only supports NIST curves, none of which are considered “**safe**” by the SafeCurves project run by Daniel J. Bernstein and Tanja Lange: [https://safecurves.cr.yp.to](https://safecurves.cr.yp.to).

### Python Examples with the "secp256k1" Curve

...
