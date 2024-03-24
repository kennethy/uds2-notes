## Chapter 3. Secure Links

Data is transmitted out in-the-open. Intermediaries with malicious intent may tamper with the data. Transport Layer Security (TLS) procol provides safety protection on data transmission.

### 3.1 Encryption

- Encryption obfucates the data being transmitted and ensures only the sender and the receiver can interpret the data.
- When a TLS connection is established, the sender and the receiver negotiates a shared encryption secret using asymmetric encryption. Symmetric encryption via the shared secret is used afterward.
  - Asymmetric encryption: when different keys are used for encryption and decryption, whereas symmetric encryption uses the same key for both encryption and decryption.
  - Shared secret is periodically re-negotiated for additional safety.

### 3.2 Authentication
- The client still need to authenticate the server to verify it's who it claims to be.
- TLS implements authentication using digital signatures based on asymmetric crytography.
  - The server generates a pair of public and private keys. The public is shared to the client, and the client uses the server's public key to verify the digital signature was actually signed with the private key.
- The client validates the shared public key using certificates.
  - A certificate includes information such as the owning entity, expiration date, public key, and a digital signature of the third-party entity that issued the certificate.
- Certificate Authority (CA) issues certificates. It is also represented with a certificated.
- A chain of certificates ends with a certificated by the root CA.
- For a TLS certificate to be trusted by the client, there should be at least one CA that is trusted by the client.
- Authentication flow:
  - Sender sends the full certifcate chain (starting with the server's certificate all the way up to the root CA) to the client.
  - The client scans the chain and finds the CA that it trusts.
  - In reverse order, the client verifies the certificates starting with the trusted CA that was found.
- Certificate could expire so auto-rotation is desired to prevent failures due to certificate expiration.

### 3.3 Integrity
- It's still possible for intermediaries to modify the transmitted data. TLS provides data integrtiy guarantees via message digest.
- Message Authentication Code (HMAC) is produced and included as part of the transmitted data. When a message is received, the digest is computed again and will be checked against the one that is already included.
- HMAC allows receivers to detect data corruption as well.

### 3.4 Handshake
- When a new TLS connection is established, the following interactions will occur in no particular order:
  - Both parties agree on a cipher suite to use.
  - Both parties use the key exchange algorithm to create a shared secret.
  - The client verifies the certificates provided by the server.

