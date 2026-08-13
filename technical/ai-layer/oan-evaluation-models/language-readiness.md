# Language Readiness

Language Readiness for Bharat Vistaar

This defines the division of responsibility between the program team and the respective State teams for evaluating and onboarding a new language into the Bharat Vistaar project.

Readiness Definition

A language is considered ready for Bharat Vistaar only when all of the following criteria are met:

Text LLM layer :&#x20;

* Translation quality is strong.
* Direct native-language answering is strong.
* Tool calling and search terms based on new language work with good accuracy
* Supporting glossary of key agri words are integrated and tested
* Moderation/safety behavior is reliable.
* Persona consistency is maintained in new language&#x20;

Voice Layer :&#x20;

* ASR quality is good in various situations - noisy background, accents, gender/age etc
* LLM gives out TTS ready text output - its compact, doesn’t give extra symbols, necessary preprocessing for symbols and numbers is present&#x20;
* TTS voice is engaging and natural <br>

Evaluation Workflow

The evaluation process follows a structured, iterative workflow to ensure a comprehensive readiness assessment.



1. PT team Action: Define Benchmark, Metrics, and Cutoffs.  ( 1 day)
2. State Action: Provide  Agri Term Glossary/ verify provided glossary ( 2 days)
3. PT team Action: Run Automated Evaluation and share preliminary results with the State team.  ( 1 day)
4. PT team Action: Define guidelines for H-Eval and train any members if required (2 days)
5. State Action: Conduct comprehensive Human Evaluation (H-Eval) using the defined rubric. (10 days)
6. State Action: Provide Language-Based Feedback (qualitative) to PT team on areas of weakness.
7. PT team Action: Review H-Eval data and feedback, compare against cutoffs, and make the Final Readiness Decision. ( 3 days)
8. Go-Live (or Iteration): If ready, the language is onboarded to Bharat Vistaar. If not ready, PT and the State team define a focused iteration plan and training cycle
9. Training data creation and verification → training of open source models. Create a base dataset to be translated across all languages that can be used to train any model to improve performance on BV questions. This is detailed more [below](https://docs.google.com/document/d/1Z1--N6wS65AnUOd99AKP5HLnjOlDeTMzX2oA4SUf_-o/edit?tab=t.0)

Program team Responsibilities

The team is responsible for defining the technical standards, metrics, and operational cutoffs for language readiness.

| Task                                                | Description                                                                                                               | Details                                                                                                                   |
| --------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------- |
| Define Benchmark Dataset                            | Specify the fixed set of samples used for automated and human-based translation and reasoning evaluation.                 | The Fixed Dataset Design (500 Samples) must be fully defined                                                              |
| Define Evaluation Metrics                           | Establish the quantitative metrics for measuring language quality, factual accuracy, moderation, and agentic capability.  | Metrics must cover translation quality, native-language performance, and reasoning.                                       |
| Define guidelines for human evals                   | Define the guidelines for scoring all eval metrics                                                                        | Guidelines doc for each metric                                                                                            |
| Define Readiness Cutoffs                            | Set the non-negotiable quantitative thresholds that a language must meet across all defined metrics to be declared ready. | Cutoffs must be based on the shared 0-4 human-only scoring rubric (as defined in the Language Expansion Evaluation Spec). |
| Automated Evaluation through LLM                    | Run the LLM against the benchmark dataset and calculate all defined metrics.                                              | Provide preliminary scores of top 2 models to the State team before human evaluation commences.                           |
| Setup Devbox for integration of new model/language  | Bharat Vitaar is setup such that one can easily switch LLMs,prompts, ASR, TTS model to generate new outputs               | Provide generated answers for evaluation through automated setup                                                          |
| Synthetic data generation setup                     | Setup synth data setup for data generation for training if current model not crossing eval benchmarks                     |                                                                                                                           |

State Team Responsibilities

The State teams are responsible for providing critical linguistic and contextual expertise through consistent human evaluation and data support.

| Task                                         | Description                                                                                                                                                                                                                        | Deliverable                                                                                                                        |
| -------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------- |
| Benchmark dataset translation and speech     | Team will confirm translations of all questions and will also provide speech for all voice based questions                                                                                                                         | Translated benchmark questions in required language and accents                                                                    |
| Human Evaluation (H-Eval)                    | Conduct human-only scoring (0-4 rubric) for checking if the language is ready, focusing on: language quality, factual and task quality, native-language agentic capability, moderation and safety behavior, and persona adherence. | Completed human evals based on defined metrics                                                                                     |
| Provide Agri Term Glossary                   | Curate and submit a list of essential agricultural terminology specific to the state's language and context, verify local lang translation of glossary terms                                                                       | Glossary for key agri terms, consistent updating of glossary based on regular feedback cycle                                       |
| Language-Based Feedback of bot answers       | Have weekly review of sample bot answers and provide feedback on answers                                                                                                                                                           | Decide personnel for consistent feedback cycle on languages                                                                        |
| Supplementing knowledge gaps with documents  | For all agri related answers where the bot is unable to retrieve related docs, team provides docs/APIs/knowledge base to supplant info                                                                                             | knowledge/docs tracker - personnel to decide if current knowledge is enough to answer some questions and framework to add new docs |
| Workflow definitions                         | Provide a definition of any new/state specific workflows that need to be implemented (e.g it should hit this source then this API and answer without modification and so on)                                                       |                                                                                                                                    |
| Resource Allocation                          | Ensure necessary personnel (native speakers, domain experts) are allocated to perform the required human evaluations and glossary curation.                                                                                        | Confirmation on  resource availability                                                                                             |
| Verification of training data (if required)  | If current systems are not good enough at instruction following, some data will be created using synthetic methods and state team will qualify, rate this data                                                                     | Rankings and feedback on synth data generated                                                                                      |

\
\
\
\
<br>

Further Training & Fine-Tuning SOP

&#x20;

Training Workflow

The further training of the models follows the below steps<br>

1. State Action: Provides specific feedback on current setups including issues with translation, instruction following and moderation - ideally following the evaluation framework defined (10 days)
2. PT team Action: Calibrate eval setup to match state feedback   (2 days)
3. PT team Action: Creates some sample synthetic data by sourcing data from the provided datasets and incorporating team feedback. This synthetic data is generated through multiple LLMs to compare ( 3 days)
4. State Action: State team reviews a small sample of the synthetically generated data and provides feedback on which LLM is generating the best outputs and any further processing required ( 3 days)
5. PT team Action: Create final finetuning dataset in SFT(Supervised Fine-Tuning) format  dataset containing 40% feedback data and 60% overall categories including moderation (2 days)
6. PT team Action: Fine tune using LoRA and evaluate checkpoints for the best outputs (10 days)
7. PT team Action: Integrate new checkpoint in dev setup and continue evaluation cycle (2 days)

&#x20;

\*Number of iterations: at least 2; the time required for state action in each iteration will reduce by \~50%

Team based responsibilities

The further training workflow defines the responsibilities of both the Program Team and the State Teams for improving model quality through iterative feedback, synthetic data generation, fine-tuning, and evaluation.

Program Team responsibilities

<table data-header-hidden><thead><tr><th valign="top"></th><th valign="top"></th><th valign="top"></th></tr></thead><tbody><tr><td valign="top">Task</td><td valign="top">Description</td><td valign="top">Deliverable</td></tr><tr><td valign="top">Calibrate Evaluation Setup</td><td valign="top">Align evaluation setup and scoring with feedback received from State teams regarding translation, moderation, and instruction-following gaps.</td><td valign="top">Updated evaluation configuration and scoring alignment</td></tr><tr><td valign="top">Synthetic Data Generation</td><td valign="top">Generate synthetic instruction-following data using provided datasets, workflows, documents, and evaluator feedback. Compare outputs across multiple LLMs.</td><td valign="top">Sample synthetic datasets from multiple LLMs</td></tr><tr><td valign="top">Final Fine-Tuning Dataset Creation</td><td valign="top">Curate final SFT dataset containing 40% corrective feedback data and 60% general workflow/category coverage including moderation.</td><td valign="top">Versioned SFT dataset</td></tr><tr><td valign="top">Fine-Tuning and Checkpoint Evaluation</td><td valign="top">Carry out LoRA-based supervised fine-tuning and evaluate checkpoints against the defined evaluation framework.</td><td valign="top">Ranked checkpoints with evaluation scores</td></tr><tr><td valign="top">Dev Integration and Iteration</td><td valign="top">Integrate selected checkpoint into Bharat Vistaar dev setup and continue iterative evaluation and feedback cycle.</td><td valign="top">Updated dev deployment with latest checkpoint</td></tr></tbody></table>



State Team Responsibilities

<table data-header-hidden><thead><tr><th valign="top"></th><th valign="top"></th><th valign="top"></th></tr></thead><tbody><tr><td valign="top">Task</td><td valign="top">Description</td><td valign="top">Deliverable</td></tr><tr><td valign="top">Provide Language and Workflow Feedback</td><td valign="top">Share structured feedback on translation quality, moderation behavior, instruction following, terminology, and workflow execution using the defined evaluation framework.</td><td valign="top">Feedback reports and annotated examples</td></tr><tr><td valign="top">Review Synthetic Data Samples</td><td valign="top">Review sampled synthetic datasets generated by the Program Team and assess linguistic quality, workflow adherence, and contextual correctness.</td><td valign="top">Rankings and qualitative feedback on generated data</td></tr><tr><td valign="top">Suggest Processing Improvements</td><td valign="top">Recommend improvements for prompting, formatting, terminology usage, moderation handling, and post-processing where required.</td><td valign="top">Actionable recommendations for dataset refinement</td></tr><tr><td valign="top">Validate Fine-Tuned Outputs</td><td valign="top">Evaluate outputs from fine-tuned checkpoints during iterative testing cycles.</td><td valign="top">Validation feedback for checkpoint selection</td></tr></tbody></table>



Eval Framework Metrics

This section outlines the combined evaluation metrics for the system once LLM has been decided. LLM for a language is decided based on [IndicIFEval](https://huggingface.co/datasets/sarvamai/mmlu-indic), [IndicMMLU](https://huggingface.co/datasets/sarvamai/mmlu-indic), [IndicGenBench](https://github.com/google-research-datasets/indic-gen-bench)<br>

<table data-header-hidden><thead><tr><th valign="top"></th><th valign="top"></th><th valign="top"></th><th valign="top"></th></tr></thead><tbody><tr><td valign="top">Dimension</td><td valign="top">Metric</td><td valign="top">Description</td><td valign="top">Human evals required</td></tr><tr><td valign="top">Factual Grounding</td><td valign="top">citation_comprehensiveness</td><td valign="top">Are the sources pulled enough to answer the question comprehensively</td><td valign="top"><p>Yes -> Domain knowledge</p><p> </p><p>( 1-4 rubric) </p></td></tr><tr><td valign="top"></td><td valign="top">no_fabrication</td><td valign="top">Absence of hallucinated or unsupported information.</td><td valign="top"><p>Yes → Language expert</p><p> </p><p>(0/1 score)</p></td></tr><tr><td valign="top"></td><td valign="top">citation_accuracy</td><td valign="top">Correctness of cited sources/references.</td><td valign="top"><p>Yes → Language expert</p><p> </p><p>(1-4 rubric)</p></td></tr><tr><td valign="top"> Response Usefulness</td><td valign="top">completeness</td><td valign="top">Whether the response fully answers the user's query.</td><td valign="top">Yes → Language expert<br>(1-4 rubric) </td></tr><tr><td valign="top"></td><td valign="top">actionability</td><td valign="top">Practical utility and guidance provided by the response.</td><td valign="top">Yes → Language expert<br>(1-4 rubric)</td></tr><tr><td valign="top"></td><td valign="top">Safety_compliance (moderation/jailbreak)</td><td valign="top">Adherence to safety and moderation policies, system to refuse to answer questions out of domain</td><td valign="top"><p>Yes → Language expert</p><p> </p><p>(0/1 score)</p></td></tr><tr><td valign="top"></td><td valign="top">context_fit</td><td valign="top">Relevance to the conversational context.</td><td valign="top">Yes → Language expert<br>(0/1 rubric)</td></tr><tr><td valign="top"></td><td valign="top">conversation_closure</td><td valign="top">Effectiveness in concluding the interaction.</td><td valign="top">Yes → Language expert<br>(0/1 rubric)</td></tr><tr><td valign="top">Linguistic Quality</td><td valign="top">grammar</td><td valign="top">Grammatical correctness in the target language.</td><td valign="top">Yes → Language expert<br>(1-4 rubric)<br>Detail out grammatical corrections</td></tr><tr><td valign="top"></td><td valign="top">terminology</td><td valign="top">Correct usage of domain-specific (agri) vocabulary.</td><td valign="top">Yes → Language expert<br>(1-4 rubric)<br>Detail out incorrect usage </td></tr><tr><td valign="top"></td><td valign="top">language_purity</td><td valign="top">Avoidance of unnecessary loanwords or code-switching.</td><td valign="top">Yes → Language expert<br>(1-4 rubric)<br>Detail out incorrect usage </td></tr><tr><td valign="top"></td><td valign="top">fluency</td><td valign="top">Natural flow and readability.</td><td valign="top">Yes → Language expert<br>(0-1  rubric)</td></tr><tr><td valign="top"></td><td valign="top">translation</td><td valign="top">Spoken naturalness, number rendering, and spoken terminology quality (e.g., Gujarati).</td><td valign="top">Yes → Language expert<br>(1-4 rubric)<br>Detail out incorrect usage </td></tr><tr><td valign="top">Voice Channel Specifics</td><td valign="top">brevity</td><td valign="top">Voice-appropriate response length (compactness for speech).</td><td valign="top">No (word length) </td></tr><tr><td valign="top"></td><td valign="top">voice_ready</td><td valign="top">Absence of text artifacts (markdown, lists) in spoken output.</td><td valign="top">No </td></tr><tr><td valign="top"></td><td valign="top">comprehensiveness</td><td valign="top">Voice comprehensiveness is defined differently such that brevity is prioritized and in incomplete parts are suggested as “Can I explain this next?”</td><td valign="top">Yes → Language expert<br>(1-4 rubric)<br>Detail out incorrect usage </td></tr><tr><td valign="top"></td><td valign="top">tone</td><td valign="top">Respectful, warm, and conversational style.</td><td valign="top">Yes → Language expert<br>(1-4 rubric)</td></tr><tr><td valign="top">Voice Channel Specifics (voice based)</td><td valign="top">Input Word &#x26; Character Error Rate</td><td valign="top">WER on input audio</td><td valign="top"><p>Yes → Language expert</p><p>(How many words are wrong and their corrections)</p></td></tr><tr><td valign="top"></td><td valign="top">WER &#x26; MOS score on voice tone and pronunciation</td><td valign="top">Words pronounced correctly<br><br><br></td><td valign="top"><p>Yes → Language experts </p><p>(How many words were spoken correctly)</p><p> </p></td></tr><tr><td valign="top"></td><td valign="top">MOS on voice tone and naturalness</td><td valign="top">Mean Opinion Score - naturalness of audio</td><td valign="top">Yes → Language experts (1-4)</td></tr><tr><td valign="top">Runtime / Performance</td><td valign="top">elapsed_seconds</td><td valign="top">Total time taken for the response.</td><td valign="top">No</td></tr><tr><td valign="top"></td><td valign="top">ttfb (Time to First Byte/Chunk)</td><td valign="top">Responsiveness for streaming voice output.</td><td valign="top">No</td></tr><tr><td valign="top"></td><td valign="top">word_count</td><td valign="top">Length of the output response in words.</td><td valign="top">No</td></tr><tr><td valign="top"></td><td valign="top">token_usage.input</td><td valign="top">LLM input token count.</td><td valign="top">No</td></tr><tr><td valign="top"></td><td valign="top">token_usage.output</td><td valign="top">LLM output token count.</td><td valign="top">No</td></tr><tr><td valign="top"></td><td valign="top">error</td><td valign="top">Any system or LLM execution error.</td><td valign="top">No</td></tr><tr><td valign="top">Process Fidelity (Text/Agentic)</td><td valign="top">agristack_workflow</td><td valign="top">Adherence to defined workflow/system steps.</td><td valign="top">No</td></tr><tr><td valign="top"></td><td valign="top">term_identification</td><td valign="top">Correct identification and processing of key terms.</td><td valign="top"><p>Yes - language expert</p><p>(0/1 prev</p></td></tr><tr><td valign="top"></td><td valign="top">tool_sequencing</td><td valign="top">Correct order and usage of available tools.</td><td valign="top">No </td></tr><tr><td valign="top"></td><td valign="top">search_quality</td><td valign="top">Relevance and effectiveness of internal/external search queries.</td><td valign="top">No</td></tr><tr><td valign="top"></td><td valign="top">output_hygiene</td><td valign="top">Cleanliness of the final text output (e.g., lack of boilerplate/ code details)</td><td valign="top">No</td></tr></tbody></table>

&#x20;

Combined Scoring Approach

The final readiness decision will be a combination of the metrics as required&#x20;

&#x20;
