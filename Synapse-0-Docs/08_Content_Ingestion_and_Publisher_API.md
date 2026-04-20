# Synapse-0 Content Ingestion and Publisher API

*Companion to [Synapse-0_Project_Brief.md](./Synapse-0_Project_Brief.md)*
*Prepared: April 20, 2026*

## 1. Purpose

This document describes how content enters Synapse-0 and how publishers should eventually submit new items for indexing.

For the MVP, ingestion quality is a core product concern. A weak corpus will make retrieval look worse than it is.

## 2. Design Principles

- keep the corpus clean and curated in Phase 1
- separate publisher submission from automatic publication
- normalize metadata before indexing
- deduplicate aggressively
- treat ingestion as a trust and quality pipeline, not just a data pipe

## 3. Content Types for MVP

The first supported content types should be:

- GitHub repositories
- developer tools
- product landing pages
- documentation sites

These content types are easier to normalize than arbitrary web pages.

## 4. Canonical Content Record

Each indexed item should have a canonical record containing:

- `id`
- `canonical_url`
- `source_type`
- `title`
- `summary`
- `tags`
- `license`
- `pricing_model`
- `is_open_source`
- `created_at`
- `updated_at`
- `publisher_id` if known
- `trust_level`
- `embedding`

Optional fields:

- GitHub stars
- last commit date
- language
- deployment model
- screenshots or logo

## 5. Ingestion Sources

### 5.1 Seed Corpus

The seed corpus can be assembled from:

- curated internal lists
- GitHub topic pages
- selected product directories
- manual editorial sourcing

### 5.2 Publisher Submission

Later phases may allow direct publisher submission through an API.

### 5.3 Controlled Connectors

Future connectors may ingest:

- GitHub repository metadata
- documentation index pages
- changelogs
- pricing pages

## 6. Ingestion Pipeline

The ingestion pipeline should follow these steps:

1. accept or fetch source data
2. canonicalize URL
3. extract normalized metadata
4. generate or review summary
5. assign tags
6. deduplicate against existing corpus
7. validate quality and safety rules
8. compute embedding
9. write to vector store and metadata DB

## 7. Deduplication Rules

At minimum, the system should deduplicate on:

- canonical URL
- normalized project name
- repository owner and repo name
- strong textual similarity of title and summary

If two records appear to refer to the same project, the system should:

- merge them when safe
- otherwise queue them for operator review

## 8. Summary Generation

Summaries should be:

- short
- concrete
- descriptive rather than promotional
- semantically useful for retrieval

Bad summaries:

- generic marketing copy
- keyword stuffing
- empty slogans

If an LLM is used to produce normalized summaries, the original source text should be retained for audit and correction.

## 9. Tagging

The MVP should use a controlled vocabulary rather than free-form tags only.

Suggested tag groups:

- category
- audience
- deployment
- pricing
- license
- primary function

Examples:

- auth
- vector-db
- scraping
- local-first
- self-hosted
- API
- SDK
- open-source

## 10. Trust and Quality Levels

Each item should carry a trust or review status:

- `editorial`
- `publisher_verified`
- `imported_unreviewed`
- `flagged`
- `rejected`

This does not need to be exposed in the first UI, but it should exist operationally.

## 11. Publisher API Goals

The Publisher API should eventually allow trusted publishers to:

- submit new items
- update metadata
- request reindexing
- verify ownership
- view moderation outcomes

## 12. Publisher API Authentication

The initial API should use:

- API keys for trusted publishers
- optional domain verification
- rate limiting

Later improvements may add:

- OAuth
- signed update requests
- organization-level access

## 13. Draft Publisher API Endpoints

### Create Item

`POST /api/publisher/items`

Example request:

```json
{
  "canonical_url": "https://example.com/tool",
  "source_type": "developer_tool",
  "title": "Example Tool",
  "summary": "A lightweight self-hosted webhook debugger.",
  "tags": ["webhooks", "self-hosted", "developer-tool"],
  "license": "MIT",
  "pricing_model": "free"
}
```

### Update Item

`PATCH /api/publisher/items/:id`

### Reindex Item

`POST /api/publisher/items/:id/reindex`

### Verify Ownership

`POST /api/publisher/verify`

## 14. Validation Rules

Requests should be rejected if:

- URL is invalid
- title is empty
- summary is too short or obviously spammy
- tags are malformed
- duplicate ownership conflicts exist
- the domain or resource is blocked

## 15. Moderation Workflow

Every new publisher-submitted item should enter one of these states:

- accepted automatically
- accepted with review pending
- held for manual review
- rejected

Triggers for review:

- new publisher
- suspicious duplication
- spam-like copy
- misleading category claims
- abusive or unsafe content

## 16. Reindexing Policy

An item should be reindexed when:

- metadata changes materially
- the summary changes
- the tagging model changes
- the embedding model changes

The system should track:

- item embedding version
- summary version
- ingestion timestamp
- last successful reindex

## 17. Operational Tooling

Operators need:

- review queue
- duplicate queue
- failed-ingestion log
- publisher history view
- item diff view
- bulk reindex controls

## 18. Anti-Spam Controls

Phase 1 controls:

- curated allowlist sources
- manual approval for first publisher submissions
- rate limits
- canonical URL checks
- duplicate detection

Future controls:

- reputation scoring
- anomaly detection
- semantic spam classifiers

## 19. Privacy Considerations

Publisher ingestion is different from user query privacy.

Publisher-facing systems may need:

- contact email
- ownership verification
- moderation logs

These should be stored separately from user-side retrieval telemetry.

## 20. Bottom Line

Synapse-0 will live or die on corpus quality in its first phase. The Publisher API should therefore arrive only after the team has a stable ingestion pipeline, clear quality rules, and an operator workflow that can keep the index useful.
