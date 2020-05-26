Generate private key

```console
$ openssl ecparam -name prime256v1 -genkey -noout -out key.pem
```
* ecparam: generates an ec parameters file
```console
$ openssl ecparam -name prime256v1 -noout
ASN1 OID: prime256v1
NIST CURVE: P-256
```
* genkey triggers private key generation in base64 pem format[^7]

Generate the corresponding public key
```console
$ openssl ec -in d ""
read EC key
Private-Key: (256 bit)
priv:
    b7:57:93:cc:eb:6b:d2:1c:04:33:48:48:2d:f3:4c:
    99:d2:8c:7e:ae:7e:40:93:6c:7d:f9:b9:88:fa:d0:
    f6:a8
pub:
    04:47:01:c7:f1:8f:fc:86:53:9b:4e:00:cd:a1:2f:
    e2:07:4f:da:bf:b6:e6:90:c0:26:c9:b1:67:64:38:
    66:ba:f9:ba:60:a2:1f:80:7c:69:dc:8f:54:6e:da:
    98:ac:5d:53:66:06:c1:42:89:2e:57:b3:72:da:1f:
    4b:f5:5a:6b:55
ASN1 OID: prime256v1
NIST CURVE: P-256
writing EC key
-----BEGIN PUBLIC KEY-----
MFkwEwYHKoZIzj0CAQYIKoZIzj0DAQcDQgAERwHH8Y/8hlObTgDNoS/iB0/av7bm
kMAmybFnZDhmuvm6YKIfgHxp3I9UbtqYrF1TZgbBQokuV7Ny2h9L9VprVQ==
-----END PUBLIC KEY-----
```

* pub section: Hex dump
* ASN1 OID: prime256v1
* NIST Curve: P-256
* EC Public key encoded version (omit with the `-noout` option)

Converting the hex dump to base64
```console
$ openssl ec -in key.pem -pubout -text -noout 2> /dev/null | grep "pub:" -A5 | sed 1d | xxd -r -p | base64 | paste -sd "\0" -
BEcBx/GP/IZTm04AzaEv4gdP2r+25pDAJsmxZ2Q4Zrr5umCiH4B8adyPVG7amKxdU2YGwUKJLhttps://davidederosa.com/basic-blockchain-programming/elliptic-curve-keys/lezctofS/Vaa1U=
```

## ECDHE

(C, G, n): Curve, base point , integer order of G (i.e n x G = 0)

### Encryption/Decryption

Converting a base64 public key to a PEM format is a pain, so you might as well save the public key as a pem before handing it out as b64:
```
$ openssl ec -pubout -out pub.pem -in key.pem -text -noout 2>/dev/null
$ cat ./pub.pem
Private-Key: (256 bit)
priv:
    b7:57:93:cc:eb:6b:d2:1c:04:33:48:48:2d:f3:4c:
    99:d2:8c:7e:ae:7e:40:93:6c:7d:f9:b9:88:fa:d0:
    f6:a8
pub:
    04:47:01:c7:f1:8f:fc:86:53:9b:4e:00:cd:a1:2f:
    e2:07:4f:da:bf:b6:e6:90:c0:26:c9:b1:67:64:38:
    66:ba:f9:ba:60:a2:1f:80:7c:69:dc:8f:54:6e:da:
    98:ac:5d:53:66:06:c1:42:89:2e:57:b3:72:da:1f:
    4b:f5:5a:6b:55
ASN1 OID: prime256v1
NIST CURVE: P-256
```

Obviously, as long as you have the private key you can always do this on demand.

### Groups

Creating groups can be tricky. A Group is essentially a set, ź, and a "group operation", θ. Well, it's a little more than that, but the 2 most important properties of a group are that A θ B will produce another element in the group, this is called closeness, and that an element θ its inverse, element¯¹, will produce the identity element, ǐ. Groups are useful because they allow us to reason in the abstract, for example ǐ could be 1, or a point at infinity on an elliptic curve.

