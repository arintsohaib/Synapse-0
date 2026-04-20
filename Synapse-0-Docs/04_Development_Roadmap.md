# Development Roadmap: Synapse-0

## Phase 1: The Local MVP (Weeks 1-4)
**Goal:** Prove the "Blind Routing" concept works in a browser.
1. Build a simple React frontend.
2. Integrate `transformers.js` to create sentence embeddings entirely in the browser RAM.
3. Setup a local Vector Database (e.g., Qdrant or ChromaDB) via Docker.
4. Populate the DB with a seed dataset (e.g., 50,000 descriptions of indie tools, Github repos, or books).
5. Demonstrate that a user typing an abstract query on the frontend instantly retrieves the right data purely via vector matching over an API call.

## Phase 2: The "Anti-Algorithm" Features (Weeks 5-8)
**Goal:** Implement the features that make this better than Google.
1. Add the **Perspective Slider**: Calculate inverse/orthogonal vectors to show conflicting opinions or alternative tools.
2. Enhance performance with WebGPU for instant embeddings on mobile devices.
3. Build the "Publisher API": Allow developers and content creators to submit their own content directly into the Vector Database (cutting out web scrapers).

## Phase 3: The Decentralized Mesh (Weeks 9-16)
**Goal:** Eliminate the central server.
1. Transition from a central Vector Database to a decentralized P2P graph. 
2. Nodes in the network share HNSW (Hierarchical Navigable Small World) index graphs.
3. Queries are routed akin to the Tor network, bouncing through nodes to find the closest vector match without revealing the IP of the requester.

## Phase 4: Commercialization & "Blind Ad" Network
**Goal:** A sustainable economic model.
1. Introduce "Resonance Bidding". A company can pay to host their vector in the premium tier, but they still only match if the math perfectly aligns with the user's intent. 
2. No target demographics, no spy pixels. If the math doesn't match >90%, the result is physically not shown.
