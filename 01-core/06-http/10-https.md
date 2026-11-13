# HTTPS (Hypertext Transfer Protocol Secure)

HTTPS (Hypertext Transfer Protocol Secure) is the secure version of HTTP. It uses TLS (Transport Layer Security) to protect communication between a client, such as a web browser, and a server.

HTTPS provides three essential security properties:

* **Confidentiality:** Prevents unauthorized parties from reading transmitted data.
* **Integrity:** Helps detect unauthorized modification of transmitted data.
* **Authentication:** Allows clients to verify the identity of the server they are communicating with.

HTTPS is widely used for:

* User authentication.
* Online banking.
* Payment processing.
* REST APIs.
* Private communication.
* Secure application data transfer.
* Modern web applications.

The standard HTTPS URL scheme is:

```text
https://example.com
```

By default, HTTPS uses TCP port `443` with HTTP/1.1 and HTTP/2. HTTP/3 uses QUIC over UDP, commonly on port `443`.

### 🖆 HTTP vs HTTPS

| Feature | HTTP | HTTPS |
| --- | --- | --- |
| Full name | Hypertext Transfer Protocol | Hypertext Transfer Protocol Secure |
| Encryption | No transport encryption by default | Uses TLS |
| Confidentiality | Not provided by HTTP itself | Provided by TLS |
| Integrity | Not provided by HTTP itself | Provided by TLS |
| Server authentication | Not provided by HTTP itself | Provided through certificate validation |
| Default port | 80 | 443 |
| URL scheme | `http://` | `https://` |

# 🗁 Why HTTPS Is Important

HTTP transfers application data without providing transport encryption.

This means an attacker who can observe an unprotected network connection may be able to read or modify transmitted information.

Consider a user submitting login credentials over an unencrypted HTTP connection.

```text
Browser                  Network                  Server
   |                        |                        |
   |------ Username ------->|----------------------->|
   |------ Password ------->|----------------------->|
   |                        |                        |
   |                  Data Can Be                    |
   |                  Intercepted                    |
```

HTTPS protects the communication channel using TLS.

```text
Browser                  Network                  Server
   |                        |                        |
   |------ TLS Data ------->|----------------------->|
   |                        |                        |
   |              Encrypted Communication            |
   |                        |                        |
   |<----- TLS Data --------|<-----------------------|
```

An attacker observing the encrypted connection cannot normally read the application data or modify it without detection.

### 🖆 Benefits of HTTPS

* Protects credentials during transmission.
* Protects sensitive application data.
* Helps prevent network-level tampering.
* Authenticates the server's identity.
* Protects cookies and session identifiers in transit when properly configured.
* Enables modern browser features that require secure contexts.
* Improves trust between users and websites.
* Supports secure communication for web APIs.

# 🗁 What Is TLS?

TLS (Transport Layer Security) is a cryptographic protocol that provides secure communication over a network.

HTTPS uses TLS to protect HTTP communication.

TLS provides:

* Encryption.
* Message integrity.
* Authentication.
* Secure cryptographic key establishment.

TLS is the successor to SSL (Secure Sockets Layer).

SSL is obsolete and should not be used in modern deployments.

### 🖆 TLS Versions

| Version | Status |
| --- | --- |
| SSL 2.0 | Obsolete |
| SSL 3.0 | Obsolete |
| TLS 1.0 | Deprecated |
| TLS 1.1 | Deprecated |
| TLS 1.2 | Widely supported and secure when properly configured |
| TLS 1.3 | Modern recommended version |

TLS 1.3 simplifies the handshake and removes several outdated cryptographic mechanisms.

For modern applications, TLS 1.3 is preferred, with TLS 1.2 retained where compatibility requires it.

### 🖆 TLS vs SSL

SSL and TLS are related protocols, but they are not interchangeable in modern deployments.

| SSL | TLS |
| --- | --- |
| Older protocol family | Successor to SSL |
| Insecure and obsolete | Modern secure transport protocol |
| Should not be enabled | TLS 1.2 and TLS 1.3 are commonly used |
| Historical terminology | Current technical terminology |

The term "SSL certificate" is still widely used in the industry, but modern HTTPS certificates are used with TLS.

# 🗁 How HTTPS Works

HTTPS establishes a secure TLS connection before exchanging HTTP application data.

The general process involves:

* Connecting to the server.
* Negotiating TLS parameters.
* Validating the server certificate.
* Establishing shared encryption keys.
* Transmitting encrypted HTTP data.

### 🖆 Basic HTTPS Workflow

