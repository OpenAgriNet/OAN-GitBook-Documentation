# Chunking and Human Intervention

Markdown outputs often contain LaTeX-style artefacts, excessive separators, and uneven row structures, especially in tables. Fully automated chunking sometimes results in:

* Uneven token distribution
* Loss of semantic coherence

Chunking is performed using token-based limits rather than character length to ensure compatibility with downstream language model constraints. Minimum and maximum token thresholds are enforced for each chunk.

Chunks that exceed the defined token limit are further split, while chunks below the minimum threshold are filtered out. This ensures that each chunk maintains semantic coherence and is suitable for retrieval and response generation.

As a result, the ingestion process is semi-automated, with mandatory human intervention at the final stage to:

* Clean artefacts
* Validate chunk quality
* Ensure compliance with ingestion format

This approach balances scalability with data quality.

For example, after OCR, a document containing tables may produce uneven Markdown output, where some rows contain only separators or partial text. These artefacts are manually reviewed and cleaned to ensure that each chunk contains meaningful, complete information before ingestion into the vector database.

<br>
