# Security

## How to store passwords in database?

* Dont store passwords as plain strings, since it will be visible to access to the database and will be easily compromised in case of attacks.
* Use a modern hashing function and store the hash in the database, since hash function are one way functions.
* Also add salting to increased security. Salts can be stored in database as plain strings.

## References

* <https://www.youtube.com/watch?v=zt8Cocdy15c&list=PLCRMIe5FDPseEIW687mH-LZ-DMNbzAQLF&ab_channel=ByteByteGo>

