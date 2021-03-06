# Challenge: Master Keys

We're given a public key (n) and two private keys that work with it (d1, d1). Find the sum of the primes that are the factors of n.

```python
n = 0x74bf4fc2d66d08c0ac7d66d90e21b16b4c0d01b1e04c40fcc2ee8ee622dff02a03f9ac98027bfff7c119894e581af56ff997e4ee2123e8137e5b7771d02f208f0f47165c76150308fd7a1d47bb91e67a608a56b12f786d04cb59a15515af5d414f809c4f3564f35ed6b28e35aaf9dc3d88dda2609ece39edfc9d081e816a7c86173b26dfe4f48835c69a55a8c5a6f851fc617eee2c3b1f81bf7c0fed7c24adb345d2cfe8fa6e00b7f0cdbb5647004c806b81d4aa6507307700a9e46dc33b3d3d15c7aa79fcdaef7848344b637c2c173de0b3f085f97a88c18e9aa755d2989f4be01d55e234c39dfe834e193b65976e4d82bd8039df8515a50833683db2289e67
d1 = 0x2c93e020c53f5cd83504ad00b424a95a0eefe875c2ef5ae1288d6e82f14e10a5fa5e9b486faa695270a90f849a9fb097d0f5eeb3fe737769fa1572c64277bc36e3de80eb4d2b38bd760beaa2393cbc847dd854c1d31e68822c234d36efd0c60aea10719ecd047ee8f791b63b47cda00adef7c1c8a2da9bc40370f268b381b589f993af73230aec51d030308a574b340586dc6ee46fdb152bc906aa52812cf25910b8864f6e29eb7e4a42b0cbd1998d1707d5a3f1789793a88f0667146cd0e098d29f5ab120c5adb3a5be2287ebbf69f34d934bb8cbba25f9e0e417d56ad1e11b4dec4036b69d74050d5c7d9cac5a44038d92ff9adff8cdd54e660a703b012601
d2 = 0xa1532fe39bac6598e18213d9c2465ac55afcea27a33b9bddeb7bfd69142e00cffe5847e07226694a31c298d2f2baa607ca8dd3a21f975f7d7870ea3812a6dcc5f3259747c3403bc6738607e9f4cea2fede62ab730296d586f77cee8c0580234c39910dee02697247ce444470f2c77c4867d5642941a8d5b2000dfa8734ec320eb671677fc41635f188303272d9ce43e802f3e60ff6b9be2979aa3800741348d57ee5483131dc44f2ba3ac883d2a30805912161e35be060973c5ac30442a3d68c40898e9595386793c3b70479a9b1e2f098efdf28fea9a240a8e404c7b1eecf3c85a6fe718b9becd7e3cd377c5db12f5e15fd3324b10854da2d9d4ad475c7dea9
```

I'm actually not sure what the fact that we've got two private keys has to do with the solution. But we can easily find out that the exponent that goes along with the public key is 65537 with trial and error. For the private key I worked with, I chose the first (d1).

Given (n, e, d), we can try to find the factors of n with brute force.
We can find this with an algorithm [posted to StackExchange](https://crypto.stackexchange.com/questions/13113/how-can-i-find-the-prime-numbers-used-in-rsa#13115) by `catpnosis`.

After about 7-9 iterations of the outer loop, if I recall correctly, you will find one of the factors of n. Divide n by that factor to get the other, and you can sum them together to retrieve the flag.

```python
n = 0x74bf4fc2d66d08c0ac7d66d90e21b16b4c0d01b1e04c40fcc2ee8ee622dff02a03f9ac98027bfff7c119894e581af56ff997e4ee2123e8137e5b7771d02f208f0f47165c76150308fd7a1d47bb91e67a608a56b12f786d04cb59a15515af5d414f809c4f3564f35ed6b28e35aaf9dc3d88dda2609ece39edfc9d081e816a7c86173b26dfe4f48835c69a55a8c5a6f851fc617eee2c3b1f81bf7c0fed7c24adb345d2cfe8fa6e00b7f0cdbb5647004c806b81d4aa6507307700a9e46dc33b3d3d15c7aa79fcdaef7848344b637c2c173de0b3f085f97a88c18e9aa755d2989f4be01d55e234c39dfe834e193b65976e4d82bd8039df8515a50833683db2289e67
e = 65537
d = 0x2c93e020c53f5cd83504ad00b424a95a0eefe875c2ef5ae1288d6e82f14e10a5fa5e9b486faa695270a90f849a9fb097d0f5eeb3fe737769fa1572c64277bc36e3de80eb4d2b38bd760beaa2393cbc847dd854c1d31e68822c234d36efd0c60aea10719ecd047ee8f791b63b47cda00adef7c1c8a2da9bc40370f268b381b589f993af73230aec51d030308a574b340586dc6ee46fdb152bc906aa52812cf25910b8864f6e29eb7e4a42b0cbd1998d1707d5a3f1789793a88f0667146cd0e098d29f5ab120c5adb3a5be2287ebbf69f34d934bb8cbba25f9e0e417d56ad1e11b4dec4036b69d74050d5c7d9cac5a44038d92ff9adff8cdd54e660a703b012601
from fractions import gcd
for m in range(1, 20): # I actually used the range (7, 20) to save time, but whatever.
    for g in range(1, 400): print(g, gcd(n, pow(g, ((d * e - 1) / 2 ** m), n) - 1))
```