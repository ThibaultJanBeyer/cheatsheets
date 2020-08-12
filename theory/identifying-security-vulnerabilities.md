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

- I.e. a DDOS Attacks is such a thread, a CDN can mitigate that issue

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

## OWASP Top 10 Proactive controls

https://owasp.org/www-project-proactive-controls/

- Controls and recommendations to apply during software development
- Gives awareness of various things that might go wrong
- Standards for testing security controls: OWASP ASVS
- Additional activities during development: OWASP SAMM, BSIMM (survey on what other companies do for security)

### 1. Define Security Requirements

- Done at the beginning or whenever new changes are adeed.
- Define what is required to secure

### 2. Leverage Frameworks and Requirements

- Use existing and proven frameworks to help getting up to speed quickly

### 3. Secure Database Access

- Don't trust any query done by the user
- Harden the DB security settings (in general the DB comes unsecured)
- Secure auth and communication

### 4. Encode and Escape data

- Prevent Injection and XSS

### 5. Validate all inputs

- Only properly formatted data can be processed
- Data escaping & Query parametrization

### 6. Implement a Digital Identity

- Authentication (NIST-800-63b reading on levels (Password only, Multi Factor, Hardware crypto modules))
- Who can access?

### 7. Enforce access control

- What can be accessed?
- Principle of least privilege (allow only just enough access and privileges)

### 8. Protect data everywhere

- Classify data based on sensitivity
- Encrypt data on rest and in transit
- Keep app secrets secret (i.e. use a password vault)

### 9. Implement security logging and monitoring

- Just enough info: timestamp, user id
- Forward logs to a central service

### 10. Handle all errors and exceptions

- Use displayed errors should not leak sensitive data
- Log enough information to be able to debug

## OWASP Top 10 Application security risk

https://owasp.org/www-project-top-ten/


**Week 2**

- https://www.exploit-db.com/
great resource for finding known exploits and learning how exploits are built

## Injection Problems (SQL)

- Untrusty data => command or query string => interpreter interprets the data as command
- Leads to read/writes in the database

Book: The Web Application Hackers Handbook

### Principle of least privilege

- Default role that can do no harm
- I.e. web app users should never be able to become root users (keep them separate)

### Prepared Statements

- Input always treated as data, never as statement
- Compiled before using
- Generates a static query (never concatenate it)
example: `password = ? AND username = ?` then `x.setString(0, $password) x.setString(1, $username)` instead of `password = $password AND username = $username`

### Stored procedures

- Query that is created and stored in the database, when it needs to run it's just called by the application
- Make sure to only use static queries (with parameters, similar to prepared statements)

### Query whitelisting

- Validating that user control data follows a known specification or is within a set of data that is acceptable
i.e. instead of `ASC and DESC` use boolean types `true and false`

## Cross-Site Scripting (XSS)

- Inject JavaScript in a web-app that is interpreted by the browser
- Modifying the DOM and including a user-defined parameter
- Leads to impersonation

### HTTP

- Protocol to communicate from a client to a server
- Human readable
- Stateless. Current requests have no relationship to previous requests

#### Cookies

- Cookies add state to HTTP (and sessions)
- JavaScript can read/write cookies
- Server-side sets the cookie to HttpOnly, which makes it unavailable from JavaScript on the client
- Cookie Scope: you can specify the domain and path that can access the cookie
- Origin: Protocol, Domain and Port.
- [Same origin policy](https://blog.thibaultjanbeyer.com/cors-cross-origin-web/).

### Reflected Cross-Site Scripting

- Malicious data is injected i.e. via URL params that are used to generate parts of the DOM.

### Stored Cross-Site Scripting

- Malicious data is injected via data i.e. POST data that is stored on a DB which is used to generate content on the page.

### DOM-Based Cross-Site Scripting

- Everything happens on the client side
- DOM is modified and data injected via modification
- Similar to the Reflected just that in the DOM-Based it never reaches the server but is directly executed on the DOM

## Command Injection

- OS command in command string
- Can lead to system compromise

## OWASP Vulnerability testing tools ("learn to hack")

- [BURP](https://portswigger.net/burp) + [Webgoat](https://owasp.org/www-project-webgoat/)
- Burp is a tool to exercise man-in-the-middle attacks
- Webgoat is a website/software that you can run locally to train various attacks on

## OWASP Positive Prevention Model for Preventing Cross-Site Scripting Vulnerabilities

- Idea: treat a HTML page like a template with slots where untrusty data can be put. Other places are not allowed.
- Each slot has different security rules
- Use secure encoding libraries (i.e. OWASP Java Encoder Project (for Java))
- Never insert untrusted data except the allowed locations
- Never in script tags, within a script, in an attribute name, in a tag name nor directly in CSS

### RULEs

- Escape before putting it into html element content
- Escape before putting it into html common attributes
- Escape inserting untrusted data into javascript (however, generally don't do that because some scripts can't be escaped)
- First HTML Escape untrusted data then JavaScript Escape it. (i.e. °.innerHTML = lib.encodeJS(lib.encodeHTML(untrusted))))
- JavaScript Escape before putting untrusted data into HTML attributes (i.e. °.setAttribute('v', °.encodeJS(untrusted)))
- Avoid including untrusted data in your JavaScript
- JavaScript Escape before putting untrusted data in CSS (i.e. °.backgroundImage = url(°.encodeJS(°.encodeURL(untrusted))))
- Populate the DOM using safe functions or properties (i.e. °.textContent = untrusted)
- Always use HTTP-Only cookies when you can

### Controls

- Define security requirements
- Leverage security frameworks and libraries
- Secure data access (DB secure configuration)
- Don't trust DB queries that have user input inside
- Encode and escape data
- Validate all user inputs
- Implement security logging and monitoring

## Command injection problems

- Web-app uses user data as input into a command passed to the OS
- I.e. for Java: `exec()`, `getParameter`, `ProcessBuilder.start()`, `setAttribute putValue getValue`, `java.net.Socket java.io`

---
[Spoofing, Tampering, Repuding, Replay Attacks]
