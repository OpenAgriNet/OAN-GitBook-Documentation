# Beckn ONIX Provider Onboarding

## Beckn ONIX Provider Onboarding Guide

### Scenario

If you're onboarding a new provider that belongs to a new domain, first create the domain in Registry. Otherwise, reuse an existing domain and continue with provider onboarding.

### 1. Add Network Domain in Registry

&#x20;Login to the Registry portal.\
Navigate to Beckn → Network Domain.\
Click the Add icon.\
Enter the following values:

| Field       | Value                                                                                                                            |
| ----------- | -------------------------------------------------------------------------------------------------------------------------------- |
| Name        | knowledge-advisory:oan:kenya                                                                                                     |
| Description | Knowledge Advisory                                                                                                               |
| Schema URL  | https://raw.githubusercontent.com/beckn/missions/refs/heads/main/VISTAAR/layer2/knowledge-advisory\_agrinet\_vistaar\_1.1.0.yaml |

### 2. Install a New ONIX BPP Adapter

Even if you already have a beckn-onix repository, perform a fresh clone in a new directory.

cd \<new-direcoty>\
git clone https://github.com/Beckn-One/beckn-onix\
cd beckn-onix/install\
wget -qO- https://gist.githubusercontent.com/tejash-jl/a3ff66e72679042d2403f0e3b6e99b99/raw/bpp-onboarder.sh | bash

Provide the prompted values exactly as below (replace placeholders where applicable):

| Prompt               | Value / Notes                                                                                                                                |
| -------------------- | -------------------------------------------------------------------------------------------------------------------------------------------- |
| BPP subscriber id    | bpp-knowledge (must be unique)                                                                                                               |
| Domain               | knowledge-advisory:oan:kenya (new or existing domain)                                                                                        |
| Webhook target URL   | http://weather-provider:3000/knowledge (sample; replace with your provider base URL if different)                                            |
| BPP Subscriber URL   | http://bpp-knowledge-adapter:8081/bpp/receiver or http://\<subscriber-id>-adapter:8081/bpp/receiver (DNS can also be used and changed later) |
| Registry URL         | http://localhost:3030/subscribers (or registry DNS)                                                                                          |
| Registry username    | root                                                                                                                                         |
| Registry password    | \*\*\*\*                                                                                                                                     |
| Install ONIX Adapter | Y                                                                                                                                            |

Expected output:

✔ Container bpp-knowledge-adapter Started\
ONIX Adapter installation successful\
Process complete. Thank you for using Beckn-ONIX!

### 3. Update Registry Network Participant

Go to Admin → Network Participants and edit the above created participant.\
• Set Network Role domain to the respective domain.\
• Change Status to Subscribed.\
• Verify signingPublicKey and encrPublicKey match config/local-simple.yaml.\
• Ensure Valid From date is one day prior to the current date.

### 4. Restart Services

docker restart \<subscriber-id>-adapter\
docker restart gateway\
docker ps | grep adapter

824df910e092   fidedocker/onix-adapter   "./server --config=/…"   Up   bpp-knowledge-adapter

Verify the adapter container is running.

### 5. Update BAP Routing (Only for New Domains)

In the previously cloned beckn-onix, modify the following

Update both configuration files by adding routing rules for the new domain alongside the existing weather domain.

Files:

• config/local-simple-routing-BAPCaller.yaml

routingRules:

&#x20; \- domain: "weather-forecast:oan:kenya"

&#x20;   version: "0.0.1"

&#x20;   targetType: "bpp"

&#x20;   endpoints:

&#x20;     \- select

&#x20;     \- init

&#x20;     \- confirm

&#x20;     \- status

&#x20;     \- track

&#x20;     \- cancel

&#x20;     \- update

&#x20;     \- rating

&#x20;     \- support

<br>

&#x20; \- domain: "weather-forecast:oan:kenya" &#x20;

&#x20;   version: "0.0.1"

&#x20;   targetType: "url"

&#x20;   target:

&#x20;     url: "http://gateway:4030/bg"

&#x20;   endpoints:

&#x20;     \- search

&#x20; \- domain: "knowledge-advisory:oan:kenya"

&#x20;   version: "0.0.1"

&#x20;   targetType: "bpp"

&#x20;   endpoints:

