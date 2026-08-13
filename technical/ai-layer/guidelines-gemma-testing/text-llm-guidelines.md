# Text LLM Guidelines

#### Human Evaluation Guidelines (text only)



* Response Usefulness
* accuracy\_completeness (1-4)
* actionability (0/1)
* conversation\_closure (0/1)
* source\_data\_comprehensiveness (0/1)
* Linguistic Quality
* translation\_accuracy (1-4)
* grammar and fluency (1-4)
* language\_purity (1-4)
* Factual Grounding
* no\_fabrication (0/1)
* citation\_accuracy (0/1)



#### Response Usefulness

accuracy/ completeness (1-4): Whether the response accurately fully answers the user's query.

* Metric Description: Evaluates if the response addresses all parts of the user's question with the necessary detail. The model is allowed to say that it doesn’t have the data to respond to the question if that’s the case
* Reasoning:
* Score 4: Fully answers all aspects of the query (e.g., identifies the pest, provides mechanical control, and lists chemical dosages).
* Score 3: Answers the main question but leaves out a minor detail (e.g., mentions the pesticide but forgets the water dilution ratio).
* Score 2: Provides a partial answer or answers something related to the crop but not answering the question
* Score 1: The response is incorrect or talks about a different scheme/crop/topic than what was asked&#x20;

actionability (0/1): Practical utility and guidance provided by the response.

* Metric Description: Measures how easily a user can implement the advice provided.
* Reasoning:
* Score 1: the answer mentions clearly what the farmer can do, clear action points&#x20;
* Score 0: Theoretical or vague advice that provides no clear starting point for the farmer.
* Example:
* Question: "How do I set up traps for whiteflies?"
* Scoring Logic:&#x20;
* Score 1 if the response says: "Install yellow sticky traps 1 foot above the crop canopy and space them 20 meters apart."&#x20;
* Score 0 if the response says: "Pest control is important for better yields."

conversation\_closure\_followup (0/1): Effectiveness in concluding the interaction and asks followup

* Metric Description: Assesses if the model ends the message in a way that transitions to the next step or completes the conversation.&#x20;
* Reasoning:
* Score 1: Provides a clear closing or asks a relevant follow-up to keep the user supported.
* Score 0: Ends abruptly or leaves the user unsure if the model is finished.
* Example:
* Response (Score 1): "I hope this helps your cotton crop. Do you want exact measurements of fertilizer required?”
* Response (Score 0):  “.. Add 5 grams of potash.”&#x20;

Source\_data\_comprehensiveness (0/1): Checks if the model says that I don’t have the data to respond to the question.&#x20;

* Metric Description: Score 1 if the bot gives a relevant answer, score 0 if the bot says that it does not have the data to respond to the question



#### Linguistic Quality

Translation accuracy (1-4): Accuracy of the translations&#x20;

* Reasoning: Score 4 if everything is translated correctly and uses correct agri terms in those languages.\
  Score 3 if general translation is correct but there are some mistakes for agri specific terms etc.\
  Score 2 if there are multiple mistakes that change the meaning of the translations in some small parts.\
  Score 1 if there are major errors that change the meaning of the answer.<br>

Grammar and fluency (1-4): Grammatical correctness in the target language.

* Metric:  The response tone should be friendly while speaking in perfect language and style that the farmer can understand easily <br>
* Reasoning: Score 4 if it sounds like a native speaker wrote it with perfect structure  and grammar and is friendly\
  Score 3 if it the grammar and structure is overall correct the language is too cold/technical or some parts of the answer don’t read very naturally.\
  Score 2 if it feels word by word translated and grammar doesn't make sense in multiple places\
  Score 1 if the answer is grammatically completely incorrect&#x20;

Language\_purity (1-4): Avoidance of unnecessary loanwords or code-switching.

* Reasoning: It stays in the target language without mixing scripts/ words from another language. For example, if the answer is in Hindi , it should not add English full forms or English numerals.\
  Score 4 if there is no language code switching at all.\
  Score 3 if only English full forms are being used or English numerals are being used. Score 2 if it randomly switches to Marathi/English/Chinese words sometimes.\
  Score 1 if it gives entire/ a chunk of the answer in a different language&#x20;

#### Factual Grounding<br>

no\_fabrication (0-1): Absence of hallucinated or unsupported information.

* Metric Description: The model must not provide advice, dosages, or "common knowledge" facts. If the sources do not contain relevant information to answer the question, the model must state that it does not have the answer. It is a failure (Score 0) if the model uses external facts to "fill the gaps" when sources are empty or irrelevant.
* Reasoning:&#x20;
* Score 1 (No Fabrication): If sources are irrelevant, the model correctly states it cannot answer.&#x20;
* Score 0 (Fabricated): The response includes some fact, number, or instruction not found in the sources, or it provides a "correct" answer using external knowledge because the sources were insufficient.&#x20;
* Example:
* Question: "How do I manage whiteflies in cotton?"\
  <br>
* Scoring Logic:&#x20;
* Score 1 if the response says: "You can use yellow sticky traps and neem oil" or  "I don't have information on wheat fertilizer in the provided sources"&#x20;
* Score 0 if the response says: "There is no source that talks about fertilizer wheat but you can use neem oil and spray 80mg of Potassium”\
  <br>

citation\_accuracy (0/1): Correctness of cited sources/references.

* Metric Description: Evaluates whether the model correctly attributes information to the provided sources and ensures that every piece of information used is properly cited.
* Reasoning:
* &#x20;Score 1: All information pulled from sources is correctly cited with the corresponding reference.
* Score 0:  No citations are provided at all when its an advisory style question or citation is for the wrong source

\
<br>
