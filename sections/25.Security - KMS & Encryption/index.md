# Overview

KMS, Encryption SDK, SSM Parameter Store

## Encryption

### Encryption in flight (SSL)

- Data is encrypted before sending and decrypted after receiving
- SSL certificates help with encryption (HTTPS)
- Encryption in flight ensures no MITM (man in the middle attack) can happen

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/38e6a562-27fe-4d96-8c18-ef8962b30ce3/Untitled.png)

### Server side encryption at rest

- Data is encrypted after being received by the server
- Data is decrypted before being sent
- It is stored in an encrypted form thanks to a key (usually a data key)
- The encryption / decryption keys must be managed somewhere and the server must ahve access to it

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a8936a21-41ab-4c12-ba87-bd835e0febf8/Untitled.png)

### Client side encryption

- Data is encrypted by the client and never decrypted by the server
- Data will be decrypted by a receiving client
- The server should not be able to decrypt the data
- Could leverage Envelope Encryption

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/2536891b-8301-4284-860d-d564a7089d07/Untitled.png)