&#x20;     \- select

&#x20;     \- init

&#x20;     \- confirm

&#x20;     \- status

&#x20;     \- track

&#x20;     \- cancel

&#x20;     \- update

&#x20;     \- rating

&#x20;     \- support

<br>

&#x20; \- domain: "knowledge-advisory:oan:kenya" &#x20;

&#x20;   version: "0.0.1"

&#x20;   targetType: "url"

&#x20;   target:

&#x20;     url: "http://gateway:4030/bg"

&#x20;   endpoints:

&#x20;     \- search

<br>

• config/local-simple-routing-BAPReceiver.yaml

routingRules:

&#x20; \- domain: "weather-forecast:oan:kenya"

&#x20;   version: "0.0.1"

&#x20;   targetType: "url"

&#x20;   target:

&#x20;     url: "http://sva\_app:8000/api/bap-webhook"

&#x20;   endpoints:

&#x20;     \- on\_search

&#x20;     \- on\_select

&#x20;     \- on\_init

&#x20;     \- on\_confirm

&#x20;     \- on\_status

&#x20;     \- on\_track

&#x20;     \- on\_cancel

&#x20;     \- on\_update

&#x20;     \- on\_rating

&#x20;     \- on\_support

<br>

&#x20; \- domain: "knowledge-advisory:oan:kenya"

&#x20;   version: "0.0.1"

&#x20;   targetType: "url"

&#x20;   target:

&#x20;     url: "http://sva\_app:8000/api/bap-webhook"

&#x20;   endpoints:

&#x20;     \- on\_search

&#x20;     \- on\_select

&#x20;     \- on\_init

&#x20;     \- on\_confirm

&#x20;     \- on\_status

&#x20;     \- on\_track

&#x20;     \- on\_cancel

&#x20;     \- on\_update

&#x20;     \- on\_rating

&#x20;     \- on\_support

<br>

<br>

Preserve all existing weather-forecast rules and add identical rules for knowledge-advisory:oan:kenya.\
<br>

docker restart bap-onix-adapter

### 6. Update bharat-oan-api

We have updated the tools to enable knowledge advisory

&#x20;cd bharat-oan-api\
git pull\
docker compose up -d --force-recreate --build

### 7. Update Sample Provider

&#x20;cd mock-bpp-provider/\
git pull\
docker build -t sample-weather-provider .\
docker stop weather-provider\
docker rm weather-provider

Original example:

docker run -d --name weather-provider --network beckn\_network -p 3002:3000 -e CALLBACK\_URL=http://bpp-onix-adapter:8081/bpp/caller/on\_search -e BPP\_ID=bpp-network -e BPP\_URI=http://bpp-onix-adapter:8081/bpp/caller -e KNOWLEDGE\_CALLBACK\_URL=http://bpp-knowledge-adapter:8081/bpp/caller/on\_search -e KNOWLEDGE\_BPP\_ID=bpp-knowledge -e KNOWLEDGE\_BPP\_URI=http://bpp-knowledge-adapter:8081/bpp/receiver -e KNOWLEDGE\_API\_KEY=klm\_bad\*\*\*\* sample-weather-provider

Generic version (replace subscriber-id and API key):

docker run -d --name weather-provider --network beckn\_network -p 3002:3000 -e CALLBACK\_URL=http://bpp-onix-adapter:8081/bpp/caller/on\_search -e BPP\_ID=bpp-network -e BPP\_URI=http://bpp-onix-adapter:8081/bpp/caller -e KNOWLEDGE\_CALLBACK\_URL=http://bpp-knwlg-adapter:8081/bpp/caller/on\_search -e KNOWLEDGE\_BPP\_ID=\<subscriber-id> -e KNOWLEDGE\_BPP\_URI=http://\<subscriber-id>-adapter:8081/bpp/receiver -e KNOWLEDGE\_API\_KEY=klm\_bad\*\*\*\* sample-weather-provider

Note: The sample provider has been updated to invoke Knowledge APIs. Update KNOWLEDGE\_API\_KEY, KNOWLEDGE\_BPP\_ID, and KNOWLEDGE\_BPP\_URI according to the subscriber that was created.

### 7. Testing

Open OAN UI

And search for “What fertilizer should I use for maize?”

<br>
