# Key Components in Call Flow

1. **Telephony Provider**\
   Handles call routing, IVR prompts, and audio streaming between the user and backend systems.
2. **Speech-to-Text (STT) Engine**\
   Converts farmer voice input (Marathi) into text for downstream AI processing.
3. **voice-oan-api**\
   Acts as the orchestration layer between telephony infrastructure and the AI module. Handles session management, query routing, and response streaming.
4. **LLM / Voice Agent**\
   Performs intent understanding, tool selection, orchestration, and response generation.
5. **Data Sources**\
   Includes real-time or near real-time data providers such as mandi price systems, weather APIs, scheme databases, and advisory knowledge systems.
6. **Text-to-Speech (TTS) Engine**\
   Converts AI-generated text responses into Marathi audio for delivery to the farmer.

<br>
