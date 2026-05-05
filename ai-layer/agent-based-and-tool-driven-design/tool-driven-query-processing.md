# Tool-Driven Query Processing

Once intent is identified, the AgriNet Agent routes the query to the appropriate[ tool.](https://github.com/OpenAgriNet/mh-oan-api/tree/mh-prod/agents/tools)

All tool calls in the AI module are executed exclusively by the AgriNet Agent. Supporting agents such as the Moderation Agent and Suggestion Agent do not invoke tools directly.

Tools encapsulate external systems such as databases, registries, and APIs. Each tool defines:

* Input schema
* Output schema
* Validation rules
* Source metadata

The agent invokes tools in a two-step process:

1. Selects the appropriate tool based on intent and prompt rules
2. Passes structured parameters to retrieve data from the underlying system

Retrieved data is then processed by the agent and formatted into a user-facing response, without altering or fabricating the original source.

#### A few examples:

#### 1. Advisory Queries and the Search Tool

For advisory-type questions, the agent invokes a Search Tool backed by a vector database. This tool retrieves relevant knowledge documents, which are then passed back to the agent as contextual input. The agent processes these results and generates a grounded response for the user.

If a farmer asks, “What fertiliser should I apply for cotton at the flowering stage?”, the AgriNet Agent classifies this as an advisory query and invokes the Search Tool. The tool retrieves relevant advisory documents from the vector database, such as fertiliser guidelines issued by agricultural universities. These retrieved documents are passed back to the agent as context, and the agent generates a response grounded in this content.

#### 2. Weather Queries

Weather-related queries are routed directly to the Weather Tool. In Bharat VISTAAR, the tool fetches real-time or historical weather data from external APIs.In Maha VISTAAR, the tool connects to an internal department-managed weather database that stores daily records. In both cases, the retrieved data is passed back to the agent, processed by the model, and returned to the user.

If a user asks, “Will it rain in my area tomorrow?”, the agent routes the query to the Weather Tool. The tool fetches weather forecast data from an external API, and the response is processed and returned to the user.

#### 3. Other Tools

Similar patterns are followed for:

* Warehouse information
* Mandi prices
* Scheme and benefit checks
* Grievance and status queries (Bharat VISTAAR)

Each tool encapsulates a specific API or data source, ensuring clear separation between reasoning and data retrieval.

\
<br>