```text
Browser                         Server
   |                               |
   |------ TCP Connection -------->|
   |                               |
   |-------- TLS Handshake ------->|
   |                               |
   |<------- TLS Response ---------|
   |                               |
   |  Validate Server Certificate  |
   |                               |
   |------ Finish Handshake ------>|
   |                               |
   |======= Secure Connection =====|
   |                               |
   |-------- HTTP Request -------->|
   |                               |
   |<------- HTTP Response --------|
```

The exact handshake differs between TLS versions.

### 🖆 Step-by-Step Overview

**Connection establishment**

The client establishes a transport connection to the server.

For HTTP/1.1 and HTTP/2 over TCP, this normally involves a TCP connection.

For HTTP/3, QUIC handles transport establishment over UDP.

**TLS negotiation**

The client and server negotiate cryptographic parameters, including the TLS version and supported cryptographic algorithms.

**Server authentication**

The server provides its certificate chain.

The browser validates the certificate according to its trust configuration, hostname, validity period, and other applicable requirements.

**Key establishment**

The client and server establish shared cryptographic secrets.

These secrets are used to derive symmetric encryption keys.

**Secure communication**

After the handshake, HTTP messages are transmitted through the encrypted TLS connection.

# 🗁 TLS Handshake

The TLS handshake establishes the cryptographic parameters and shared secrets required for secure communication.

### 🖆 TLS 1.2 Handshake

A simplified TLS 1.2 handshake using an ephemeral Diffie-Hellman key exchange looks like this:

```text
Client                                      Server
  |                                            |
  |------------ ClientHello ----------------->|
  |                                            |
  |<----------- ServerHello ------------------|
  |<----------- Certificate ------------------|
  |<----------- ServerKeyExchange ------------|
  |<----------- ServerHelloDone --------------|
  |                                            |
  |------------ ClientKeyExchange ----------->|
  |------------ ChangeCipherSpec ------------>|
  |------------ Finished -------------------->|
  |                                            |
  |<----------- ChangeCipherSpec -------------|
  |<----------- Finished ---------------------|
  |                                            |
  |========== Secure Communication ===========|
```

The exact messages depend on the selected cipher suite and authentication method.

### 🖆 TLS 1.3 Handshake

TLS 1.3 simplifies the handshake.

A typical full handshake looks like this:

```text
Client                                      Server
  |                                            |
  |------------ ClientHello ----------------->|
  |                                            |
  |<----------- ServerHello ------------------|
  |<----------- EncryptedExtensions ---------|
  |<----------- Certificate -----------------|
  |<----------- CertificateVerify -----------|
  |<----------- Finished --------------------|
  |                                            |
  |------------ Finished -------------------->|
  |                                            |
  |========== Secure Communication ===========|
```

TLS 1.3 normally completes a full handshake in one round trip after transport establishment.

It can also support 0-RTT early data in certain resumed connections, but early data has replay considerations and should not be used indiscriminately for operations that must not be repeated.

### 🖆 ClientHello

The client begins by sending a `ClientHello` message.

It includes information such as:

* Supported TLS versions.
* Supported cipher suites.
* Key share information.
* Supported extensions.
* Server Name Indication (SNI), when applicable.
* Application-Layer Protocol Negotiation (ALPN), when applicable.

### 🖆 ServerHello

The server responds with a `ServerHello`.

It selects compatible cryptographic parameters and provides its key share information.

### 🖆 Certificate

The server sends its certificate chain so that the client can authenticate the server's identity.

### 🖆 CertificateVerify

The server proves possession of the private key associated with its certificate by signing handshake information.

### 🖆 Finished

The handshake finishes with cryptographic verification messages.

Both parties verify the handshake transcript and establish the secure connection.

# 🗁 Digital Certificates

A digital certificate is an electronic document that associates a public key with an identity.

HTTPS servers use certificates to establish their identity to clients.

Certificates are commonly issued by Certificate Authorities (CAs).

### 🖆 Certificate Contents

A typical X.509 certificate contains information such as:

* Subject identity.
* Public key.
* Issuer information.
* Validity period.
* Subject Alternative Names (SANs).
* Digital signature.
* Certificate extensions.

Example certificate information:

```text
Subject:
  CN = example.com

Issuer:
  Example Certificate Authority

Subject Alternative Name:
  DNS:example.com
  DNS:www.example.com

Public Key:
  RSA or ECDSA

Validity:
  Not Before: ...
  Not After: ...
```

The Common Name (CN) is a historical certificate field. Modern hostname validation primarily relies on Subject Alternative Name (SAN).

