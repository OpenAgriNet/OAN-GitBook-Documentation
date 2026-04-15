# README.md

OpenAgriNet (OAN) is a global network dedicated to transforming agriculture through Digital Public Infrastructure (DPI) and AI-powered solutions. By uniting governments, innovators, and organisations, we aim to revolutionise agricultural practices worldwide, driving sustainability and inclusivity in the sector.

**Purpose of this document**\
This documents is meant to provide all the necessary technical information that help all the participants who wants to build and integrate their application with the OAN Network.&#x20;

OAN Network is a Beckn protocol based network which allows transaction between Network Participants.



**Focus**\
OAN network aims to integrate the services like Schemes, Financial services, Market Access, Weather system, Soil health system with network and hence to all the participants.

Schemes and Weather work is progressed and initial integration is available to experience.



Arctiecture Diagram for OAN ;

<figure><img src="https://lh7-rt.googleusercontent.com/slidesz/AGV_vUceDKMDk0vaImh1zQC_MQCIIROQM-y6Vox5ityK0Q1BxZC7z6qgrb9WAMApVp70rM6HYWorT9_z22zfkQewnyZR4eDBZmSQxQGjiw5lnNoWWMhe2wPy0OXZJ5ltGnPqEPpJ2MEy1w=s2048?key=5vuxKC9WiJ7JMHnUZ8frzJWx" alt=""><figcaption></figcaption></figure>

<figure><img src="https://lh7-rt.googleusercontent.com/slidesz/AGV_vUc7Uh_wY2ri1sNWVUfOo27R5Y_Gx89drNmzpbPoAYPOcHvjpIZYTkkDaI377a9JSW7wE84GMeBJOOaJjn_ayvs4yGqEkKDU4fbjUkM1uVV8Xh2HWwPxwWcDYErO2n4YRslKd0AJfA=s2048?key=5vuxKC9WiJ7JMHnUZ8frzJWx" alt=""><figcaption><p>AI Layer</p></figcaption></figure>

Above architecture illustrates how various components interact in Open AgriNet (OAN) to support discovery, advisory, and service fulfillment in agriculture using Beckn protocol and AI interfaces.

### Key Components

#### 1. User Layer

* User: A farmer or agriculture stakeholder interacting via mobile or web.
* Seeker Platform: UI layer that facilitates interaction with the user. Connects to the AI layer for conversational experience.

#### 2. AI Layer (Agentic AI Component)

* Manages dialogue, session memory, tool use.
* Interfaces with BAP (via middleware) to trigger Beckn search and order calls.
* Performs retrieval-augmented generation (RAG) for crop and pest advisory.
* Translation and speech-to-text/TTS engines for multi-lingual and voice interfaces.

#### 3. Seeker-Side Beckn Adaptor (BAP)

* Client-Facing Interface: Interfaces to AI, Middleware to receive requests.
* Network-Facing Interface: Implements Beckn APIs and communicates with the Gateway and BPPs.
* Domain Schema: Defines the domain-specific ontology (agriculture-specific) being used for interaction.

#### 4. Provider-Side Beckn Adaptor (BPP)

* Client-Facing Interface: Connects to Provider Platform via middleware.
* Network-Facing Interface: Responds to Beckn protocol messages (search, on\_search, etc.).
* Domain Schema: Ensures consistency in the response format (e.g., crop advisory attributes, mandi price fields).

#### 5. Provider Platform

* Backend system integrate with actual services or information for Agriculture use case such as :
  * Mandi prices
  * Weather based advice APIs
  * Warehouse availability
  * Document-based crop advisory (PDFs, manuals)

#### 6. Network Infrastructure

* Beckn Gateway: Routes messages securely between BAPs and BPPs using signature validation and discovery.
* Beckn Registry: Maintains participant metadata, public keys, and service discovery information.

### Data Flow

1. User inputs a query (e.g., “What is the price of tomatoes in Nashik?”) via the Seeker Platform.
2. AI Layer processes the intent and either:
   1. Answers directly using its vector DB + documents.&#x20;
   2. Or calls the Beckn API (via BAP adaptor) to search for live data.
3. Seeker BAP sends a Beckn compliant search/Data to Gateway.
4. Gateway routes it to registered BPPs that match the criteria.
5. Provider BPP fetches data from its Provider Platform and responds.
6. Response is returned through the same path back to the User via the AI layer.
7. Middleware between AI Layer and BAP/BPPs handles schema translation and data validation.
8. Domain Schema acts as the common vocabulary between AI-generated queries and Beckn compliant payloads.
9. The architecture supports multi-language input/output via integration with services like Bhashini.

### System Overview

This system is designed to:

1. Enable discovery and access of agricultural services via a Beckn based network.
2. Provide a conversational AI layer for interaction in English and Indic languages.
3. Ingest structured (API-based) and unstructured (document-based) knowledge.
4. Host the user interface via web/mobile frontend with an AI-powered chatbot.
5. Enable robust monitoring and behavior analysis in production.

### Agriculture Use Cases Supported

1. Crop, pest, and pesticide advisory
2. Weather forecast (region-specific)
3. Mandi/APMC price discovery
4. Warehouse storage capacity availability

<br>
