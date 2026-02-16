# Security Policy & Threat Model

At Phantoma, security is not a feature—it is the core of our architecture. This document outlines our vulnerability disclosure process and our internal security protocols.

## 1. Reporting a Vulnerability

If you discover a security vulnerability within Phantoma, please do not open a public issue. Instead, help us protect the community by reporting it privately:

* **Email:** [diego@phantomaplatforms.com](mailto:diego@phantomaplatforms.com)
* **Response Time:** We aim to acknowledge all security reports within 24 hours.
* **PGP Key:** [Link to your PGP key or mention it is available upon request]

## 2. Our Threat Model

Phantoma is designed to operate in high-adversity environments. Our security controls focus on three main vectors:

### A. Supply Chain Attacks (Release Integrity)
To prevent unauthorized code injection, every Phantoma release is:
* **Multi-Sig Signed:** No single engineer can push a release. A quorum of keys is required to sign the binaries.
* **Verifiable Builds:** We use **Sigstore/Cosign** to allow users to verify the provenance of every container image and binary.
* **SBOM Provided:** Every release includes a Software Bill of Materials (CycloneDX/SPDX) for full dependency transparency.

### B. Data Exfiltration (Egress Protection)
We assume the AI model itself might be "chatty" or compromised. 
* **The Shadow Gateway** enforces a default-deny egress policy at the kernel level. 
* Any attempt to reach an unauthorized IP is logged and blocked instantly.

### C. Persistent Forensic Risk
To counter "Harvest Now, Decrypt Later" or physical server seizure:
* **Atomic Ephemerality:** We use encrypted RAM-disks (`tmpfs`) for all inference payloads. 
* **Secure Deletion:** We implement memory-wiping protocols to ensure that data does not survive a process termination or power cycle.

## 3. Supported Versions

We currently only provide security updates for the latest Alpha/Beta release.

| Version | Supported |
| ------- | --------- |
| 0.1.x (Alpha) | ✅ Yes |
| < 0.1.0 | ❌ No |

## 4. Responsible Disclosure Policy

We believe in the "Security Researcher's Rights." If you act in good faith, we will not pursue legal action against you and will work with you to understand and resolve the issue.

## 5. Deployment Security (User Responsibility)

Phantoma hardens the AI layer, but the underlying VPS is the user's responsibility. We strongly recommend:
* Running Phantoma as a non-privileged user.
* Disabling SSH password authentication.
* Keeping the Host OS kernel updated to support the latest eBPF security features.

---
*Phantoma may make mistakes, but your data will always be safe.*
