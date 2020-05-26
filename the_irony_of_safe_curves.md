The security of public key cryptosystems in the wild are contingent on the intractibility of the following problems:    

* __Integer factorization__: We can't compute primes (p, q) given `n=pq`. 
* __Discrete Log Problem__: We can't figure out `g^xy` given `g^x` and `g^y`. 
* __Elliptic Curve DLP__: Given 3 points (P, aP, bP) on *certain* elliptic curves, finding `abP` without  (a, b) is hard. 
<hr>
The last problem is the most interesting in some regards, because the curve you pick determines the difficulty x performance target of the encryption. Eg, `NIST P-256`, which Chrome uses to talk to Amazon, makes the following decisions for "efficiency":    

* Set prime: `2^256-2^224+2^192+2^96-1`
* Curve shape: `y^2=x^3-3x+b mod P` 
* Takes a "as small as possible" cofactor

Clearly everyone should pick the [hardest, safest, baddest](http://safecurves.cr.yp.to/) curve; right? but in practice, average clients (web browsers) only support NSA suite B curves (`P-256,384...`). So we just use `P-256` to minimize trouble. If your womanhood is threatened by using a 256 bit curve, use a 384 bit curve. It will probably increase your cpu and network costs by some negligable fraction. However, if you pick a non-NSA curve, some widespread web browser might not be able to talk to your server.
<hr>
Just as a point of reference, you can view available curves through

> openssl ecparam -list_curves

When we generate Bitcoin [private keys](https://davidederosa.com/basic-blockchain-programming/elliptic-curve-keys/) we use
> openssl ecparam -name secp256k1 -genkey -out ec-priv.pem

Which prodcues a 256 bit key over `y^2 = x^3 + 7`, the second (non-NSA) curve in the list above. 