### 🖆 Public Key and Private Key

Asymmetric cryptography uses a related pair of keys:

**Public key**

* Included in the certificate.
* Can be shared publicly.
* Used in cryptographic operations such as signature verification and key establishment.

**Private key**

* Kept secret by the server.
* Used to prove possession of the certificate's private key.
* Must be protected from unauthorized access.

The private key should never be distributed with the certificate.

### 🖆 Certificate Validation

A browser validates a server certificate by checking several properties:

* The certificate is within its validity period.
* The hostname matches the requested domain.
* The certificate chain leads to a trusted root CA.
* Certificate signatures are valid.
* Applicable certificate constraints are satisfied.
* Revocation information and browser-specific policies are considered.

If validation fails, the browser may display a security error.

# 🗁 Certificate Authorities

A Certificate Authority is an organization trusted to issue and sign digital certificates.

Browsers and operating systems maintain trusted root CA certificates.

A server normally provides its certificate and the required intermediate certificates so that the client can build a trusted certificate chain.

### 🖆 Certificate Chain

A typical certificate chain looks like this:

```text
Trusted Root CA
      |
      v
Intermediate CA
      |
      v
Server Certificate
      |
      v
example.com
```

The browser validates the chain back to a trusted root.

The root certificate is commonly distributed through the browser or operating system's trust store.

### 🖆 Types of TLS Certificates

**Domain Validated (DV)**

A DV certificate verifies control over a domain.

It is commonly used for websites and APIs.

**Organization Validated (OV)**

An OV certificate includes additional organization identity validation.

**Extended Validation (EV)**

An EV certificate requires a more extensive organization verification process.

The encryption strength is not inherently determined by whether a certificate is DV, OV, or EV.

### 🖆 Wildcard Certificates

A wildcard certificate can cover multiple subdomains at a particular domain level.

Example:

```text
*.example.com
```

It can cover:

```text
www.example.com
api.example.com
mail.example.com
```

It does not automatically cover the root domain:

```text
example.com
```

The root domain must be included separately if it also needs certificate coverage.

# 🗁 Public Key Infrastructure (PKI)

Public Key Infrastructure (PKI) is the system of technologies, policies, and processes used to create, distribute, validate, and manage digital certificates and public keys.

PKI supports the trust model used by HTTPS.

### 🖆 Main PKI Components

| Component | Responsibility |
| --- | --- |
| Certificate Authority (CA) | Issues and signs certificates |
| Registration Authority (RA) | Performs identity or domain validation functions |
| Digital Certificate | Associates an identity with a public key |
| Public Key | Used for public-key cryptographic operations |
| Private Key | Kept secret and used for signing or other cryptographic operations |
| Certificate Repository | Distributes certificate information |
| Trust Store | Contains trusted root certificates |

### 🖆 How PKI Supports HTTPS

```text
Website Owner
     |
     | Request Certificate
     v
Certificate Authority
     |
     | Validate Domain
     v
Issue Signed Certificate
     |
     | Install Certificate
     v
Web Server
     |
     | Present Certificate
     v
Browser
     |
     | Validate Certificate Chain
     v
Trusted HTTPS Connection
```

# 🗁 Symmetric and Asymmetric Encryption

TLS uses both symmetric and asymmetric cryptographic techniques.

### 🖆 Symmetric Encryption

Symmetric encryption uses the same secret key for encryption and decryption.

Example:

```text
Plaintext
    |
    | Encrypt using Secret Key
    v
Ciphertext
    |
    | Decrypt using Same Key
    v
Plaintext
```

Common symmetric encryption algorithms include:

* AES.
* ChaCha20.

Symmetric encryption is efficient and is used to protect application data after the TLS handshake.

### 🖆 Asymmetric Cryptography

Asymmetric cryptography uses a public key and a private key.

It supports operations such as:

* Digital signatures.
* Signature verification.
* Public-key-based key establishment.

Common algorithms include:

* RSA.
* ECDSA.
* Ed25519 in other cryptographic contexts.

TLS 1.3 uses modern key exchange and signature mechanisms. It does not use RSA key transport for establishing session keys.

### 🖆 Why TLS Uses Both

Asymmetric cryptography supports authentication and secure key establishment.

Symmetric cryptography efficiently protects the actual data exchanged between the client and server.

```text
TLS Handshake
     |
     | Authenticate Server
     | Establish Shared Secrets
     v
Derive Symmetric Keys
     |
     v
Encrypt HTTP Communication
```

