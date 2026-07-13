# SSL/TLS Basics — Quick Revision Notes

## 1. SSL vs TLS

* **SSL (Secure Sockets Layer)** is the older, deprecated protocol.
* **TLS (Transport Layer Security)** is its modern and secure replacement.
* People still commonly say **“SSL certificate,”** but modern systems actually use TLS.
* HTTPS means normal HTTP traffic is protected using TLS.

## 2. Symmetric Encryption

* Uses the **same secret key** to encrypt and decrypt data.
* It is fast, so TLS uses it to protect the actual data exchanged after the connection is established.
* Main challenge: both sides must securely obtain the same secret key.

```text
Plaintext + Secret Key -> Encrypted Data
Encrypted Data + Same Secret Key -> Plaintext
```

## 3. Asymmetric Encryption

* Uses a **key pair**:

  * **Public key:** can be shared openly.
  * **Private key:** must remain secret with its owner.
* Data encrypted with the public key can only be decrypted with the corresponding private key.
* A private key can also create a digital signature that others verify using the public key.
* TLS uses asymmetric cryptography during authentication and key exchange.
* SSH key-based login also uses asymmetric cryptography instead of relying only on passwords.

> TLS combines both methods: asymmetric cryptography securely establishes the connection, and symmetric encryption efficiently protects the data.

## 4. SSH Keys vs TLS Keys

### `ssh-keygen`

Used mainly to generate key pairs for **SSH authentication**:

```bash
ssh-keygen -t ed25519
```

Typical files:

```text
id_ed25519       # Private SSH key
id_ed25519.pub   # Public SSH key
```

### `openssl`

Used for TLS-related operations such as:

* Generating private keys
* Creating Certificate Signing Requests (CSRs)
* Creating self-signed certificates
* Inspecting and verifying certificates

## 5. Digital Certificate

A certificate proves that a public key belongs to a particular server, domain, organization, or client.

It normally contains:

* Subject/domain name
* Public key
* Issuer (the CA)
* Valid-from and expiry dates
* Serial number
* CA's digital signature
* Subject Alternative Names (SANs), such as `app01.com`

Common filename conventions:

```text
Certificate:  app01.crt, app01.pem
Private key:  app01.key, app01-key.pem
CSR:          app01.csr
```

> `.pem` describes a text encoding/container and may hold a certificate, private key, public key, or certificate chain. Check its contents instead of relying only on its extension.

## 6. Certificate Authority (CA)

A **Certificate Authority** is a trusted organization that verifies certificate requests and digitally signs certificates. Examples include DigiCert, GlobalSign, and Let's Encrypt.

CAs have their own public/private key pairs:

* The CA signs a certificate using its **private key**.
* Browsers verify the signature using the CA's **public key**.
* Trusted root CA certificates are already stored in browser or operating-system trust stores.

### How a browser detects an untrusted or fake certificate

The browser checks:

* Whether the certificate chains back to a trusted root CA
* Whether the CA's digital signature is valid
* Whether the requested hostname matches the certificate's SAN
* Whether the certificate is currently valid and not expired
* Whether the certificate has been revoked, when revocation information is available

If these checks fail, the browser displays a security warning.

## 7. Certificate Signing Request (CSR)

A CSR is a request sent to a CA asking it to issue a signed certificate.

It contains:

* The public key
* Requested identity/domain information
* A signature created using the corresponding private key

It **does not contain the private key**.

Example:

```bash
openssl req -new -key app01.key -out app01.csr
```

CA-issued certificate flow:

```text
Generate private key
        ↓
Generate CSR using the private key
        ↓
Send only the CSR to the CA
        ↓
CA verifies the request and signs the certificate
        ↓
Install the certificate and existing private key on the server
```

> Never send the private key to the CA or expose it publicly.

## 8. Self-Signed Certificate

* A self-signed certificate is signed using its own private key rather than by a trusted CA.
* It still provides encryption, but browsers cannot automatically trust its identity.
* It is useful for development, testing, labs, and some private internal systems.
* Public-facing production websites should normally use a certificate signed by a trusted CA.

Example:

```bash
openssl req -x509 -newkey rsa:2048 -nodes \
  -keyout app01.key -out app01.crt -days 365
```

## 9. Client Certificates and mTLS

* In normal TLS, the **server** presents a certificate and the client verifies it.
* With **mutual TLS (mTLS)**, both the server and client present certificates.
* The server can therefore authenticate the client without relying only on a username and password.
* mTLS is commonly used for secure service-to-service communication.

## 10. Apache TLS Configuration

Configure Apache to use the certificate and its matching private key in `/etc/httpd/conf.d/ssl.conf`:

```apache
SSLCertificateFile /etc/httpd/certs/app01.crt
SSLCertificateKeyFile /etc/httpd/certs/app01.key
```

Check the configuration and restart Apache:

```bash
sudo apachectl configtest
sudo systemctl restart httpd
```

Inspect the certificate served by Apache:

```bash
echo | openssl s_client -showcerts \
  -servername app01.com -connect app01:443 2>/dev/null \
  | openssl x509 -inform pem -noout -subject -issuer -dates
```

## 11. PKI (Public Key Infrastructure)

**PKI** is the complete system used to create, issue, distribute, trust, verify, renew, and revoke digital certificates.

It includes:

* Public and private keys
* Digital certificates
* CSRs
* Certificate Authorities
* Root and intermediate certificate chains
* Trust stores
* Certificate revocation mechanisms
* Policies and identity-verification processes

## Quick Summary

| Concept                 | Purpose                                                              |
| ----------------------- | -------------------------------------------------------------------- |
| Symmetric encryption    | Fast encryption using one shared key                                 |
| Asymmetric encryption   | Authentication, signatures, and secure key exchange using a key pair |
| Private key             | Kept secret; used to prove identity or create signatures             |
| Public key              | Shared openly; used for encryption or signature verification         |
| Certificate             | Binds an identity/domain to a public key                             |
| CSR                     | Request sent to a CA for a signed certificate                        |
| CA                      | Trusted authority that verifies and signs certificates               |
| Self-signed certificate | Encrypts traffic but is not automatically trusted                    |
| Client certificate      | Identifies a client; commonly used with mTLS                         |
| PKI                     | Entire framework that manages keys, certificates, and trust          |

## One-Line Flow to Remember

```text
Private key -> CSR -> CA signs -> Certificate issued -> Install certificate + private key -> Configure server -> Browser verifies -> Secure TLS connection
```
