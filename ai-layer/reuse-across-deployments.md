# Reuse across Deployments

The OAN AI module is designed in a modular way to leverage the effort and tools across various implementations. This ensures the base framework remains the same, and each implementation has flexibility to use the custom workflow such as KVK, Mandi prices, etc based on the context and data source availability.&#x20;

The AI module has been architected as a shared, reusable capability across multiple OAN deployments.

Current deployments like Bharat VISTAAR and Maha VISTAAR both:

* Use the same core AI architecture
* Follow the same agent-based workflow
* Share common tooling patterns (search, weather, advisory)
* Are LLM agnostic and capable of multi-agentic and multi LLM flow

Differences between deployments arise primarily from:

* The set of tools and APIs exposed (e.g. status and grievance tools in Bharat VISTAAR)
* The language of interaction
* The data sources integrated
* The language model/s used

This approach allows OAN to maintain a single AI core while supporting contextual customisation at the state or programme level.

#### <mark style="color:$primary;">Language Support</mark>

While the vector database is configured with a multilingual model, ingestion currently happens only in English. User queries in Marathi, Hindi, or English are translated or normalised before processing.

&#x20;For example, a farmer’s query is first converted into English, and variations in phrasing or terminology, such as local or colloquial crop names like “kapas”, are mapped to a standard representation such as “cotton”, ensuring that advisory content is retrieved consistently regardless of how the crop is referred to.

The architecture supports future multilingual ingestion with minimal changes.

#### <mark style="color:$primary;">Designed for Scale, Replication, and Governance</mark>

From inception, the AI module has been designed with:

* State/geography-level replication in mind
* Clear separation between data ingestion, reasoning, and delivery
* Human oversight in critical data preparation workflows and response evaluation
* Monitoring and logging for continuous improvement

As a result, the same AI pipeline can be extended to new states or use cases by:

* Adding new data sources
* Integrating new APIs
* Updating prompts and guardrails
* Testing and benchmarking the right LLM/SLMs

No fundamental architectural changes are required.
