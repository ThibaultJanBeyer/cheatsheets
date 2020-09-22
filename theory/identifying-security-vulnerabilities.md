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

- I.e. a DDOS Attacks is such a threat, a CDN can mitigate that issue

## Threat Modeling

Book suggestion: Threat Modeling, Designing for Security by Adam Shostack

### Goals

Data flow diagram to

- Determine trust boundaries
- Understand the system
- Find potential threats
- Prioritize and fix threats

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

A tool used to brainstorm potential threats

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

- User displayed errors should not leak sensitive data
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

# HTTP & Sessions

- Whenever a client communicates with server: first it makes a TCP/IP request and then sends over the HTTP data
- Request: HTTP1 shows request in human readable form: Method Path Version Headers Body. HTTP2 however will require more work to be human readable
- Response: Version Status-code Status-message Headers
- HTTP is stateless. The state is added using cookies

## Vulnerability

- Without encryption, an attacker can sniff the session headers from the users HTTP calls and then re-use the same to authenticate
- The new york times article explains [HTTP sniffing in details](https://www.nytimes.com/2011/02/17/technology/personaltech/17basics.html)

## HSTS

- HSTS: HTTP strict transport security
- Tells the client to always use HTTPS to communicate

## REST

- Representational State Transfer
- RESTful resource: Create Read Update Delete (GET, POST, DELETE)
- Use web application frameworks to handle Auth & Session, don't build your own
- Web frameworks should at least use HttpOnly on cookies, set path params correctly, Prevent session tokens from being included in URLs

## Common types of sessions

- Browser forgets about cookie on shutdown
- Session ID is encrypted within cookie

### Persistent sessions

- Remember token (random string of digits)
- Server stores remember token hash digest in DB
- Send Remember token as cookie
- Send encrypted user ID as cookie
- Client sends both token to the server, the server then decrypt the userID and remember token, finds the user in the DB and checks if the remember token is the correct one

### Generic Guidelines on Sessions

- UserIDs should be unique and case sensitive
- Passwords should have a good length, complexity and secure storage (never plaintext password)
- Auth pages accessed using TLS (Transport Layer Security HTTPS)
- Force users to re-auth accessing a sensitive feature (prevents session hijacking)
- Rate-limit/Lock-out after several failed logins (prevent brute-force)

**Week 3**

## Authentication

### Handling error messages

- Via error messages, an attacker could get hints on username/passwords
- It might also be possible to guess incorrect passwords based on the time the server takes to respond
- On Auth, make error messages as generic as possible (i.e. Login failed: Incorrect userID or incorrect password)
- Log login failures + timestamp and username/account
- Log account lockouts + timestamp and username/account

### Session management

- HTTP is stateless. Cookies are used to handle session state.
- Session generation: is the token predictable?
- Session handling: is the token data leaked?
- Session termination: was the session removed completely?

#### Preventions

- Create strong tokens (that can't be brute-forced) and transport them only via HTTPs. Never as part of the URL.
- Sessions should expire quickly (i.e. 15mins)
- Prevent logins from more than one place
- Set cookie scope as restrictive as possible (domain & path)
- HTTP-Only Cookies
- Clear out session information on log-out. If server receives an invalid request from a client, the server should force the client to re-auth

#### Discretionary Access Control

Where owners of resources grant access to other users to access their resources.
If those owners of those resources don't explicitly grant access to those resources, access is automatically denied.
(i.e. google-drive)

#### Access Controls (Role Based)

- Vertical attack: user escalates privileges from regular user to admin
- Horizontal attack: access more resources than the user should
- Design Phase: specify exactly who has access to what (i.e. employee, PUT, /emp1/resource.jpg):
- - Principal: Who/What
- - Access type: action on resource (think RESTful)
- - Resource: file, page, endpoint
- Define Role/Group: regular user, admin, guest,…

##### Securing

- Explicitly design your accesses (roles, groups)
- Don't trust any user controlled data for access rights. Always validate it
- Centralized component that handles access controls
- Any request should be checked using that centralized module
- Restrict special pages with an IP whitelist
- For critical actions, re-auth the user

#### Secure session-ids against brute-force

- Session-IDs should be as random as possible not too short
- Session-IDs should have a bit length longer than 128 bits
- Associate session IDs by TLS connections in a 1:1 relationship
- See https://owasp.org/www-community/vulnerabilities/Insufficient_Session-ID_Length
- https://betterexplained.com/articles/understanding-the-birthday-paradox/

#### Session-Fixation

- Attacker tries to trick client to use session id i.e. through phishing emails, specially crafted login page with a hidden field
- The client passes the session token when logging in, the server accepts it as valid, the attacker can now use the session token himself
- The server-side should invalidate a token if it's given by the browser on login
- On successful login, a new session token should be given

#### Logging

##### Why?

- Debugging
- Prevent repudiation
- Detecting attacks

##### What?

- Authentication successes & failures
- Authorization successes & failures
- Session management failures
- Account lock-outs

##### How?

- Hold logs on the app for a short amount of time (1-2 days) (i.e. save to file)
- Send logs to a central logging system over tls and retain for a longer time (1-2 years)

**Week 4**

## Data Exposures

Happen when data is not protected enough:
- i.e. the Cryptography on the passwords is too weak (or no encryption at all)
- When personal identifiable information is used to compose the session ID
- Improperly storing password
- Using HTTP instead of HTTPs

## OWASP Mitigation Strategies

- Know your data (Requirements list)
- Build security in your softwares requirements
- Be aware of caches and how to disable them
- Understand Cryptography
- Do not store sensitive data unless needed

## Issue 1: Using PII to Compose Session IDs

- May be found in application logs
- Might not be encrypted enough
- Attackers could determine the pattern and make up IDs for users

Solution:

- Generate hard to guess token
- Use random generators
- Do not use any user identification data

## Issue 2: Not Encrypting Sensitive Information

- Data in transit. (i.e when attacker has access to http, they could easily steal the info)
- Data at rest. (an attacker gains access to the DB he can easily steal sensitive data)

Solution:

- Know your data (i.e. Categorize our data based on their sensitivity levels)
- Don't store date you don't have to (i.e. credit-card numbers)
- Use standard cryptographic measures (i.e. AES block ciphers in CBC mode with random IV)

## Issue 3: Improperly Storing Passwords

Solution:

- Do not store the actual plaintext password but rather a hash output that can't be reverted
- Salting and Hashing of passwords
- Using stronger encryption to slow down brute-force (i.e. PBKDF2, bcrypt)
- Use 2FA Auth
- Use generic error messages (i.e. "Invalid username or password entered")

### Salting and Hashing

- If you use the same hash function for each user, you might end up with 2 or more hashes in your database that are identical. Which is a hint for attackers to have it easier to brute-force.
- This is where salting comes into play. "Salt" is a fixed length, randomly generated string that is concatenated with the plain text password before performing the hash calculation.
- Generate the salt using a CSPRNG (Cryptographically Secure Pseudo-Random Number Generator)
- Salt length need to be at least the same length as the output of the hash function (i.e. 256 bits is you use SHA256)
- Generate a new one-time salt for each user (do not re-use the same salt)

### Storing a password for a new user

1. Generate a salt using CSPRNG
2. Concatenate the salt and the plaintext password
3. Hash the result of step 2 using a slow hashing function (i.e. PBKDF2 or bcrypt). To make brute-force too slow to be useful 
4. Store the hash and the salt in the DB along with the username

### Authenticate the user

1. Get the salt Associated with the username
2. Concatenate the salt and the plaintext password
3. Hash the result of step 2 again using the same hash function as when generating
4. Compare the result of step 3 with what's in the DB
