# Spring Security

## Security Basics

| Aspect               | Encoding                                                | Encryption                                                              | Hashing                                                                 |
|----------------------|---------------------------------------------------------|-------------------------------------------------------------------------|-------------------------------------------------------------------------|
| Purpose              | Data representation or transformation only              | Data confidentiality and privacy                                        | Data integrity and verification                                         |
| Reversibility        | Easily reversible (not designed for security)           | Reversible with the right key (decryption)                              | Irreversible (one-way transformation)                                   |
| Security Focus       | Not intended for security                               | Designed for data security                                              | Designed for data integrity                                             |
| Key Usage            | No keys required                                        | Requires keys (public-private key pair or shared secret)                | No keys required                                                        |
| Output Length        | Output length is typically the same as input            | Output length can vary (depends on encryption algorithm)                | Fixed output length (hash size)                                         |
| Use Cases            | URL encoding, Base64 encoding, character set conversion | Secure data storage, secure communication                               | Password storage, data verification                                     |
| Example Algorithms   | URL encoding, Base64 encoding                           | AES, RSA, DES, Blowfish                                                 | MD5, SHA-1, SHA-256, bcrypt, scrypt                                     |
| Security Strength    | No security strength (not designed for security)        | Provides strong security when used properly                             | Provides data integrity but not privacy                                 |
| Collision Resistance | Not applicable                                          | Critical (preventing two different inputs from producing the same hash) | Critical (preventing two different inputs from producing the same hash) |
