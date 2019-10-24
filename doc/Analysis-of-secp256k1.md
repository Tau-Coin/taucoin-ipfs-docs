# Implementation analysis of secp256k1

## Secp256k1 core
### sign
Create an ECDSA signature.
### verify
Verify an ECDSA signature.
### recoveryFromsignature
Recover an ECDSA public key from a signature.
## Implementation in different languages
### Java
There are two implementation methods in java.
- [Bouncycastle](http://www.bouncycastle.org/java.html)
- [Spongycastle](https://rtyley.github.io/spongycastle/)

[Ethereumj](https://github.com/ethereum/ethereumj) is using spongycastle as crypoto lib. 
### C
Details can be seen in:
- [Libsecp256k1 in bitcoin core](https://github.com/bitcoin-core/secp256k1)

[Bitcoin](https://github.com/bitcoin/bitcoin) and [Bitcoinj](https://github.com/bitcoinj/bitcoinj) are using libsecp256k1 as crypoto lib.

## Analysis in our test
In the beginning, we used spongycastle as our crypoto lib in TAU, just like ethereumj did. But then we found that the low efficient in practice, especially in android operating system of mobile phones. A recoveryFromsignature may take about 150ms in block and transaction signature, which means 50 txs take about 7.5s. In contast, libsecp256k1 takes bout 12ms in the same opeation and computing platform. The efficiency is off by an order of magnitude.

So we integrate the libsecp256k1 in [tau-mobile-mining](https://github.com/tau-coin/taucoin-mobile-mining) and recommend you to use libsecp256k1 in your project.