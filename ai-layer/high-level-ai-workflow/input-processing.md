# Input Processing

For text queries, the input is passed directly to the AI processing pipeline.

For voice queries, the system first performs Automatic Speech Recognition (ASR). OpenAgriNet uses Bhashini for speech-to-text conversion. In current deployments, automatic language detection at the ASR layer is configured selectively based on the target user base.&#x20;

For example, in Maha VISTAAR, Bhashini is configured to prioritise Marathi inputs rather than using fully automatic language detection. This design choice was made because Hindi and Marathi share several phonetic and lexical similarities, which can lead to ambiguity and misclassification in automatic detection for rural speech patterns. By constraining the ASR to a known primary language, the system achieves higher transcription accuracy and more reliable downstream processing.&#x20;

In deployments such as Bharat VISTAAR, where multiple languages are actively supported, automatic language detection can be enabled as a configurable option.

Voice input is converted into an English text query, which becomes the standard input format for downstream processing. This normalisation allows the AI pipeline to remain language-agnostic internally, while still supporting multilingual user interactions at the edges.
