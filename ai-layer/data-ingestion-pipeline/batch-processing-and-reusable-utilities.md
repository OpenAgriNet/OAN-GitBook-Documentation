# Batch Processing & Reusable Utilities

The ingestion utilities support batch processing of multiple documents in a single execution. Documents can be processed at a folder level, enabling scalable ingestion of large datasets.

During batch execution, progress is tracked and logged. Failures for individual documents are captured separately, allowing ingestion to continue for remaining documents without interruption. This supports operational reliability and simplifies reprocessing of failed inputs.

The ingestion pipeline produces multiple output artefacts as part of AI module setup. These include cleaned Markdown files, structured and tokenised datasets prepared in the standard ingestion schema, and associated metadata required for vector database indexing. These artefacts form the input for the retrieval layer and are reused across deployments without requiring reprocessing, unless source documents change.

All OCR, document processing, and ingestion activities are performed offline as part of AI module setup and configuration. These steps are not invoked during live user interactions. At runtime, the AI module operates only on pre-ingested data through retrieval and tool-based APIs, ensuring predictable latency and performance during user queries.

<div data-with-frame="true"><figure><img src="../../.gitbook/assets/unknown (5).png" alt=""><figcaption></figcaption></figure></div>

<br>

<br>
