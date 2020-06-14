# tiny-secp256k1-wasm
[![NPM](https://img.shields.io/npm/v/tiny-secp256k1-wasm.svg)](https://www.npmjs.org/package/tiny-secp256k1-wasm)

This library is under heavy development. Currently only supports nodejs environment.

## Installation

### npm

``` bash
npm install tiny-secp256k1-wasm
```

### yarn

```bash
yarn add tiny-secp256k1-wasm
```

## Documentation

### isPoint (A)
``` haskell
isPoint :: Buffer -> Bool
```
Returns `false` if
* `A` is not encoded with a sequence tag of `0x02`, `0x03` or `0x04`
* `A.x` is not in `[1...p - 1]`
* `A.y` is not in `[1...p - 1]`


### isPointCompressed (A)
``` haskell
isPointCompressed :: Buffer -> Bool
```
Returns `false` if the signature is **not** compressed.


### isPrivate (d)
``` haskell
isPrivate :: Buffer -> Bool
```
Returns `false` if
* `d` is not 256-bit, or
* `d` is not in `[1..order - 1]`


### pointAdd (A, B[, compressed])
``` haskell
pointAdd :: Buffer -> Buffer [-> Bool] -> Maybe Buffer
```
Returns `null` if result is at infinity.

##### Throws:
* `Expected Point` if `!isPoint(A)`
* `Expected Point` if `!isPoint(B)`


### pointAddScalar (A, tweak[, compressed])
``` haskell
pointAddScalar :: Buffer -> Buffer [-> Bool] -> Maybe Buffer
```
Returns `null` if result is at infinity.

##### Throws:
* `Expected Point` if `!isPoint(A)`
* `Expected Tweak` if `tweak` is not in `[0...order - 1]`


### pointCompress (A, compressed)
``` haskell
pointCompress :: Buffer -> Bool -> Buffer
```

##### Throws:
* `Expected Point` if `!isPoint(A)`


### pointFromScalar (d[, compressed])
``` haskell
pointFromScalar :: Buffer [-> Bool] -> Maybe Buffer
```
Returns `null` if result is at infinity.

##### Throws:
* `Expected Private` if `!isPrivate(d)`


### pointMultiply (A, tweak[, compressed])
``` haskell
pointMultiply :: Buffer -> Buffer [-> Bool] -> Maybe Buffer
```
Returns `null` if result is at infinity.

##### Throws:
* `Expected Point` if `!isPoint(A)`
* `Expected Tweak` if `tweak` is not in `[0...order - 1]`


### privateAdd (d, tweak)
``` haskell
privateAdd :: Buffer -> Buffer -> Maybe Buffer
```
Returns `null` if result is equal to `0`.

##### Throws:
* `Expected Private` if `!isPrivate(d)`
* `Expected Tweak` if `tweak` is not in `[0...order - 1]`


### privateSub (d, tweak)
``` haskell
privateSub :: Buffer -> Buffer -> Maybe Buffer
```
Returns `null` if result is equal to `0`.

##### Throws:
* `Expected Private` if `!isPrivate(d)`
* `Expected Tweak` if `tweak` is not in `[0...order - 1]`


### sign (h, d)
``` haskell
sign :: Buffer -> Buffer -> Buffer
```
Returns normalized signatures, each of (r, s) values are guaranteed to less than `order / 2`.
Uses RFC6979.

##### Throws:
* `Expected Private` if `!isPrivate(d)`
* `Expected Scalar` if `h` is not 256-bit


### signWithEntropy (h, d, e)
``` haskell
sign :: Buffer -> Buffer -> Buffer -> Buffer
```
Returns normalized signatures, each of (r, s) values are guaranteed to less than `order / 2`.
Uses RFC6979.
Adds `e` as Added Entropy to the deterministic k generation.

##### Throws:
* `Expected Private` if `!isPrivate(d)`
* `Expected Scalar` if `h` is not 256-bit
* `Expected Extra Data (32 bytes)` if `e` is not 256-bit


### verify (h, Q, signature[, strict = false])
``` haskell
verify :: Buffer -> Buffer -> Buffer -> Bool
```
Returns `false` if any of (r, s) values are equal to `0`,  or if the signature is rejected.

If `strict` is `true`, valid signatures with any of (r, s) values greater than `order / 2` are rejected.

##### Throws:
* `Expected Point` if `!isPoint(Q)`
* `Expected Signature` if `signature` has any (r, s) values not in range `[0...order - 1]`
* `Expected Scalar` if `h` is not 256-bit

---

## Credit
This library uses [rust-secp256k1](https://github.com/rust-bitcoin/rust-secp256k1) to access the native library [secp256k1](https://github.com/bitcoin-core/secp256k1) by the bitcoin-core developers, including derivatives of its tests and test vectors.

Special thanks go to yuki-js, wg-wasm and beginners channel on the Rust Discord, and #rust-bitcoin on freenode IRC.

# LICENSE [MIT](LICENSE)
