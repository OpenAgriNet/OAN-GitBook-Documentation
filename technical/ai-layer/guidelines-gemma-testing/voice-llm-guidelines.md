# Voice LLM Guidelines

#### Human Evaluation Guidelines (Voice only)



* Voice Channel Specifics (Voice)
* brevity (1-4)
* voice\_ready (0-1)
* tone\_naturalness (1-4)
* speech\_accuracy

brevity (1-4): Voice-appropriate response length.

* Metric Description: The answer should be short and spoken in under 30 seconds. If a comprehensive answers is long, then it just says the crux/first part of the answer and the rest of the answer is asked back as followup
* Reasoning:\
  Expected behaviour is that the response is short and is spoken under 30 seconds.  Score 4 if the response is compact and can be spoken in under 30 seconds and has a followup for the rest of the answer Score 3 if the bot gives a short answer but there is no proper followup question for the rest of the answer. Score 2 if the answer is around 30 seconds to 60 seconds. Score 1 if the answer is more than 60 seconds. <br>

voice\_ready\_text (0-1): Absence of text artifacts in spoken output.

* Metric Description: Output should be all speech and devoid of any text/chat artifacts like ‘:’, ‘\*’ that is spoken out by the TTS system.&#x20;
* Reasoning: Score 1 if the text is clean of markdown and doesn’t include any ‘non-spoken’ text. Score 0 if the AI literally says artifacts like "Asterisk," "Bullet point," or "Bold."\
  \
  Also point out the ‘word’/’symbol’ that were spoken

tone\_naturalness (1-4): Respectful, warm, and conversational style.

* Metric Description: Tone should be warm and friendly while sounding natural &#x20;
* Reasoning: Score 4 if the voice sounds natural and voice tone is friendly and helpful Score 1 if the tone is cold, dismissive, or overly robotic. Score 2, 3 depending on how robotic and unnatural it sounds &#x20;

**Speech\_accuracy**&#x20;

* Metric Description: Number of words spoken incorrectly or pronounced incorrectly. This is just a number of words that were spoken wrong/pronounced wrong.&#x20;
