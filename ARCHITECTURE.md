
# Phantoma Architecture: Security-First Orchestration

This document outlines the high-level architecture of Phantoma. Our design principle is **Zero-Knowledge Infrastructure**: the Phantoma Control Plane manages the node's lifecycle without ever having access to the data being processed.

## 1. High-Level Topology

The system is split into two distinct environments to ensure data isolation:

### A. The Control Plane (Phantoma Cloud)
* **Function:** Licensing, update distribution, and node telemetry (health checks).
* **Security:** Cannot initiate connections to nodes. Only responds to outbound heartbeats.
* **Data Access:** Zero. No customer keys or inference data are stored here.

### B. The Data Plane (Your Private VPS)
* **Function:** Houses the Phantoma Gateway, the LLM Engine, and the Local Vault.
* **Environment:** Hardened Linux environment (Docker/K8s or Bare Metal).
* **Isolation:** All inference happens within this perimeter.

## 2. The Shadow Gateway (Egress Control)

The "Shadow Gateway" is our proprietary egress controller. It acts as a one-way cryptographic valve.

* **Inbound:** Accepts encrypted requests from authorized clients.
* **Outbound Blockade:** Using kernel-level filtering (eBPF), the gateway intercepts any attempt by the AI model to communicate with external IPs (the "phone home" problem).
* **Whitelisting:** Only explicitly defined model-download endpoints are allowed during maintenance windows.

## 3. Atomic Ephemerality (Memory Lifecycle)

To mitigate the risk of forensic data recovery, Phantoma implements **Atomic Ephemerality**:

1. **Request Ingestion:** Data is loaded directly into an isolated RAM buffer (tmpfs).
2. **Processing:** Inference is executed. Logs are piped to a volatile stream, never hitting the SSD/HDD.
3. **Hardware Wipe:** Upon completion of the task (or a timeout), a `shred`-equivalent operation is performed on the memory buffer, and the specific process space is terminated.

## 4. Key Management & Encryption

* **Transport:** All data in transit is secured via AES-256-GCM.
* **At Rest:** Phantoma is designed for **Zero-Persistence**. However, if a user opts for encrypted storage, keys are generated on the client-side and never shared with the Control Plane.
* **Multi-Sig Release:** Every binary running on your architecture is signed by a multi-signature quorum of our lead engineers to prevent supply chain injection.

## 5. Security Trade-offs

* **Performance:** There is a minor latency overhead (<10ms) due to the egress filtering and RAM-shredding operations. We believe this is a necessary trade-off for high-stakes environments.
* **Complexity:** Setting up a sovereign node requires basic knowledge of VPS management, as we refuse to offer a "managed cloud" version to protect your privacy.

---
*Phantoma may make mistakes, but your data will always be safe.*