# 🗁 TLS Cipher Suites

A cipher suite defines a set of cryptographic algorithms used by TLS.

The exact components depend on the TLS version.

### 🖆 TLS 1.2 Cipher Suite

A TLS 1.2 cipher suite can specify:

* Key exchange.
* Authentication.
* Symmetric encryption.
* Message authentication or integrity protection.

Example:

```text
TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256
```

This identifies a combination involving:

* ECDHE key exchange.
* RSA authentication.
* AES-128-GCM authenticated encryption.
* SHA-256 in the suite's associated TLS functions.

### 🖆 TLS 1.3 Cipher Suite

TLS 1.3 cipher suites specify the symmetric AEAD algorithm and hash algorithm used by the TLS handshake.

Example:

```text
TLS_AES_128_GCM_SHA256
```

TLS 1.3 negotiates key exchange groups and signature algorithms separately.

This is different from the naming and structure of many TLS 1.2 cipher suites.

# 🗁 HTTPS Ports

HTTPS commonly uses TCP port `443`.

The port depends on the HTTP version and transport protocol.

| Protocol | Transport | Common Port |
| --- | --- | --- |
| HTTP/1.1 | TCP | 80 |
| HTTP/2 | TCP | 443 for HTTPS |
| HTTP/3 | QUIC over UDP | 443 |

### 🖆 Port 443

When a browser visits:

```text
https://example.com
```

it normally connects to port `443` unless another port is explicitly specified.

Example:

```text
https://example.com:8443
```

This URL uses port `8443`.

# 🗁 HTTPS and HTTP Versions

HTTPS can be used with HTTP/1.1, HTTP/2, and HTTP/3.

TLS provides transport security, while the HTTP version defines application protocol behavior.

### 🖆 HTTP/1.1 over TLS

HTTP/1.1 can operate over a TLS connection.

```text
HTTP/1.1
    |
    v
TLS
    |
    v
TCP
    |
    v
IP
```

### 🖆 HTTP/2 over TLS

HTTP/2 commonly operates over TLS using ALPN to negotiate the HTTP protocol.

```text
HTTP/2
   |
   v
TLS
   |
   v
TCP
   |
   v
IP
```

### 🖆 HTTP/3 over QUIC

HTTP/3 uses QUIC, which integrates TLS 1.3 handshake mechanisms into its secure transport.

```text
HTTP/3
   |
   v
QUIC + TLS 1.3
   |
   v
UDP
   |
   v
IP
```

HTTP/3 does not use the traditional TLS-over-TCP architecture.

# 🗁 SNI and ALPN

SNI and ALPN are TLS extensions that help clients and servers negotiate the appropriate connection behavior.

### 🖆 Server Name Indication (SNI)

SNI allows a client to indicate the hostname it wants to connect to during the TLS handshake.

This is useful when multiple HTTPS websites share the same IP address.

Example:

```text
Server IP:
203.0.113.10

Hosted Domains:
example.com
api.example.com
shop.example.net
```

The browser includes the intended hostname in the ClientHello.

The server can select the appropriate certificate and TLS configuration.

Traditional SNI is visible in the ClientHello. Encrypted ClientHello (ECH) is a newer mechanism designed to protect certain ClientHello information, including the real server name, when supported and correctly configured.

### 🖆 Application-Layer Protocol Negotiation (ALPN)

ALPN allows the client and server to negotiate the application protocol during the TLS handshake.

For example:

```text
Client Supports:
- h2
- http/1.1

Server Selects:
- h2
```

The connection then uses HTTP/2.

Common ALPN identifiers include:

| Identifier | Protocol |
| --- | --- |
| `http/1.1` | HTTP/1.1 |
| `h2` | HTTP/2 |
| `h3` | HTTP/3, negotiated through QUIC mechanisms |

# 🗁 Certificate Installation and Configuration

An HTTPS server requires an appropriate certificate, private key, and TLS configuration.

### 🖆 Certificate Files

A typical certificate deployment may contain:

```text
/etc/ssl/
    |
    +-- certs/
    |     |
    |     +-- example.com.crt
    |
    +-- private/
          |
          +-- example.com.key
```

The certificate file contains the public certificate information.

The private key file must remain protected.

### 🖆 Obtaining a Certificate

Certificates can be obtained from:

* Let's Encrypt.
* Commercial Certificate Authorities.
* Cloud platform certificate services.
* Internal organizational CAs.

Let's Encrypt provides free automated domain-validated certificates.

### 🖆 Certbot Installation on Ubuntu

