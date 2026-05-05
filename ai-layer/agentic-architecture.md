# Agentic Architecture

The AI module follows a single-agent architecture (the AgriNet Agent) that orchestrates all reasoning, tool selection, and response generation.To improve safety and user experience, this core agent is supported by two specialised control agents:

* A Moderation Agent, which validates and filters incoming queries before they reach the core agent.
* A Suggestion Agent, which generates optional follow-up prompts (enabled in Maha VISTAAR only).

These supporting agents do not make independent decisions or invoke tools. All business logic, orchestration, and tool calls are executed by the AgriNet Agent.

#### <mark style="color:$primary;">**1. AgriNet Agent**</mark>

Once a query passes moderation, it is forwarded to the AgriNet Agent, which is the core intelligence of the AI module. This agent is responsible for:

* Understanding the user’s intent
* Deciding which tool(s) to invoke
* Orchestrating API calls
* Processing retrieved data
* Generating the final response

Tool selection is driven entirely through prompt-based reasoning. The prompt provided to the agent is aligned in such a way that, based on the user query, the agent can autonomously decide whether to:

* Call a search tool (for advisory or scheme information)
* Call a weather tool
* Call a warehouse or mandi API
* Call status or grievance APIs

Each use case is implemented through reusable, capability-based tools. The AgriNet Agent selects and invokes the appropriate tool based on user intent, ensuring that all responses are grounded in verified systems and APIs rather than generated directly by the model.

The system relies on LLM tool-calling capabilities, which is also why model selection and evaluation are critical. During experimentation, some models (for example, certain Mistral variants like Mistral 24B model) showed inconsistent tool invocation, whereas OpenAI models demonstrated more reliable tool calling behaviour.

The AgriNet Agent operates entirely through registered tools and does not respond from internal memory. All domain knowledge, factual data, and external information must be retrieved via explicit tool calls.

Each tool must be formally registered with the agent at configuration time. Only registered tools are visible to the agent and eligible for invocation. If a tool is not registered, the agent cannot reason about or use it, even if the underlying API exists.

Tool invocation is governed by natural-language system prompts that define:

* Which tool should be used for which class of query
* Mandatory ordering of tools (e.g. fetch profile before weather)
* Validation rules and fallback behaviour
* Source attribution requirements

This design ensures that:

* All responses are grounded in verifiable systems
* Tool usage is auditable and deterministic
* New use cases can be enabled simply by registering new tools and updating prompts

#### <mark style="color:$primary;">2. Moderation Agent</mark>

The Moderation Agent functions as a dedicated query validation layer. Every user query is first passed through this agent before reaching the AgriNet Agent.

Its primary responsibility is to validate whether the incoming query:

* Is relevant to agriculture and farmer welfare
* Falls within the permitted functional scope of OpenAgriNet
* Does not violate safety, policy, or domain-specific guardrails

The agent performs intent-based classification using contextual awareness and multi-turn conversation history, rather than simple keyword matching. This allows it to interpret short replies, follow-up questions, and ambiguous queries in relation to prior user interactions.

Queries are categorised into structured classes such as:

* Valid agricultural queries
* Non-agricultural or irrelevant queries
* Unsafe or illegal requests (e.g. banned pesticides)
* Political or controversial content
* Role manipulation or system override attempts
* Culturally sensitive topics

Only queries classified as valid agricultural are forwarded to the AgriNet Agent for further processing. All other queries are intercepted at this stage and handled using predefined, safe refusal templates. This ensures that out-of-scope, unsafe, or inappropriate requests never enter the main reasoning and tool orchestration flow.

In the current implementation, the Moderation Agent uses the same underlying language model as the AgriNet Agent, but operates with a dedicated system prompt and strict classification logic. The moderation logic is fully prompt-driven and configurable, allowing classification rules and guardrails to evolve as new use cases and domains are added to OpenAgriNet, without requiring architectural changes.

Over time, this design also allows the Moderation Agent to be decoupled and replaced with a specialised safeguards model (such as GPT-based safeguard models or Nemotron-style safety models). This future transition will enable stronger policy enforcement, improved robustness, and potentially lower operational costs, while preserving the same moderation interface and downstream workflow.

#### <mark style="color:$primary;">3. Suggestion Agent</mark>

A third agent, the Suggestion Agent, is designed to generate follow-up questions or suggestions to guide users after a response is delivered.

* In Maha VISTAAR, the Suggestion Agent is currently enabled.
* In Bharat VISTAAR, this agent has been removed for now to keep interactions more deterministic and task-focused.



\
<br>
