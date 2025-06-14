# Security

## How to store passwords in database?

1. **Hashing**: Store the hashed version of the password in the database. Hash functions transform passwords into unique strings that cannot be reversed to obtain the original password.

    * Advantages:
        - Difficult for hackers to retrieve passwords from plaintext form.
        - Slow and computationally expensive for attackers, making it difficult to crack passwords.
    * Disadvantages: 
        - rainbow table attacks

2. **Salting**: Add a random salt value to the password before hashing. This ensures that even if an attacker uses a rainbow table to crack the password, they will not be able to obtain the original password using precomputed tables.

    * Advantages:
        - Prevents rainbow table attacks.
        - Makes brute-force attacks slower and more difficult.
    * Disadvantages: requires additional storage space for the salt value.

3. **Stretching**: Apply a slow hashing algorithm (like bcrypt) multiple times to passwords, making it harder for attackers to crack them using brute force or rainbow tables.

    * Advantages:
        - Reduces the effectiveness of brute-force attacks.
        - Slows down hacker attempts to crack passwords.
    * Disadvantages: 
        - requires significant computational resources and memory, making it challenging to use in high-traffic applications.
## Different methods to authenticate users

### JWT
JSON Web Token (JWT) is an open standard (RFC 7519) that defines a compact and self-contained way for securely transmitting information between parties as a JSON object. This information can be verified and trusted because it is digitally signed.

#### Advantages of JWT
* **Stateless Authentication**: All user data is stored within the token, eliminating the need for server-side sessions.
* **High Scalability**: Easily supports distributed systems and microservices without sharing session state.
* **Built-in Security**: Tokens are cryptographically signed (and optionally encrypted), ensuring integrity and authenticity.
* **Flexible Structure**: Supports custom claims to include roles, permissions, tenant IDs, etc.
* **Efficient Performance**: Token verification (e.g., HMAC) is often faster than database lookups.
* **Cross-Domain Compatibility**: Works well with CORS when sent via HTTP headers, unlike cookies with domain restrictions.


#### Key Concepts of JWT

1. **Structure**: A JWT is composed of three parts:
    - **Header**
    - **Payload**
    - **Signature**

2. **Encoding**: JWTs are typically encoded using Base64 URL encoding.

3. **Use Cases**: Commonly used for authentication and information exchange.

#### How JWT Works

##### 1. **JWT Structure**

A JWT is structured as follows:

```
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c
```

- **Header**: Contains metadata about the token, such as the signing algorithm and token type.
  ```json
  {
    "alg": "HS256",
    "typ": "JWT"
  }
  ```
- **Payload**: Contains the claims or information you want to transmit (e.g., user information, expiration).
  ```json
  {
    "sub": "1234567890",
    "name": "John Doe",
    "iat": 1516239022
  }
  ```
- **Signature**: Generated by signing the header and payload with a secret key.
  ```
  HMACSHA256(
    base64UrlEncode(header) + "." +
    base64UrlEncode(payload),
    secret)
  ```

##### 2. **Flow of JWT Authentication**

Here’s how the JWT authentication process works, illustrated with a simple diagram:

```
+----------+       +----------+       +----------+
|   User   |       |   API    |       |  Server  |
+----------+       +----------+       +----------+
      |                  |                    |
      |----Login-------->|                    |
      |                  |                    |
      |                  |----Authenticate--->|
      |                  |                    |
      |                  |<----JWT Token-----|
      |                  |                    |
      |<---JWT Token-----|                    |
      |                  |                    |
      |----Request------>|                    |
      |                  |                    |
      |                  |----Validate JWT--->|
      |                  |                    |
      |<---Response------|                    |
      |                  |                    |
```

1. **User logs in**: The user sends their credentials (like username and password) to the API.
2. **API authenticates**: The API checks the credentials against the database.
3. **JWT Token issued**: If the credentials are valid, the API generates a JWT and sends it back to the user.
4. **User makes requests**: The user includes the JWT in the header of subsequent requests.
5. **API validates**: The API validates the JWT (signature, expiration, etc.) and processes the request accordingly.

> Note:
> **How Client Stores JWT**
> * **Web Apps**:
>
>    * *Options*: `localStorage`, `sessionStorage`, or **HttpOnly cookies**.
>    * *Recommendation*: Prefer **HttpOnly cookies** to mitigate XSS risks.
> * **Mobile Apps**:
>
>    * Use platform-specific **secure storage** like:
>
>        * **Keychain** (iOS)
>        * **Keystore** (Android)

#### JWT Conventions

- **Header Algorithms**: Common algorithms include:
    - **HS256** (HMAC with SHA-256)
    - **RS256** (RSA Signature with SHA-256)
    - **ES256** (ECDSA with SHA-256)

- **Claims Types**:
    - **Registered Claims**: Standard claims defined by JWT (e.g., `iss` for issuer, `exp` for expiration).
    - **Public Claims**: Can be defined at will and must be collision-resistant.
    - **Private Claims**: Custom claims created to share information between parties.

- **Expiration**: It’s recommended to set an expiration time (`exp` claim) for security reasons.

#### Security Considerations

- **Secret Management**: Keep signing secrets secure and never expose them.
- **Token Expiration**: Regularly refresh tokens to limit the impact of a stolen token.
- **HTTPS**: Always use HTTPS to protect JWTs in transit.
- **Keep Private Keys Secure**
- **Choose Strong Signing Algos**
- **Dont send sensitive Data in payload** : Payload is only base64 encoded and not encrypted

JWTs provide a scalable and efficient way to handle authentication and data sharing between systems, making them a popular choice in modern web applications.
## References

* chatgpt
* https://chatgpt.com/share/6704262b-7028-800c-a39a-2d2fe43628f0
* https://newsletter.systemdesign.one/p/how-to-store-passwords-in-database?utm_source=share&utm_medium=android&r=40ln3j&triedRedirect=true
* <https://www.youtube.com/watch?v=zt8Cocdy15c&list=PLCRMIe5FDPseEIW687mH-LZ-DMNbzAQLF&ab_channel=ByteByteGo>
* https://blog.algomaster.io/p/json-web-tokens?utm_source=share&utm_medium=android&r=40ln3j&triedRedirect=true
* https://jwt.io/