Install Certbot:

```bash
sudo apt update
sudo apt install certbot
```

For Nginx integration:

```bash
sudo apt install python3-certbot-nginx
```

For Apache integration:

```bash
sudo apt install python3-certbot-apache
```

Obtain and configure a certificate for Nginx:

```bash
sudo certbot --nginx -d example.com -d www.example.com
```

For Apache:

```bash
sudo certbot --apache -d example.com -d www.example.com
```

These commands require the domain to resolve to the server and the appropriate validation method to be available.

### 🖆 Verify Certificate Renewal

Check renewal configuration:

```bash
sudo certbot renew --dry-run
```

This tests whether automated renewal can succeed.

Certificates have limited validity periods, so automated renewal is important.

# 🗁 HTTPS Configuration with Nginx

Nginx can terminate TLS connections and forward requests to an application server.

### 🖆 Basic HTTPS Configuration

Example:

```nginx
server {
    listen 443 ssl;
    server_name example.com www.example.com;

    ssl_certificate /etc/letsencrypt/live/example.com/fullchain.pem;

    ssl_certificate_key /etc/letsencrypt/live/example.com/privkey.pem;

    location / {
        proxy_pass http://127.0.0.1:3000;

        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

This configuration:

* Listens on HTTPS port `443`.
* Loads the certificate chain.
* Loads the private key.
* Proxies requests to an application running on port `3000`.
* Forwards useful request information to the backend.

### 🖆 Redirect HTTP to HTTPS

Add a separate server block:

```nginx
server {
    listen 80;
    server_name example.com www.example.com;

    return 301 https://$host$request_uri;
}
```

This redirects HTTP requests to HTTPS.

### 🖆 Test Nginx Configuration

Check the configuration:

```bash
sudo nginx -t
```

Reload Nginx:

```bash
sudo systemctl reload nginx
```

# 🗁 HTTPS Configuration with Apache

Apache supports HTTPS through its SSL module.

### 🖆 Enable SSL Module

```bash
sudo a2enmod ssl
```

Enable the site configuration:

```bash
sudo a2ensite default-ssl
```

Reload Apache:

```bash
sudo systemctl reload apache2
```

### 🖆 Basic Virtual Host Configuration

```apache
<VirtualHost *:443>
    ServerName example.com

    DocumentRoot /var/www/html

    SSLEngine on

    SSLCertificateFile /etc/letsencrypt/live/example.com/fullchain.pem

    SSLCertificateKeyFile /etc/letsencrypt/live/example.com/privkey.pem
</VirtualHost>
```

Validate the configuration:

```bash
sudo apachectl configtest
```

Reload Apache:

```bash
sudo systemctl reload apache2
```

# 🗁 HTTP to HTTPS Redirection

HTTP-to-HTTPS redirection automatically sends users from an unencrypted URL to its secure equivalent.

Example:

```text
http://example.com
       |
       v
301 Redirect
       |
       v
https://example.com
```

### 🖆 301 Redirect

A `301 Moved Permanently` response indicates that the requested resource has moved permanently to another URL.

Example:

```http
HTTP/1.1 301 Moved Permanently
Location: https://example.com/dashboard
```

Browsers and search engines can update their understanding of the resource location.

### 🖆 308 Permanent Redirect

`308 Permanent Redirect` also indicates a permanent redirect.

Unlike `301`, it preserves the original request method and body.

Example:

```http
HTTP/1.1 308 Permanent Redirect
Location: https://example.com/api
```

For HTTPS enforcement, `301` and `308` are both possible choices, depending on application requirements.

# 🗁 HTTP Strict Transport Security (HSTS)

HSTS is a security policy that instructs browsers to access a website using HTTPS for future requests.

It is defined by the `Strict-Transport-Security` response header.

### 🖆 HSTS Header

Example:

```http
Strict-Transport-Security: max-age=31536000
```

This tells the browser to remember the HTTPS-only policy for one year.

### 🖆 HSTS with includeSubDomains

```http
Strict-Transport-Security: max-age=31536000; includeSubDomains
```

This applies the policy to the domain and its subdomains.

Before enabling it, ensure that all relevant subdomains support HTTPS.

### 🖆 HSTS Preloading

HSTS preload lists allow supported browsers to enforce HTTPS before a site has been visited.

A typical preload-oriented header is:

```http
Strict-Transport-Security: max-age=63072000; includeSubDomains; preload
```

The `preload` token alone does not automatically add a domain to browser preload lists. A domain must satisfy the relevant requirements and be submitted through the applicable preload process.

### 🖆 HSTS Benefits

* Reduces the risk of HTTP downgrade attacks.
* Prevents browsers from using HTTP after learning the policy.
* Helps protect against certain SSL-stripping attacks.
* Enforces HTTPS for supported future visits.

HSTS should only be sent over HTTPS.

# 🗁 Mixed Content

Mixed content occurs when an HTTPS page loads resources through an insecure HTTP connection.

Example:

```html
<script src="http://example.com/app.js"></script>
```

The page is served over HTTPS, but the JavaScript file is requested over HTTP.

This can compromise the security of the page.

### 🖆 Types of Mixed Content

**Blockable mixed content**

Active resources such as scripts and stylesheets can execute or modify page behavior.

Modern browsers generally block insecure active content.

**Upgradable mixed content**

Some resource types may be automatically upgraded to HTTPS by browsers when applicable.

Behavior depends on the resource type and browser implementation.

### 🖆 Correct Implementation

Use HTTPS URLs for all resources:

```html
<script src="https://example.com/app.js"></script>

