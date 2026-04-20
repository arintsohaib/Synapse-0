# Synapse-0 Evaluation Framework

*Companion to [Synapse-0_Project_Brief.md](./Synapse-0_Project_Brief.md)*
*Prepared: April 20, 2026*

## 1. Purpose

This document defines how Synapse-0 should be evaluated before and after the MVP launch.

The system must be measured on more than retrieval quality. It must also be evaluated for:

- latency
- privacy leakage
- corpus quality
- operational reliability
- user trust

## 2. Evaluation Principles

- Measure what matters to the actual product wedge.
- Separate offline retrieval quality from live user behavior.
- Treat privacy claims as testable claims.
- Record baselines before optimization.
- Prefer repeatable evaluations over ad hoc demos.

## 3. Evaluation Categories

Synapse-0 should be evaluated in five categories:

- retrieval relevance
- latency and performance
- privacy and leakage
- ingestion quality
- pilot user outcomes

## 4. Offline Retrieval Benchmark

### 4.1 Goal

Determine whether semantic retrieval on the chosen corpus returns useful results for real intent-style queries.

### 4.2 Dataset Construction

Build a benchmark set consisting of:

- 200 to 1,000 realistic user queries
- curated relevance judgments
- multiple query styles per intent
- hard negative examples

For the developer-tool wedge, the benchmark should include:

- vague natural-language queries
- exact tool-name queries
- comparison queries
- constrained queries

### 4.3 Relevance Labeling

Each query should have graded labels such as:

- 3 = ideal match
- 2 = good match
- 1 = somewhat relevant
- 0 = irrelevant

Judging should be performed by at least two raters for a subset of the benchmark to measure consistency.

### 4.4 Core Metrics

- Recall@5
- Recall@10
- NDCG@5
- NDCG@10
- MRR
- success rate of top-3 results

## 5. Latency Evaluation

### 5.1 Goals

Measure the real user wait time from input submission to rendered results.

### 5.2 Break Down the Pipeline

Measure:

- client model load time
- embedding latency
- request serialization time
- API round-trip time
- vector DB query time
- metadata fetch time
- render time

### 5.3 Target Metrics

Initial targets:

- warm embedding p50 <= 75 ms
- warm embedding p95 <= 200 ms
- backend retrieval p50 <= 75 ms
- end-to-end search p50 <= 300 ms
- end-to-end search p95 <= 800 ms

These can be refined after the first real measurements.

### 5.4 Test Environments

Measure on:

- modern desktop browser
- mid-range laptop
- mobile-class browser if supported
- low-end network conditions

## 6. Privacy and Leakage Evaluation

### 6.1 Goal

Quantify how much private information can be inferred despite keeping raw text on-device.

### 6.2 Questions to Answer

- Can the chosen embedding be inverted with meaningful fidelity?
- Can sensitive attributes be inferred from vectors?
- What metadata is still visible to the backend?
- Does repeated use create linkable behavior?

### 6.3 Required Tests

- embedding inversion benchmark
- nearest-neighbor leakage analysis
- sensitive-term recovery analysis
- logging audit
- retention audit
- telemetry capture review

### 6.4 Privacy Metrics

- exact reconstruction rate
- semantic reconstruction score
- PII recovery rate
- vector retention duration
- percent of requests with raw text leakage
- number of persistent identifiers emitted in the request path

## 7. Ingestion Quality Evaluation

### 7.1 Goal

Ensure the corpus itself is not degrading retrieval quality.

### 7.2 Metrics

- duplicate rate
- invalid URL rate
- missing metadata rate
- broken link rate
- tag consistency
- publisher rejection rate
- spam detection precision and recall once moderation data exists

### 7.3 Manual Review

Each ingestion batch should be sampled for:

- summary quality
- tag quality
- canonicalization correctness
- content safety or abuse risk

## 8. Live Pilot Evaluation

### 8.1 Goal

Validate whether real users find Synapse-0 useful, understandable, and trustworthy.

### 8.2 Pilot Questions

- Did users find something they would not have found easily through keyword search?
- Did users trust the system's privacy posture?
- Were result cards understandable enough to act on?
- Did the corpus feel deep enough to be worth revisiting?

### 8.3 Pilot Metrics

- query sessions per user
- click-through rate
- refined-query rate
- return rate
- saved-result rate
- qualitative trust score
- perceived relevance score

## 9. Comparative Baselines

Synapse-0 should not be evaluated in isolation.

Compare against:

- basic keyword search over the same corpus
- hybrid lexical + vector retrieval over the same corpus
- manual browse workflows where relevant

The goal is to measure whether local-embedding retrieval adds enough value to justify the added complexity.

## 10. Experiment Matrix

The team should maintain an experiment matrix across:

- embedding model versions
- chunking or summary strategy
- corpus versions
- metadata weighting rules
- filter settings
- result-card design

Each experiment should record:

- hypothesis
- changed variable
- benchmark result
- latency impact
- privacy impact
- decision

## 11. Release Gates

The product should not advance from internal prototype to pilot unless:

- offline retrieval beats baseline keyword search on chosen metrics
- p95 latency is acceptable for target hardware
- raw query text never leaves the client in the normal path
- logging and retention are audited
- ingestion quality is stable enough to support repeated usage

## 12. Dashboard Requirements

At minimum, the team should have a dashboard or report containing:

- corpus size
- query counts
- latency percentiles
- Recall@10 and NDCG@10 trends
- ingestion health
- logging compliance checks
- top failure queries by category

## 13. Failure Analysis Loop

Every weak query should be classified into one or more buckets:

- corpus missing relevant item
- item exists but summary is weak
- embedding miss
- filtering issue
- deduplication issue
- UI presentation issue

This prevents the team from blaming the model for every failure.

## 14. Recommended Cadence

- per-commit microbenchmarks for latency-sensitive changes
- weekly retrieval benchmark runs
- biweekly ingestion audits
- monthly privacy review
- pilot review after each significant user cohort

## 15. Bottom Line

Synapse-0 should succeed only if it passes three tests at once:

- useful retrieval
- acceptable latency
- defensible privacy posture

If one of those fails, the product story weakens quickly. The evaluation framework must therefore be part of the product, not an afterthought.
