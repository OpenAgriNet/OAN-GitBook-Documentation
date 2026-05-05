# Supporting Multilingual and Multimodal Access

The AI module is built to support multilingual and multimodal interactions, recognising the diversity of users across states and regions.

At the user level:

* Queries can be entered as text or voice
* Regional languages are supported at the input and output layers

Internally:

* Voice queries are processed using Automatic Speech Recognition (ASR) to convert speech into text, and Text-to-Speech (TTS) is used to deliver responses back in audio form where required.
* Queries are normalised into a common processing language to ensure consistency
* Responses are generated and translated back to the user’s preferred language

This separation allows OAN to scale language support without duplicating logic across use cases or states.