<link rel="stylesheet" href="https://example.com/styles.css">

<img src="https://example.com/images/logo.png" alt="Logo">
```

Where appropriate, use relative URLs or URLs that inherit the secure page's scheme.

# 🗁 HTTPS and Cookies

HTTPS protects cookies during transmission when they are sent over a TLS connection.

However, HTTPS alone does not configure cookie security attributes.

### 🖆 Secure Cookie

Example:

```http
Set-Cookie: session_id=abc123; Secure; HttpOnly; SameSite=Lax; Path=/
```

Important attributes:

| Attribute | Purpose |
| --- | --- |
| `Secure` | Restricts transmission to secure connections |
| `HttpOnly` | Prevents JavaScript access |
| `SameSite` | Controls cross-site cookie sending |
| `Path` | Controls URL path matching |

### 🖆 Why Secure Cookies Matter

Without the `Secure` attribute, a cookie may be sent over an unencrypted HTTP request if the other cookie-matching conditions allow it.

Use HTTPS throughout the application and mark authentication cookies as `Secure`.

# 🗁 HTTPS and Web APIs

HTTPS is important for APIs because API requests frequently contain sensitive information.

Examples include:

* Login credentials.
* Access tokens.
* User profile data.
* Payment information.
* Application secrets.

### 🖆 HTTPS API Request

```http
POST /api/login HTTP/1.1
Host: api.example.com
Content-Type: application/json

{
  "email": "user@example.com",
  "password": "secret"
}
```

When sent over HTTPS, the HTTP request is protected by TLS during transmission.

### 🖆 HTTPS API Response

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
  "message": "Login successful",
  "authenticated": true
}
```

The response is also protected by the TLS connection.

### 🖆 HTTPS in Fetch API

JavaScript can send requests to HTTPS endpoints using Fetch:

```javascript
async function getUsers() {
  const response = await fetch("https://api.example.com/users");

  if (!response.ok) {
    throw new Error(`HTTP error: ${response.status}`);
  }

  return response.json();
}
```

For production applications, sensitive API endpoints should require HTTPS.

# 🗁 HTTPS and CORS

CORS (Cross-Origin Resource Sharing) is a browser security mechanism that controls whether frontend JavaScript can access resources from another origin.

HTTPS does not eliminate the need for CORS configuration.

Example:

```text
Frontend:
https://app.example.com

API:
https://api.example.com
```

These are different origins because their hostnames differ.

The API may need to return:

```http
Access-Control-Allow-Origin: https://app.example.com
```

If cross-origin credentials are required, the server must explicitly allow the origin and credentials:

```http
Access-Control-Allow-Origin: https://app.example.com
Access-Control-Allow-Credentials: true
```

The frontend must also opt into credentialed requests:

```javascript
fetch("https://api.example.com/profile", {
  credentials: "include",
});
```

When credentials are included, the server cannot use `Access-Control-Allow-Origin: *`.

CORS controls browser access to cross-origin responses. It is not a substitute for authentication or authorization.

# 🗁 Testing and Debugging HTTPS

Several command-line tools help inspect HTTPS connections and certificates.

### 🖆 Test HTTPS with cURL

```bash
curl -I https://example.com
```

This displays response headers.

### 🖆 Display TLS Connection Details

```bash
curl -v https://example.com
```

Verbose output can show connection details, certificate verification, and TLS negotiation information.

### 🖆 Inspect a Certificate with OpenSSL

```bash
openssl s_client -connect example.com:443 -servername example.com
```

