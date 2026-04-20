# Synapse-0 MVP Product Spec

*Companion to [Synapse-0_Project_Brief.md](./Synapse-0_Project_Brief.md)*
*Prepared: April 20, 2026*

## 1. Product Goal

Build a working local-embedding semantic retrieval product that proves three things:

- browser-side embedding is fast enough for real interaction
- vector retrieval can return useful results on a curated corpus
- a meaningful search experience can exist without sending raw query text to the backend

## 2. MVP Product Name

Working product name:

`Synapse-0 Explorer`

This can change later. It is only a product label for the first implementation.

## 3. Chosen Wedge

The MVP should focus on:

`developer tool and repository discovery`

Why this wedge:

- the corpus is relatively structured
- payloads are public and easy to index
- semantic discovery is genuinely useful
- users often search by abstract need, not exact keywords
- legal risk is lower than health, hiring, or dating

Examples of searchable items:

- GitHub repositories
- indie developer tools
- SDKs and open-source utilities
- AI tooling projects

## 4. User Problem

Users often know the shape of what they want but not the exact term:

- "an auth library that feels minimal and self-hostable"
- "a lightweight vector database for local prototyping"
- "a tool for scraping docs into markdown without heavy infra"

Keyword search handles this poorly. Synapse-0 should make these intent-driven searches easier while keeping raw wording on the device.

## 5. User Stories

- As a user, I can type a natural-language query and get results without sending the raw text to the server.
- As a user, I can see why a result matched at a basic level through metadata and similarity score.
- As a user, I can refine my search quickly with another local query.
- As a publisher, I can submit a tool or repository for inclusion in the index.
- As an operator, I can evaluate quality and latency without storing raw user intent.

## 6. Non-Goals

The MVP is not:

- a general web search engine
- a decentralized network
- a privacy-proof cryptographic system
- a hiring platform
- an ad marketplace
- a social or dating product

## 7. MVP Scope

### 7.1 Included

- web UI
- local text embedding in browser
- vector search against a curated corpus
- result cards with metadata
- basic filtering
- similarity-ranked top-k retrieval
- lightweight analytics without raw query retention
- publisher ingestion path for approved submissions

### 7.2 Excluded

- accounts as a hard requirement
- chat interface
- conversational memory
- reranking with a large server-side LLM
- personalized profiling
- mesh routing
- blockchain or token layer

## 8. User Experience

### 8.1 Core Flow

1. User opens the app.
2. User enters a search query.
3. Browser computes embedding locally.
4. Client sends vector to backend.
5. Backend retrieves top matches.
6. UI renders ranked results with titles, tags, summaries, and links.

### 8.2 Result Card Fields

Each result should show:

- title
- short description
- type
- tags
- source URL
- optional GitHub stars or quality signal
- optional similarity score bucket

### 8.3 Filters

The first version may include:

- category
- license type
- hosted vs self-hosted
- open source vs paid
- recency bucket

## 9. Technical Architecture

### 9.1 Frontend

- Next.js or React app
- browser-side model execution
- WebAssembly baseline
- WebGPU acceleration where supported

### 9.2 Backend

- Node.js API
- Qdrant for ANN search
- PostgreSQL for item metadata
- background ingestion job for content indexing

### 9.3 Retrieval Model

The first retrieval model should be:

- lightweight
- stable
- easy to run in browser
- dimensionally compatible with the indexed corpus

The first model choice should be treated as a product decision, not protocol law.

## 10. Corpus Strategy

The initial corpus should be curated rather than open submission.

Suggested starting corpus:

- 25,000 to 100,000 items
- public repositories and tool pages
- manually cleaned metadata
- deduplicated entries

Each item should have:

- unique ID
- canonical URL
- title
- summary
- tags
- source type
- precomputed embedding
- trust or quality metadata if available

## 11. Quality Strategy

The MVP should optimize for:

- relevant first-page results
- low query latency
- clean metadata
- trust in the corpus

The MVP should not optimize for:

- full recall of the public web
- adversarial SEO resistance at internet scale
- personalized ranking

## 12. Success Metrics

### 12.1 Product Metrics

- percent of sessions with at least one result click
- repeat user rate
- query refinement rate
- successful result save or open rate

### 12.2 Retrieval Metrics

- Recall@10
- NDCG@10
- MRR
- query-to-first-result latency

### 12.3 Privacy Metrics

- zero raw-query transmissions in normal search path
- zero raw-query logging on backend
- vector retention duration
- telemetry minimization compliance

## 13. MVP Release Criteria

The MVP is ready for a small pilot when:

- local embedding works reliably on target browsers
- median end-to-end latency is acceptable
- offline retrieval evaluation is stable
- the threat-model checklist is completed
- the corpus has enough quality to support real searches

## 14. Proposed Milestones

### Milestone 1: Prototype

- search box
- local embedding
- vector query endpoint
- simple results list

### Milestone 2: Usable Demo

- curated corpus
- metadata cleanup
- filters
- latency instrumentation

### Milestone 3: Pilot Build

- ingestion workflow
- operator review tools
- privacy hardening
- evaluation dashboard

## 15. Key Risks

- browser inference is slower than expected on lower-end hardware
- the chosen embedding model is weak on developer-tool intent
- corpus quality is noisy
- result cards are not understandable enough for trust
- privacy messaging outruns technical reality

## 16. Open Decisions

- exact model for client embedding
- whether to support anonymous sessions only or optional sign-in
- which browser targets define launch readiness
- how much result explanation to expose
- whether to add a limited "perspective shift" mode in MVP or postpone it

## 17. Recommended 90-Day Output

The first 90 days should produce:

- a live searchable prototype
- a seeded corpus
- an evaluation report
- a documented privacy posture
- a short pilot with real users

## 18. Bottom Line

The MVP should prove the architecture, not the whole future protocol.

If Synapse-0 can help users find developer tools and repositories better than keyword search while keeping raw text local, that is enough to validate the first step.