__Not a group__
ź9=[1,2,3,4,5,6,7,8,9]
θ: multiplication mod 9
Closeness: our choice of θ and ź makes this obvious
ǐ: 1
The problem with this group is that 3¯¹ and 6¯¹ don't exist. For an element in ź9 to have an inverse it must be relatively prime to 9, i.e gcd(element, 9) = 1.

__Maybe a group__
ź9´=[1,2,4,5,7,8,9] - ź9 with the troublesome elements kicked out
θ: multiplication mod 9
Closeness: unclear (because a multiplication could produce an element we kicked out, eg: 4 θ 8 could produce 6).

The latter is an Abelian group under multiplication mod 9. In fact, any set with integers i=0..n-1 for which gcd(i, n)=1 is Abelian under multiplication mod n. I know this because a text book proved it to me, but it's usually not that straightforward. So the easiest way to produce a set that satisfies Closeness in a trivial and obvious way, like ź9, and where gcd(element, n) = 1, i.e all elements have an inverse, like ź9´, is to make n = prime.

### Elliptic curves

The security of public key cryptosystems in the wild (i.e in your webbrowser) are contingent on the intractibility of the following problems:
* Integer factorization: Attacker can't figure out 2 primes, (p, q), given n=pq. In other words, attackers can't compute an RSA private key from the corresponding public key.
* DLP: Attacker can't figure out g^xy given g^x and g^y. In other words, the Diffie-Hellman key exchange used in DSA is done once g^x and g^y are exchanged, but the attacker can't compute the session key g^xy.
*

It has to do with the Diffie–Hellman assumption. The DH key exchange is secure in groups where the computational DH assumption holds. One of the simplest such groups is the multiplicative group modulo a large prime.

### Safe curves and the NSA

Don't waste too much time trying to pick a "safe" curve [^1], out of the available options
```
$ openssl ecparam -list_curves
  secp112r1 : SECG/WTLS curve over a 112 bit prime field
  secp112r2 : SECG curve over a 112 bit prime field
...
```
Basically the curve you pick determines the difficulty x performance target of the encryption. Eg, `NIST P-256`, which we will use later, makes the following decisions for "efficiency":
* Set prime: `2^256-2^224+2^192+2^96-1`
  - primes are free generators of groups since we can define operations that don't produce an inverse
* Curve shape: `y^2=x^3-3x+b mod P` forming a [abelian] group
  - + 2 elements in the group to produce a third, 1:1, associative, commutative etc
* Takes a "as small as possible" cofactor

This *doesn't* make the curve unsafe, as Bernstein's paper might have you believe with the hazardous looking red sign. What he's trying to say is that the safe implementation of crypto primitives with these curves is trickier than with the "safe" curves. In practice, average clients (web browsers) only support NSA suite B curves (`P-256,384...`). So we just use `P-256` to minimize trouble. If your manhood is threatened by using a 256 bit curve, use a 384 bit curve. It will probably increase your cpu and network costs by some negligable fraction. If you pick a non-NSA curve, some widespread web browser might not be able to talk to your server :(

Just for a point of reference, in Bitcoin, we'd use[^6]
```console
$ openssl ecparam -name secp256k1 -genkey -out ec-priv.pem
```
Which is a 256 bit key over `y^2 = x^3 + 7`.

## Appendix

[^1]: [Safe curves](http://safecurves.cr.yp.to/)
[^2]: [xxd](https://linux.die.net/man/1/xxd)
[^3]: [Hex to base64](http://tomeko.net/online_tools/hex_to_base64.php?lang=en)
[^4]: [Hex to binary](http://www.unit-conversion.info/texttools/hexadecimal/)
[^5]: [More safe curves](https://security.stackexchange.com/questions/78621/which-elliptic-curve-should-i-use)
[^6]: [Bitcoin keys](https://davidederosa.com/basic-blockchain-programming/elliptic-curve-keys/)
[^7]: [Openssl EC operations](https://wiki.openssl.org/index.php/Command_Line_Elliptic_Curve_Operations)
[^8]: [Openssl encryption/decryption](https://jameshfisher.com/2017/04/14/openssl-ecc.html)