This establishes a TLS connection and displays certificate and handshake information.

The `-servername` option sends SNI for the requested hostname.

### 🖆 Display Certificate Information

```bash
openssl s_client -connect example.com:443 -servername example.com </dev/null 2>/dev/null | openssl x509 -noout -text
```

This displays certificate details such as:

* Subject.
* Issuer.
* Validity.
* Public key information.
* Subject Alternative Names.

### 🖆 Check Certificate Expiration

```bash
echo | openssl s_client -connect example.com:443 -servername example.com 2>/dev/null | openssl x509 -noout -dates
```

Example output:

```text
notBefore=...
notAfter=...
```

### 🖆 Verify a Certificate Chain

```bash
openssl s_client -connect example.com:443 -servername example.com -verify_return_error
```

The verification result depends on the available trust store and certificate configuration.

### 🖆 Test TLS Versions

Check TLS 1.2:

```bash
openssl s_client -connect example.com:443 -servername example.com -tls1_2
```

Check TLS 1.3:

```bash
openssl s_client -connect example.com:443 -servername example.com -tls1_3
```

These commands test whether the server accepts the specified TLS version, subject to the local OpenSSL build and server configuration.

# 🗁 Common HTTPS Errors

### 🖆 Certificate Expired

The certificate is outside its validity period.

**Possible solution:**

Renew the certificate and verify automatic renewal.

```bash
sudo certbot renew
```

### 🖆 Certificate Hostname Mismatch

The requested hostname does not match the certificate's valid hostnames.

**Possible solution:**

Issue or configure a certificate containing the correct hostname in its Subject Alternative Name extension.

### 🖆 Untrusted Certificate Authority

The browser cannot build a trusted certificate chain.

**Possible solution:**

Use a certificate issued by a trusted CA and configure the complete certificate chain correctly.

### 🖆 Missing Intermediate Certificate

The server does not provide the necessary intermediate certificates.

**Possible solution:**

Configure the server to provide the appropriate full certificate chain.

### 🖆 Mixed Content Error

An HTTPS page requests resources through HTTP.

**Possible solution:**

Update resource URLs to HTTPS and verify that all required resources support secure connections.

### 🖆 TLS Version Mismatch

The client and server do not share a compatible TLS version.

**Possible solution:**

Configure modern TLS versions and compatible cipher suites.

### 🖆 Private Key Mismatch

The configured private key does not correspond to the installed certificate.

**Possible solution:**

Install the correct private key and certificate pair.

# 🗁 HTTPS Security Best Practices

* Use HTTPS for all pages and API endpoints.
* Prefer TLS 1.3 and support TLS 1.2 when necessary.
* Disable SSL, TLS 1.0, and TLS 1.1.
* Use certificates from trusted Certificate Authorities.
* Protect private keys with appropriate filesystem permissions and access controls.
* Configure certificate renewal automatically.
* Redirect HTTP traffic to HTTPS.
* Use HSTS after confirming HTTPS is correctly deployed across the intended domain scope.
* Enable HSTS preloading only when its requirements and operational consequences are understood.
* Set authentication cookies with `Secure` and `HttpOnly`.
* Configure an appropriate `SameSite` policy.
* Avoid mixed content.
* Use secure cipher suites and modern cryptographic configurations.
* Monitor certificate expiration.
* Test TLS configuration regularly.
* Avoid exposing private keys through source control or public storage.
* Use correct hostname and certificate chain configuration.
* Ensure reverse proxies correctly handle TLS termination and forwarded protocol information.
* Do not treat HTTPS as a replacement for application authentication, authorization, or input validation.

# 🗁 Practical Exercises

### 🖆 Exercise: Visit an HTTPS Website

Open an HTTPS website and inspect its connection information using browser Developer Tools.

Identify:

* Certificate issuer.
* Certificate validity period.
* Hostname.
* TLS version.
* Connection security information.

### 🖆 Exercise: Inspect a Certificate

Run:

```bash
openssl s_client -connect example.com:443 -servername example.com
```

Inspect the certificate chain.

### 🖆 Exercise: Check Certificate Expiration

Run:

```bash
echo | openssl s_client -connect example.com:443 -servername example.com 2>/dev/null | openssl x509 -noout -dates
```

Identify the certificate's validity period.

### 🖆 Exercise: Configure HTTPS with Nginx

Install Nginx and Certbot on an Ubuntu server.

Configure a domain with a valid certificate.

Verify that the website is accessible over HTTPS.

