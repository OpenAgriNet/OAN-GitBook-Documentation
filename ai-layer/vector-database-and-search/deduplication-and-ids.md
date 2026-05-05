# Deduplication and IDs

To ensure reliable retrieval:

* Each row must have:
* A document ID (common across chunks from the same source)
* A unique ID (distinct for each chunk)
* This prevents loss of chunks and ensures proper deduplication

If not explicitly handled, Marqo applies automatic deduplication, which can unintentionally suppress valid chunks.

<br>
