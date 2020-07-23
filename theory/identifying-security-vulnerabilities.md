# Identifying Security Vulnerabilities

Summary of https://www.coursera.org/learn/identifying-security-vulnerabilities

**Week 1**

## Fundamental Concepts

### Confidentiality

Restriction on access, protect privacy and proprietary info:

- Encryption in Transit
- Encryption at Rest
- Access control lists (ACL)

### Integrity

Data meets quality expectations, is consistent and accurate.
(Make sure the data con not be manipulated)

### Availability

User access to resources.

- I.e. a DDOS Attacks is such a thread, a CDNM can mitigate that issue

## Thread Modeling

Book suggestion: Threat Modeling, Designing for Security by Adam Shostack

### Goals

Data flow diagram to

- Determine trust boundaries
- Understand the system
- Find potential threads
- Prioritize and fix threads

### Trust Boundaries Diagram

1. Determine trust boundaries (TB)
2. How does the data flow from boundary to boundary?

i.e.:
```
Untrusted     Trusted       Untrusted
external      internal      external
network       network       network

Client  ) <=> Server <=>  ( Database (external)
  TB 1  )                 ( TB2
```

After the data flow diagram use:

### STRIDE

A tool used to brainstorm potential threads

- S – Spoofing (pretending to be someone else) (i.e. unauthorized users have to login and access level is in token)
- T – Tampering (modify system data) (i.e. encryption can prevent tampering between data flow, also need encryption at rest (when saved))
- R – Repudiation (lie about changes) (i.e. logging mechanisms for requests)
- I – Info Leak (get without allowance) (i.e. encryption as for tampering)
- D – Denial of Service (i.e. requires further testing to ensure data is available and load balancer could be used)
- E – Elevation of Privilege (i.e. login to get token, tamper resistent token mechanisms)

Goals: Determine what should be protected, then define user roles, privileges and the attack surface of systems

#### Example

- Spoofing: i.e. unauthorized users have to login and access level is in token
- Tampering: i.e. encryption can prevent tampering between data flow, also need encryption at rest (when saved)
- Repudiation: i.e. logging mechanisms for requests
- Info Leak: i.e. encryption as for tampering
- Denial of Service: i.e. requires further testing to ensure data is available and load balancer could be used
- Elevation of Privilege: i.e. login to get token, tamper resistent token mechanisms

In this example, based on stride we know that we need encryption, authorization, logging, code reviews.

## Applied Cryptography – Overview

Book suggestion: Cryptography Engineering, Design principles and practical applications by Ferguson, Schneider and Kohno

- Encrypt data
- Authenticate sensitive data
- Prevent data tampering

### Symmetric Key Encryption

Sender and receiver use the same private key

#### Block Ciphers

Algorithm used to take a plain text message (you want to keep secret), put it through the block cipher and get out the cipher text output (which can only be decrypted with a private key).

- Plain text => block cipher => cipher text output
- Output has no relation to input
- Block = fixed lenght of bits

##### Using Block Ciphers

Recommended: encryption method AES with 256 Bit size in CBC mode with a random initialization vector

###### ECB Mode

- **Don't use it.**
- It will map a block of text to a matching block cipher length
- It will have a relation to the input and leak information

###### OFB Mode

?

###### CTR Mode

- Use Block Cipher as stream cipher to avoid the need of padding
- Not always good

###### CBC Mode

- **The currently recommended mode**
- Add padding to the input text to match the block cipher length before encrypting
- Randomize message by using previous cipher block results

### Symmetric key encryption

- Sender and receiver have the same private key
- Example: AES

#### Asymmetric Cryptography

- Ability to agree on a shared secret key in order to perform symmetric encryption.
- Shared public key
- Sender uses public key to encrypt messages
- Receiver has public and private key to decrypt message
- Public key can only encrypt private key can only decrypt (kinda like ssh)
- 1:1 relation ship between public and private key

Receiver => Public Key => Public Key Repository => Sender  
Receiver generates pub & priv key

- Sender: Plaintext => Receiver's public key used in encryption algorithm => ciphertext
- Receiver: ciphertext => Receiver's private key used in decryption algorithm => Plaintext

### Hash Functions

- Used to map data (of arbitrary length) to other data (of fixed length)
- Don't guarantee uniqueness (by default)

### Cryptographic Hash Functions

- Same message = Same Hash
- No Hash collision
- Relatively fast
- No relationship between Hashes (or input)

- If you generate a message and a hash, then modifying that message, then generate another hash from that modified message, you should not see any relationship between the old and new hash.
- You can't get the message back from a hash unless you try to generate all possible messages.
- The same message always results in the same hash as the first property.

### Message Authentication Code

- Used to ensure that message came from sender and/or if it was tampered.
- Example: HMAC

A message authentication code is a tag, or, effectively, a piece of data that has always the same length of bits used to determine whether a message actually came for the sender that we had expected.

i.e. signatures in JWTs

#### Replay Attacks

- I.e. an attacker could re-use the same message authentication code as send previously.
- A simple way to protect against replay attacks in this scenario is to add a message number on which the sender and receiver agree on to tag messages to make sure that no key can be re-used


---
[Spoofing, Tampering, Repuding, Replay Attacks]
