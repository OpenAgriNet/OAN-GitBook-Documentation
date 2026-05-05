# Portability Across States

The ingestion utilities and the AI processing pipeline in OpenAgriNet are intentionally designed to be state-agnostic, enabling reuse of the same technical components across multiple state deployments without requiring changes to the core architecture.

From a data perspective, the system does not embed any state-specific logic into the ingestion or retrieval layers. Advisory documents, scheme-related content, or institutional materials from any state are treated as external inputs. Once documents for a new state are made available, typically through state departments, agricultural universities, or programme teams, the same document ingestion pipeline can be executed. This includes OCR processing, text cleaning, token-based chunking, and preparation of datasets in the standard schema required for vector database ingestion.

The vector database configuration, including schema definition, indexing approach, and similarity search mechanism, remains unchanged across states. State-specific content is distinguished through metadata such as source and document identifiers, rather than through separate database structures or customised logic.

Similarly, the AI workflow that handles user queries is reused as-is across deployments. The moderation process, intent identification, tool selection, and response generation follow the same sequence regardless of state. Differences between deployments such as supported use cases, APIs, or languages are handled through configuration and tool availability, not through modifications to the AI agents themselves.

Language handling also follows a consistent pattern across states. While user interactions may occur in different regional languages, queries are normalised into a common processing language before entering the AI pipeline, allowing the same reasoning and retrieval logic to be applied uniformly.

Because both the ingestion pipeline and the AI orchestration logic are decoupled from state-specific assumptions, onboarding a new state primarily involves:

* Collecting and validating source documents and datasets
* Running the existing ingestion utilities to prepare data for retrieval
* Configuring the relevant tools and APIs for that state’s use cases
* Enabling the appropriate languages at the interaction layer

No changes are required to the underlying AI architecture, agent design, or retrieval mechanisms. This approach enables rapid replication and scaling of OpenAgriNet deployments beyond Maharashtra, while ensuring consistency, maintainability, and predictable system behaviour across states.

\
<br>
