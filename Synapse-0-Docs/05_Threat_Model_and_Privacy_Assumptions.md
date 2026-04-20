# Synapse-0 Threat Model and Privacy Assumptions

*Companion to [Synapse-0_Project_Brief.md](./Synapse-0_Project_Brief.md)*
*Prepared: April 20, 2026*

## 1. Purpose

This document defines what Synapse-0 does and does not protect in its first buildable form.

The goal is to avoid vague privacy claims. Synapse-0 should only promise protections that are tied to a specific architecture, threat model, and measurement process.

## 2. System Boundary

For the MVP, Synapse-0 is assumed to be:

- a web client that runs an embedding model locally
- a centralized retrieval backend using a vector database
- a metadata store containing public or publisher-submitted payloads
- a curated corpus, not the entire public web

This document does not assume:

- a decentralized mesh
- onion routing
- differential privacy by default
- cryptographic private retrieval
- zero-knowledge proofs

## 3. Core Privacy Objective

The primary privacy objective is:

`raw user text should not leave the client device during retrieval`

This is the baseline Synapse-0 protection. Everything beyond that must be stated more carefully.

## 4. Assets to Protect

The following assets matter most:

- raw query text
- repeated query history
- user identity and IP address
- click behavior and follow-up behavior
- latent intent related to health, politics, finance, religion, sexuality, work, or relationships
- account-linked behavioral profiles

## 5. Adversaries

### 5.1 Passive Backend Operator

The backend receives the transmitted vector and serves nearest-neighbor results. It does not see raw text, but it may attempt to infer the original query from:

- the vector itself
- repeated vectors or nearby vectors
- timestamps
- IP addresses
- returned results

### 5.2 Active Network Observer

An observer on the network path may see:

- source IP address
- destination
- message timing
- request size
- response size

TLS mitigates plaintext interception, but it does not hide metadata.

### 5.3 Malicious Publisher

A malicious publisher may try to:

- flood the corpus with adversarial content
- place payloads near sensitive semantic neighborhoods
- infer query themes from which payloads are returned
- poison ranking quality

### 5.4 Malicious or Curious Client Environment

The user device itself may be compromised by:

- browser extensions
- malware
- operating-system level logging
- screen capture or keylogging

Synapse-0 cannot defend against a compromised endpoint.

### 5.5 Legal or Regulatory Adversary

A subpoena, court order, regulator, or civil discovery process may compel production of:

- access logs
- stored vectors
- account associations
- publisher records
- monitoring data

This is why retention and observability design matter.

## 6. MVP Assumptions

The following assumptions are required for the MVP privacy story:

- the embedding model runs fully on-device
- raw query text is not transmitted
- raw query text is not logged server-side
- the backend stores no long-term user profile by default
- the corpus payloads are public or publisher-approved for indexing
- transport uses HTTPS/TLS
- the backend is centralized and trusted to execute the documented logging policy

## 7. What Synapse-0 Protects in MVP

If the MVP is implemented correctly, it can credibly claim:

- the retrieval backend does not receive the raw text query
- the core retrieval flow can work without persistent user accounts
- the backend can be designed to avoid retaining raw search history
- query processing can be reduced to vector matching plus payload lookup

This is already meaningful. It is a narrower and stronger claim than "complete privacy."

## 8. What Synapse-0 Does Not Yet Protect

The MVP does not automatically protect against:

- embedding inversion
- semantic inference from stored vectors
- IP-based correlation
- timing attacks
- repeated-query linkage
- clickstream profiling if clicks are logged
- browser or endpoint compromise
- corpus poisoning
- legal compulsion over logs that still exist

Any public-facing material should make these limits explicit.

## 9. Embedding Privacy Assumption

The biggest open assumption is whether the transmitted embedding is sufficiently hard to invert or exploit.

Current evidence suggests caution:

- embeddings are not equivalent to raw text
- embeddings are also not guaranteed to be safe disclosures
- modern inversion work shows that dense text embeddings can leak far more than older intuition suggested

Therefore, Synapse-0 should treat vectors as sensitive derived data, not as harmless anonymized data.

## 10. Logging Policy Requirements

The MVP should adopt the following defaults:

- no raw query logging
- no vector logging unless explicitly enabled for short-lived debugging
- short retention for operational logs
- IP truncation or elimination where possible
- separate telemetry from retrieval data
- opt-in analytics rather than silent collection

If vector logging is temporarily enabled during development, it should be:

- isolated
- access-controlled
- time-limited
- deleted automatically

## 11. Compliance Position

Synapse-0 should assume that vectors may still count as personal data when they are linkable to a person, device, session, or behavior pattern.

Practical consequence:

- treat vectors as regulated data
- design for deletion and minimization
- do not describe vectors as anonymous by default
- avoid "privacy by physics" language unless backed by measured evidence and legal review

## 12. Sensitive Use Cases to Avoid at Launch

The following categories should be excluded from the initial product:

- hiring and candidate screening
- medical or mental-health search
- dating or personality matching
- political persuasion or civic targeting
- law-enforcement or surveillance use cases

These carry high fairness, privacy, and regulatory risk.

## 13. Abuse and Poisoning Risks

Even with no raw text visibility, the corpus can still be attacked.

Examples:

- semantic spam around profitable intent clusters
- publishers shaping metadata to win proximity matches
- adversarial embeddings for content items
- mass low-quality content injection

Required mitigations:

- curated ingestion in Phase 1
- publisher approval or API-key gating
- deduplication
- rate limits
- manual review and takedown process

## 14. Security Goals by Phase

### Phase 1

- local embedding only
- centralized backend
- minimal retention
- no raw-query transmission

### Phase 2

- leakage benchmarking
- logging hardening
- stronger publisher abuse controls
- optional privacy-preserving transformations if quality remains acceptable

### Phase 3

- IP protection research
- decentralized routing experiments
- poisoning resistance
- replication and trust models

## 15. Privacy Claims Synapse-0 Can Make Today

Safe language:

- "The raw query text can stay on the user's device."
- "The retrieval backend can operate on vector representations rather than plaintext queries."
- "The system is designed to minimize server visibility into raw intent."

Unsafe language:

- "The server learns nothing about the user."
- "Vectors are anonymous."
- "Embeddings are impossible to reverse."
- "Synapse-0 guarantees zero knowledge."

## 16. Launch Gate Checklist

Before any public beta, the team should be able to answer:

- Is raw text ever transmitted anywhere in the retrieval path?
- Are vectors logged? If yes, why, where, and for how long?
- Can a user use the system without creating an account?
- What metadata is collected even when raw text stays local?
- What is the retention schedule for operational telemetry?
- What sensitive categories are excluded from launch?
- What evidence exists on inversion resistance for the chosen model?

## 17. Open Research Questions

- How much semantic information can a passive backend recover from logged vectors?
- What level of noise or transformation meaningfully reduces leakage while preserving retrieval quality?
- Can query-private retrieval be achieved with acceptable latency for real users?
- How should decentralized retrieval be designed without recreating a surveillance surface through network metadata?

## 18. Bottom Line

Synapse-0 has a real privacy advantage if it keeps raw intent on-device. That is already valuable.

But the honest MVP position is:

- raw text can stay local
- vectors remain sensitive
- metadata still matters
- privacy must be measured, not assumed

That framing is strong enough to build on and rigorous enough to defend.