### 🖆 Exercise: Redirect HTTP to HTTPS

Configure an HTTP server block that redirects all requests to HTTPS.

Test both URLs:

```text
http://example.com
https://example.com
```

### 🖆 Exercise: Enable HSTS

Configure:

```http
Strict-Transport-Security: max-age=31536000
```

Verify the response header using:

```bash
curl -I https://example.com
```

### 🖆 Exercise: Inspect TLS Versions

Use OpenSSL to test TLS 1.2 and TLS 1.3 support.

Compare the results.

### 🖆 Exercise: Configure Secure Cookies

Create a backend endpoint that sets a cookie with:

```http
Set-Cookie: session_id=abc123; Secure; HttpOnly; SameSite=Lax; Path=/
```

Verify the cookie attributes using browser Developer Tools.

# 🗁 Interview Questions

### 🖆 What is HTTPS?

HTTPS is HTTP communication protected by TLS, providing confidentiality, integrity, and server authentication.

### 🖆 What is TLS?

TLS is a cryptographic protocol that secures communication over a network.

### 🖆 What is the difference between HTTP and HTTPS?

HTTP does not provide transport encryption by itself. HTTPS uses TLS to protect HTTP communication.

### 🖆 What is the default HTTPS port?

The standard HTTPS port is `443`.

### 🖆 What is an SSL certificate?

It is a commonly used industry term for a digital certificate used in TLS deployments to associate a public key with an identity.

### 🖆 What is a Certificate Authority?

A Certificate Authority is an organization trusted to issue and digitally sign certificates.

### 🖆 What is a TLS handshake?

It is the process through which a client and server negotiate security parameters, authenticate the server, and establish shared cryptographic secrets.

### 🖆 What is the difference between TLS 1.2 and TLS 1.3?

TLS 1.3 simplifies the handshake, removes outdated cryptographic mechanisms, and generally completes a full handshake with fewer round trips.

### 🖆 What is a digital certificate?

A digital certificate is an electronic document that associates a public key with an identity and is digitally signed by an issuer.

### 🖆 What is a certificate chain?

A certificate chain links a server certificate through intermediate certificates to a trusted root CA.

### 🖆 What is the difference between symmetric and asymmetric encryption?

Symmetric encryption uses the same secret key for encryption and decryption. Asymmetric cryptography uses related public and private keys for operations such as signing and key establishment.

### 🖆 What is SNI?

Server Name Indication allows a client to specify the hostname it wants during the TLS handshake.

### 🖆 What is ALPN?

Application-Layer Protocol Negotiation allows the client and server to negotiate an application protocol such as HTTP/2.

### 🖆 What is HSTS?

HTTP Strict Transport Security is a policy that instructs browsers to use HTTPS for future requests to a website.

### 🖆 What is mixed content?

Mixed content occurs when an HTTPS page loads resources over an insecure HTTP connection.

### 🖆 What is TLS termination?

TLS termination is the process of decrypting incoming TLS traffic at a server, reverse proxy, or load balancer before forwarding requests to another component.

### 🖆 Can HTTPS prevent every cyberattack?

No. HTTPS protects data in transit and authenticates the server, but it does not automatically prevent application vulnerabilities, malicious scripts, compromised endpoints, or authorization errors.

### 🖆 Does HTTPS use TCP or UDP?

HTTP/1.1 and HTTP/2 commonly use TLS over TCP. HTTP/3 uses QUIC over UDP and integrates TLS 1.3 security mechanisms.

# 🗁 References

* [MDN — HTTPS](https://developer.mozilla.org/en-US/docs/Glossary/HTTPS)
* [MDN — Transport Layer Security](https://developer.mozilla.org/en-US/docs/Web/Security/Transport_Layer_Security)
* [MDN — TLS](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/Authentication#tls)
* [RFC 8446 — TLS 1.3](https://www.rfc-editor.org/rfc/rfc8446)
* [RFC 9110 — HTTP Semantics](https://www.rfc-editor.org/rfc/rfc9110)
* [RFC 9114 — HTTP/3](https://www.rfc-editor.org/rfc/rfc9114)
* [RFC 6797 — HTTP Strict Transport Security](https://www.rfc-editor.org/rfc/rfc6797)
* [Let's Encrypt Documentation](https://letsencrypt.org/docs/)
* [Certbot Documentation](https://eff-certbot.readthedocs.io/)
* [OpenSSL Documentation](https://docs.openssl.org/)
* [OWASP — Transport Layer Security Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Transport_Layer_Security_Cheat_Sheet.html)
