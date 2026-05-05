# LLM Call Sequencing

The number of LLM calls per query depends on the use case and the tools involved. Typically:

* First call: Moderation
* Second call: Tool selection and invocation
* Third call: Data processing and response generation

In some complex scenarios, particularly where multiple API calls are required, the total number of calls may increase to three or four.

For example, if a farmer asks, “What is the mandi price of soybean today?”, the first LLM call validates that the query is an agriculture-related request. The second call determines that the mandi price tool should be invoked and triggers the relevant API. Once the price data is retrieved, a third call processes the response and presents the information to the user in a clear, structured format.

<br>
