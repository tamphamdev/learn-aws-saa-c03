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

## AWS KMS (Key Management Service)

- Anytime you hear “encryption” for an AWS service, it’s most likely KMS
- AWS manages encryption keys for us
- Fully integrated with IAM for authorization
- Easy to control access to your data
- Able to audit KMS Key usage using CloudTrail
- Seamlessly integrated into most AWS service (EBS, S3, RDS, SSM…)
- **Never ever store your secrets in plaintext, esspecially in your code**
  - KMS Key encryption also available through API calls (SDK, CLI)
  - Encrypted secrets can be stored in the code / environment variables

### KMS Keys Types

- **KMS Keys is the new name of KMS Customer Master Key**
- **Symmetric (AES-256 keys)**
  - Single encryption key that is used to Encrypt and Decrypt
  - AWS service that are integrated with KMS use Symmetric CMKs
  - You never get access to the KMS Key unencrypted (must call KMS API to use)
- **Asymmetric (RSA & ECC key pairs)**
  - Public (Encrypt) and Private Key (Decrypt) pair
  - Used for Encrypt/Decrypt, or Sign/Verify operations
  - The public key is downloadable, but you can’t access the Private Key unencrypted
  - Use case: encryption outside of AWS users who can’t call the KMS API

### AWS KMS (Key Management Service)

- Types of KMS Keys:
  - AWS Owned Keys (free): SSE-S3, SSE-SQS, SSE-DDB (default key)
  - AWS Managed Key: **free** (aws/service-name, example: aws/rds or aws/ebs)
  - Customer managed keys created in KMS: **$1 / month**
  - Customer managed keys imported (must be symmetric key): $1 / month
  - - pay for API call to KMS ($0.03 / 10000 calls)
- Automatic Key rotation:
  - AWS-managed KMS Key automatic every 1 year
  - Customer-managed KMS Key: (must be enabled) automatic every 1 year
  - Imported KMS Key: only manual rotation possible using alias

### Copying Snashots across regions

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/1d2cf18e-73cd-42ad-95d7-e746728d823d/Untitled.png)

### KMS Key Policies

- Control access to KMS keys, “similar” to S3 bucket policies
- Diffirence: you cannot control access without them

- **Default KMS Key Policy:**
  - Created if you don’t provide a specific KSM Key Policy
  - Complete access to the key to the root user = entire AWS account
- **Custom KMS Key Policy:**
  - Define users, roles that can access the KMS key
  - Define who can administer the key
  - Useful for cross-account access of your KMS Key

### Coying Snapshots across accounts

1. Create a Snapshot, encrypted with your own KMS Key (Customer Managed Key)
2. **Attach a KMS Key Policy to authorize across-account access**
3. Share the encrypted snapshot
4. (in target) Create a copy of the Snapshot, encrypt it with a CMK in your account
5. Create a volume from the snapshot

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/91330eae-043a-4ea5-82b7-71a1011b8073/Untitled.png)
