# Security Policy

## Our Commitment
At Phantoma, security is not a feature; it is our foundational mandate. We operate under a "Zero-Trust" infrastructure philosophy.

> **"Phantoma puede cometer errores pero tus datos siempre estarán seguros."**
> *(Phantoma may make mistakes, but your data will always be safe.)*

## Security Strategy
* **Zero Persistence:** No sensitive data is ever written to non-volatile storage. 
* **Encryption Standards:** We exclusively use **AES-256-GCM** for data in transit and **ECDH** for secure key exchange.
* **Kernel Isolation:** Our deployment restricts OS-level introspection to prevent host-side data leaks.

## Reporting Vulnerabilities
We take security reports seriously. If you discover a vulnerability regarding our memory isolation or encryption protocols, please contact:
* **Lead Architect:** [diego@phantomaplatforms.com](mailto:diego@phantomaplatforms.com)

We strive to acknowledge all security inquiries within 12 hours.
