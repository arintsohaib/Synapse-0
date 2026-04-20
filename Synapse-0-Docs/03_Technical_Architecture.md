# Technical Architecture: Synapse-0

## Overview
Synapse-0 is a completely decoupled system. The semantic embedding logic runs exclusively on the Client (Frontend), while the similarity matching and data storage run on the Server (Backend Mesh).

## 1. Client-Side (The Local Vault)
The client must be able to embed text into a dense vector (e.g., 384 dimensions) in milliseconds, natively in the browser, without an external API.

### Tech Stack:
- **Framework:** React / Next.js
- **Model Execution:** `Transformers.js` (Hugging Face)
- **Model Choice:** Built-in `Xenova/all-MiniLM-L6-v2` or similar lightweight ONNX models. It takes ~30MB of memory and embeds text in ~10-50ms via WebAssembly/WebGPU.
- **Data Flow:**
  1. User inputs text (intent).
  2. Text is passed to Transformers.js `pipeline('feature-extraction')`.
  3. A raw Float32Array (Vector) is generated.
  4. Only this array is transmitted via WebSocket or HTTPS POST.

## 2. Backend Mesh (The Geometry Engine)
The backend does no NLP context processing. It receives a vector and searches its graph for nearest neighbors.

### Tech Stack:
- **Server:** Node.js (Express) or Go/Rust for high performance.
- **Database:** Vector Database
  - **Milvus / Qdrant:** Highly scalable, open source, supports HNSW index for ultra-fast billion-scale vector search.
  - **Redis Stack:** If we need hyper-fast in-memory matching.
- **Storage:** PostgreSQL (to store the actual payload descriptions/links associated with the vectors).

### Workflow:
1. Server receives Vector `V`.
2. Database executes Cosine Similarity Search (`SELECT payload FROM items ORDER BY vector <=> V LIMIT 10`).
3. Payload metadata is returned to the user.

## 3. Security & Zero-Knowledge Properties
- Because the embedding models are "Dense", returning to the original input text from the vector is mathematically impossible without brute-forcing an infinite combination of words.
- The server logs only show mathematical queries, meaning even if the server is hacked or subpoenaed, user intent is cryptographically safe.
