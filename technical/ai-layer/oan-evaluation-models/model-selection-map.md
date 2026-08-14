# Model Selection Map

OAN Evaluation/Model selection — Decision Map

How we pick and gate the LLM at the core of the OAN agent

Summary

We select a model in three narrowing stages. First, inference fit: does the model's size and serving profile work for us ; can it run on our hardware at the concurrency and latency we need? Models that clear that bar are then benchmarked on open-source metrics for reasoning and tool-calling, alongside  Indic benchmarks that test the same capabilities in Indic languages, to screen out models weak in Indic. The shortlisted models are finally run through the custom OAN evaluation pipeline, where a large model acts as an LLM-as-judge to decide based on multiple metrics which one best fits our needs across languages. That judge is itself calibrated against human evaluations on the same metrics, so we stay confident the automated pipeline tracks human judgement.

1\. LLM benchmarks used for selection

* Tool calling and intelligence

These metrics define the tool calling ability and general instruction following capability mostly in English, most big model releases publish these numbers

1. MMLU-Pro:  knowledge/reasoning
2. GPQA: hard reasoning
3. IFEval: instruction following
4. BFCL, τ²-bench: agentic / tool-calling — most relevant to OAN

* Indic

For Indic languages. We  use language specific tasks (translation, QA, summarization, instruction-following) to exclude models that show poor performance in required Indic languages

1. FloresIN :  translation accuracy
2. XorQA-IN :  cross-lingual question answering
3. CrossSumIN :  summarization  ability for Indic docs
4. IndicIFEval :  instruction following capabilities in Indic

2\. Custom eval benchmark (LLM-as-judge on simulated queries)

Benchmark question set across languages + judge model (big model as judge, not the same model as being evaluated ) → calibrate to human evals

Metrics scored by the judge (current pipeline)

Factual grounding — no\_fabrication, citation\_accuracy, citation\_comprehensiveness

Response usefulness — accuracy\_completeness, actionability, context\_fit, conversation\_closure, source\_data\_comprehensiveness, safety\_compliance

Linguistic quality — grammar\_fluency, terminology, language\_purity, translation\_accuracy

Voice channel — brevity, voice\_ready\_text, comprehensiveness, tone, WER / MOS (ASR/TTS layer, human-scored)

Process fidelity / agentic — agristack\_workflow, term\_identification, tool\_sequencing, search\_quality, output\_hygiene

<figure><img src="../../../.gitbook/assets/image (16).png" alt="" width="375"><figcaption></figcaption></figure>

Runtime / performance (deterministic) — elapsed\_seconds, ttfb, word\_count, token\_usage.input, token\_usage.output, error

3\. Keeping the eval benchmark fresh

We use logs as seed data and then simulate new questions using the new docs / APIs / tool calls that keep getting added to the system. We generate candidate questions → human-validate a sample → add to a versioned set. (Repeated periodically)

Guardrail: generator model ≠ judge model, so questions aren't graded by the model that wrote them.

4\. Thresholds for benchmarks

<table data-header-hidden><thead><tr><th valign="top"></th><th valign="top"></th><th valign="top"></th><th valign="top"></th></tr></thead><tbody><tr><td valign="top">Benchmark</td><td valign="top">Proposed cutoff</td><td valign="top">Gemma 4 31B</td><td valign="top">Qwen 3.6 27B</td></tr><tr><td valign="top">MMLU-Pro</td><td valign="top">≥ 82</td><td valign="top">85.2</td><td valign="top">86.2</td></tr><tr><td valign="top">GPQA Diamond</td><td valign="top">≥ 80</td><td valign="top">84.3</td><td valign="top">87.8</td></tr><tr><td valign="top">τ²-bench (agentic)</td><td valign="top">≥ 72</td><td valign="top">76.9</td><td valign="top">73.4</td></tr><tr><td valign="top">BFCL (function calling)</td><td valign="top">≥ 60</td><td valign="top">63.4</td><td valign="top">63.3</td></tr><tr><td valign="top">IFEval</td><td valign="top">≥ 80 <em>(provisional)</em></td><td valign="top">93.5</td><td valign="top">94.3</td></tr><tr><td valign="top">Latency (p50)</td><td valign="top">~5 sec</td><td valign="top">5</td><td valign="top">5</td></tr><tr><td valign="top">Load   (concurrent req served with 1 node)</td><td valign="top">20</td><td valign="top">20</td><td valign="top">20</td></tr></tbody></table>



Indic benchmarks

<table data-header-hidden><thead><tr><th valign="top"></th><th valign="top"></th><th valign="top"></th></tr></thead><tbody><tr><td valign="top">Benchmark</td><td valign="top">Metric</td><td valign="top">Cutoff</td></tr><tr><td valign="top">FloresIN (Indic MT)</td><td valign="top">chrF</td><td valign="top"></td></tr><tr><td valign="top">XorQA-IN (Indic QA)</td><td valign="top">F1</td><td valign="top"></td></tr><tr><td valign="top">CrossSumIN (Indic summ.)</td><td valign="top">ROUGE-L</td><td valign="top"></td></tr><tr><td valign="top">IndicIFEval</td><td valign="top">accuracy</td><td valign="top"></td></tr></tbody></table>

Similarly cutoffs are defined for each of the LLM as judge eval metrics

<figure><img src="../../../.gitbook/assets/image (17).png" alt=""><figcaption></figcaption></figure>

5\. Flow diagram

_\*\* Open items: similarly define metrics for ASR, TTS, embedding model/search_

**Self- hosting vs calling API’s**&#x20;

Pay-per-call scales linearly and punishes more calls. Owning GPUs turns cost into fixed infrastructure.

<img src="../../../.gitbook/assets/image (18).png" alt="" data-size="original">&#x20;

&#x20;
