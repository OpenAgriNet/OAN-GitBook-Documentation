# Architectural Characteristics

#### AI as an Orchestration Layer

The AI agent does not generate factual agricultural data independently. All factual outputs are retrieved via tool calls to verified data systems before response generation.

#### Tool-Driven Data Retrieval

All operational responses (mandi prices, weather, schemes, etc.) are grounded in external data sources accessed through registered tools.

#### Streaming Response Design

Responses are streamed back incrementally using server-sent events (SSE), improving perceived latency during voice interactions.

#### Multilingual Processing

Voice input is captured in Marathi, normalised internally for processing, and converted back into Marathi speech for user delivery.

<br>
