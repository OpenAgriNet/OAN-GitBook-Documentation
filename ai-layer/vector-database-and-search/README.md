# Vector Database and Search

OpenAgriNet uses Marqo as its vector database to enable fast and accurate semantic search across ingested agricultural knowledge and programme content. During data ingestion, processed documents are converted into vector embeddings and indexed along with relevant metadata such as source, document identifiers, and content type.&#x20;

At runtime, user queries are converted into embeddings and matched against stored vectors using similarity search to retrieve the most relevant content chunks. These retrieved results are then passed to the AI agent to generate grounded responses, ensuring that information delivered to users is contextually relevant, traceable to source institutions, and aligned with OAN’s principles of verifiable and auditable knowledge delivery.
