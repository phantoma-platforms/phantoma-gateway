# Phantoma Threat Model (STRIDE Analysis)

We operate under the assumption that the host environment (VPS) and the AI models themselves are potentially untrusted.

| Threat Category | Phantoma Mitigation Strategy |
| :--- | :--- |
| **Information Disclosure** | Data exists only in RAM (`tmpfs`). Secure memory wiping (shredding) post-inference. |
| **Data Exfiltration** | **Shadow Gateway:** Kernel-level egress blockade prevents models from sending data to external IPs. |
| **Tampering** | All binaries are Multi-Sig signed. Verifiable via Sigstore. |
| **Privilege Escalation** | Phantoma runs in a rootless container environment with limited syscall access via seccomp profiles. |

### Known Residual Risks (Out of Scope)
- **Compromised Host Kernel:** If the underlying VPS kernel is compromised, Phantoma's eBPF guards could be bypassed. We recommend running on hardened kernels (e.g., HardenedLinux).
- **Physical Access:** If an attacker has physical access to the server RAM while the process is active, cold-boot attacks are theoretically possible.
- **Side-Channel Attacks:** We do not currently mitigate against advanced power-analysis or timing attacks on the CPU during inference.

---
*Phantoma may make mistakes, but your data will always be safe.*
