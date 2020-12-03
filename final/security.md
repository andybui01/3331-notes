### Symmetric ciphers
**Stream ciphers** encrypt one bit at a time.

**Block ciphers** break plaintext messages into equal-sized blocks. Then encrypt each block as a unit.

**Stream ciphers**
Combine each bit of a _keystream_ with bit of _plaintext_ to get bit of _ciphertext_.

```
m(i) = ith bit of message
ks(i) = ith bit of keystream
c(i) = ith bit of ciphertext

c(i) = ks(i) XOR m(i)
.
.
.
m(i) = ks(i) XOR c(i)

```

**Block cipher**
Ciphertext processed as _k_ bit blocks by using a table with 1:1 mappings that map k-bit plaintext to k-bit cipher text.

The table size is 2^k and there are (2^k)! possible permutations so we choose a large k (within allowable space).

### Public key crypto
Involves a public encryption key available to all.

Example:
Bob gives Alice his _public_ key, which Alice uses to encrypt her message into ciphertext. She sends the encrypted message to Bob who decrypts it with his _private_ key to get the plaintext message.

**RSA in practice**
RSA is computationally intensive and DES is at least 100 times faster than RSA. Therefore we use a public key crypto to establish a secure connection, then use a symmetric key for encrypting data.

### Confidentiality vs Integrity
**Confidentiality:** message private and secret
**Integrity:** protection against message tampering

Encryption alone may not be able to guarantee integrity as an attacker can modify message under encryption without learning what it is.

**Digital signatures**
Crypto technique analogous to hand-written signatures:
- sender digitally signs document, establishing he is document owner
- recipient can prove to someone that Bob, and no one else, must have signed document

We can use public key encryptions to do this but it's computationally expensive!

**Message digests**
Apply hash function _H_ to m, get fixed size message digest, _H(m)_.

MD5: 128-bit message digest
SHA-1: 160-bit message digest

This is still prone to man in the middle attacks!

**Certification authorities**
CAs binds public key to particular entity, E.

E(person, router) registers its public key with CA.
- E provides proof of identity to CA
- CA creates certificate binding E to its public key
- certificate containing E's public key digitally signed by CA: CA says "this is E's public key"

**Secure email**
Alice encrypts message with symmetric key `K_s` then send the symmetric key with the encrypted message with the key itself (encrypted via a public key).

Bob decrypts `K_s` with his private key, then using the symmetric key to decrypt the message.