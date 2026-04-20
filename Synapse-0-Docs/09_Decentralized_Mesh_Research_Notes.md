# Synapse-0 Decentralized Mesh Research Notes

*Companion to [Synapse-0_Project_Brief.md](./Synapse-0_Project_Brief.md)*
*Prepared: April 20, 2026*

## 1. Purpose

This document captures the long-range research direction for the Synapse-0 decentralized mesh.

It is intentionally exploratory. The decentralized mesh is not required for MVP success, but it is central to the long-term protocol vision.

## 2. High-Level Goal

Replace the centralized retrieval backend with a distributed network of nodes that can:

- host portions of the index
- respond to vector queries
- synchronize updates
- tolerate churn
- reduce single-operator control
- eventually obscure requester identity more effectively than a central service can

## 3. Why This Is Hard

ANN systems such as HNSW work extremely well in centralized deployments. They become much harder when the system must tolerate:

- node churn
- inconsistent replicas
- partial outages
- malicious peers
- poisoned content
- routing privacy constraints
- economic incentives

This is a distributed systems and adversarial ML problem, not just an infrastructure migration.

## 4. Recommended Framing

The decentralized mesh should be treated as:

`a research track toward protocolization`

not:

`a prerequisite for product-market fit`

## 5. Phased Path

### Phase 0: Centralized Index

- single operator
- centralized vector DB
- curated ingestion
- minimal privacy surface

### Phase 1: Federated Read Replicas

- multiple trusted nodes
- replicated index shards
- centralized control plane
- query distribution for reliability experiments

### Phase 2: Semi-Open Federation

- approved third-party nodes
- versioned index snapshots
- signed content packages
- limited trust domains

### Phase 3: Open Mesh

- dynamic node membership
- distributed discovery
- decentralized replication
- stronger abuse resistance
- privacy-preserving query routing

## 6. Research Questions

### 6.1 Index Topology

- Should the mesh replicate full indices or shards?
- How should shard boundaries be chosen?
- Can semantic neighborhoods be partitioned without damaging recall?

### 6.2 Query Routing

- Which nodes should receive a given query?
- How do we avoid spraying sensitive vectors widely across the network?
- Can routing preserve both latency and privacy?

### 6.3 Churn Handling

- How much recall is lost when nodes disappear?
- How often must graph links be repaired?
- Which repair strategy works best under high churn?

### 6.4 Adversarial Behavior

- How do we detect poisoned nodes?
- How do we bound sybil influence?
- How do we remove bad actors without centralizing trust too aggressively?

## 7. Candidate Building Blocks

Potentially useful building blocks include:

- libp2p for transport
- Kademlia-style discovery
- gossip-based state dissemination
- signed index snapshots
- per-shard replication groups
- reputation or stake-like admission controls

These are ingredients, not a final design.

## 8. Candidate Architectural Patterns

### 8.1 Replicated Shard Network

Each node hosts one or more shards plus their local ANN structures.

Pros:

- easier scaling
- bounded storage per node

Cons:

- routing becomes hard
- misses increase if shards are badly assigned

### 8.2 Full-Replica Trusted Federation

Each node holds the full index or a large subset.

Pros:

- simpler routing
- easier quality control

Cons:

- expensive
- weak openness

### 8.3 Hybrid Directory + Local ANN

A directory layer identifies candidate shards or nodes, then local ANN executes retrieval.

Pros:

- more practical as a transition architecture

Cons:

- directory layer may become a centralization point

## 9. Privacy Constraints

The decentralized mesh must not recreate the same surveillance surface it is trying to remove.

Important constraints:

- queries should not be broadcast widely
- no raw text should leave the client
- vector exposure should be minimized to the smallest plausible node set
- routing metadata should be considered sensitive

Even if vectors remain hidden from some nodes, network-level metadata can still deanonymize users.

## 10. Trust Model Options

### 10.1 Trusted Federation

Only approved nodes join.

Pros:

- operationally easier
- lower abuse risk

Cons:

- weaker decentralization

### 10.2 Open Participation with Strong Admission Rules

Anyone can attempt to join, but must satisfy network requirements.

Pros:

- more protocol-native

Cons:

- much harder to defend against sybils and poisoning

### 10.3 Layered Trust

Core nodes are highly trusted, edge nodes are less trusted, and query routing uses both.

This may be the most realistic intermediate design.

## 11. Index Update Strategy

The mesh needs a coherent story for:

- item creation
- item deletion
- metadata updates
- embedding model upgrades
- reindex events

A plausible early approach:

- publish signed index snapshots
- distribute deltas through gossip
- rebuild local ANN structures from trusted packages

This is more practical early on than fully online peer-written HNSW mutation.

## 12. Abuse and Poisoning Risks

Risks include:

- malicious nodes lying about recall
- low-quality nodes serving stale indices
- poisoned shards targeting sensitive queries
- sybil clusters biasing routing
- spam publishers coordinating through multiple nodes

Mitigations to explore:

- signed content bundles
- replica voting or consistency checks
- challenge queries
- operator reputation
- random audits

## 13. Measurement Plan

The decentralized mesh should be evaluated on:

- Recall@10 under churn
- p50, p95, and p99 query latency
- storage overhead
- repair bandwidth
- stale-index rate
- poisoning success rate
- node-join and node-recovery time

## 14. Simulation Before Deployment

Before any open mesh experiment, the team should build:

- a churn simulator
- a shard-placement simulator
- a query-routing simulator
- an adversarial node test harness

This avoids turning users into debugging infrastructure.

## 15. Practical Near-Term Experiments

Good first experiments:

- multi-region read replicas with synthetic failover
- replaying benchmark queries against multiple synchronized nodes
- testing simple shard routing against full-replica baseline
- measuring recall loss under node dropout

## 16. What Not to Do Too Early

- do not launch an open node network before abuse controls exist
- do not add token incentives before the network behavior is understood
- do not promise censorship resistance before query privacy and poisoning resistance are designed
- do not mutate a distributed HNSW graph in production without recovery tooling

## 17. Success Criteria for the Research Track

This track is succeeding if the team can eventually show:

- useful retrieval under churn
- bounded degradation under failures
- acceptable privacy exposure at the network layer
- practical update and deletion mechanics
- a trust model that does not collapse back to hidden centralization

## 18. Bottom Line

The decentralized mesh is one of the most ambitious parts of Synapse-0. It is worth pursuing, but only as a staged research effort built on top of a successful centralized product.

The right sequence is:

- prove retrieval value first
- measure privacy honestly
- then explore decentralization from a position of technical clarity
