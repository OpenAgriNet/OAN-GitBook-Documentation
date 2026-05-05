# Tool Invocation & Term Identification

For advisory and knowledge-based queries, the AgriNet Agent follows a structured term identification workflow before retrieving content.

Instead of directly searching documents using raw user input, the agent first invokes a search terms tool to identify standardised agricultural terminology. This step resolves:

* Local crop names
* Colloquial expressions
* Romanised regional language inputs
* Ambiguous or incomplete terms

The verified terms returned by the tool are then used to construct precise search queries for the document retrieval system. This two-step process improves retrieval accuracy and ensures semantic consistency across languages and regions.

For example, if a farmer asks:\
“Which fertiliser should I apply for pigeonpea and green gram?”\
The system first identifies standard terms such as pigeonpea, green gram, and fertiliser, and then performs document search using these normalised terms.
