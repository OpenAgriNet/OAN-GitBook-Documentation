# State-Agnostic AI Core

## Context

OAN already runs multiple deployments (MahaVISTAAR, Bharat VISTAAR) with different languages, tool sets, and data sources. Without a deliberate boundary, state-specific logic tends to leak into the ingestion and reasoning layers, making each new state deployment a fork rather than a configuration.

## Decision

The ingestion pipeline and the AI orchestration logic are kept state-agnostic by design:

* Documents from any state are treated as external inputs to the same OCR → chunking → vector-DB ingestion pipeline; the vector DB schema, indexing, and similarity search are unchanged across states, with state-specific content distinguished only via metadata (source, document IDs).
* The AI workflow (moderation, intent identification, tool selection, response generation) runs the same sequence regardless of state. Differences in use cases, APIs, or languages are handled through configuration and tool availability, not through per-state agent modifications.
* User queries in any regional language are normalised into a common processing language before entering the pipeline, so retrieval and reasoning logic stays uniform.

## Why it matters

* Onboarding a new state becomes: collect and validate documents, run the existing ingestion utilities, configure that state's tools/APIs, enable its languages — with no changes to the underlying AI architecture, agent design, or retrieval mechanisms.
* This is what let the same AI core be reused as-is across MahaVISTAAR and Bharat VISTAAR, with the two deployments differing only in the tools/APIs exposed (e.g. status and grievance tools in Bharat VISTAAR), the interaction language, and the data sources integrated — not in the core reasoning pipeline.
* The tradeoff made explicitly: ingestion currently happens only in English (queries are translated/normalised into English first), which the architecture supports extending to multilingual ingestion later with minimal changes, rather than solving upfront.
