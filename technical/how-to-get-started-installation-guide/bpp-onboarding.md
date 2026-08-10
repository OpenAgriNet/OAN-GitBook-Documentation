# BPP Onboarding

## BPP / Data Provider Onboarding and Flow Testing

This section helps the client configure a BPP or data provider, connect it to the network, and validate the end-to-end flow in the sandbox environment. Use the commands below as copy-paste-ready steps.

### 1. Verify running services

First, confirm that the required sandbox containers are up and running.

Use the following command:

docker ps

Expected services:

| Container   | Image                  | Ports / Notes |
| ----------- | ---------------------- | ------------- |
| redis       | redis:6.2.5-alpine     | 6379          |
| sandbox-api | fidedocker/sandbox-api | 4010 -> 4000  |
| gateway     | fidedocker/gateway     | 4000, 4030    |
| registry    | fidedocker/registry    | 3000, 3030    |

Make sure the above services are visible in docker ps before continuing.

### 2. Add the sample OPA policy

Inside the beckn-onix repository, copy the sample OPA policy into the config directory.

cp pkg/plugin/implementation/opapolicychecker/testdata/example.rego config/

This policy is used for local sandbox testing and sample flow validation.

### 3. Update the OPA network policy configuration

Update the network policy file so it points to the copied rego file.&#x20;

File: beckn-onix/config/opa-network-policies.yaml\
\
location: /app/config/example.rego

### 4. Add the network domain in Registry

Login to the Registry portal and create the network domain used for the sandbox flow.

·       Login to registry

·       Click on Beckn -> Network Domain

·       Click on the add icon

·       Enter the following values and click Done

| Field       | Value                                                                                                                            |
| ----------- | -------------------------------------------------------------------------------------------------------------------------------- |
| Name        | weather-forecast:oan:kenya                                                                                                       |
| Description | weather forecast                                                                                                                 |
| Schema URL  | https://raw.githubusercontent.com/beckn/missions/refs/heads/main/VISTAAR/layer2/knowledge-advisory\_agrinet\_vistaar\_1.1.0.yaml |

### 5. Update network participants for BPP and BAP

Update the Registry entries for both BPP and BAP using the same flow below.

In Registry, go to Admin -> Network Participants, then edit the required participant.

In Network Role, set the domain to weather-forecast:oan:kenya and change the status to Subscribed.

Update the Subscriber URL as shown below for each participant.

| Participant | Subscriber URL                            |
| ----------- | ----------------------------------------- |
| BPP         | http://bpp-onix-adapter:8081/bpp/receiver |
| BAP         | http://bap-onix-adapter:8081/bap/receiver |

Verify the participant key values match the configuration files and update the Registry if needed.

Check signingPublicKey and encrPublicKey against config/local-simple.yaml.

Also verify the Valid From date is one day prior to the current date.

### 6. Update routing configuration files

Update both routing files with the same routing rules shown below.

File: config/local-simple-routing-BAPCaller.yaml

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

<br>

These rules route Search to the Gateway and the remaining APIs to the subscribed BPP.

File: config/local-simple-routing-BPPReceiver.yaml

<br>

routingRules:

&#x20; \- domain: "weather-forecast:oan:kenya"  # Retail domain

&#x20;   version: "0.0.1"

&#x20;   targetType: "url"

&#x20;   target:

&#x20;     url: "http://sample-bpp:3000/weather/1.1.0"

&#x20;   endpoints:

&#x20;     \- search

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

Replace the target url with the URL of the provider service which should receive the request.

Replace the routing configuration in config/local-simple-routing.yaml with the following:

routingRules:

&#x20; \- domain: "weather-forecast:oan:kenya"

&#x20;   version: "0.0.1"

&#x20;   targetType: "bpp"

&#x20;   endpoints:

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

&#x20; \- domain: "weather-forecast:oan:kenya" &#x20;

&#x20;   version: "0.0.1"

&#x20;   targetType: "url"

&#x20;   target:

&#x20;     url: "http://gateway:4030/bg"

&#x20;   endpoints:

&#x20;     \- on\_search

<br>

### 7. Start the ONIX adapters for Seeker and Provider

docker run -p 8080:8081 \\\
&#x20; -v $(pwd)/config:/app/config \\\
&#x20; -v $(pwd)/schemas:/app/schemas \\\
&#x20; -e CONFIG\_FILE="/app/config/local-simple.yaml" \\\
&#x20; \--network=beckn\_network -d --name bap-onix-adapter fidedocker/onix-adapter\
\
docker run -p 8081:8081 \\\
&#x20; -v $(pwd)/config:/app/config \\\
&#x20; -v $(pwd)/schemas:/app/schemas \\\
&#x20; -e CONFIG\_FILE="/app/config/local-simple.yaml" \\\
&#x20; \--network=beckn\_network -d --name bpp-onix-adapter fidedocker/onix-adapter

### 8. Restart the Gateway

After all Registry and routing changes are complete, restart the Gateway container so the updated configuration is picked [up.cd](http://up.cd)&#x20;

docker restart gateway

### 9. Validate the request flow

Use the following curl command to trigger the Search request from the Seeker/BAP side.

curl -X POST http://localhost:8080/bap/caller/search \\\
&#x20; -H "Content-Type: application/json" \\\
&#x20; -d '{\
"context": {\
&#x20; "domain": "weather-forecast:oan:kenya",\
&#x20; "country": "IND",\
&#x20; "city": "std:080",\
&#x20; "action": "search",\
&#x20; "version": "0.0.1",\
&#x20; "bap\_id": "bap-network",\
&#x20; "bap\_uri": "http://bap-onix-adapter:8081",\
&#x20; "transaction\_id": "550e8400-e29b-41d4-a716-446655440000",\
&#x20; "message\_id": "550e8400-e29b-41d4-a716-446655440001",\
&#x20; "timestamp": "2023-06-15T09:30:00.000Z",\
&#x20; "ttl": "PT30S"\
},\
"message": {\
&#x20; "intent": {\
&#x20;   "fulfillment": {\
&#x20;     "start": {\
&#x20;       "location": {\
&#x20;         "gps": "12.9715987,77.5945627"\
&#x20;       }\
&#x20;     },\
&#x20;     "end": {\
&#x20;       "location": {\
&#x20;         "gps": "12.9715987,77.5945627"\
&#x20;       }\
&#x20;     }\
&#x20;   }\
&#x20; }\
}\
&#x20; }'

Expected response:

{"message":{"ack":{"status":"ACK"\}}}

Confirm that the request reaches the provider endpoint and is received at the provider service.

https://provider-domain/../../search

Note: To return {"message":{"ack":{"status":"ACK"\}}} once the request is received at the provider service, the documentation for how to return the actual response will be shared in another document.

### 10. Validation checklist

·       ☐ OPA sample policy copied to config/

·       ☐ opa-network-policies.yaml updated with location: /app/config/example.rego

·       ☐ Network domain weather-forecast:oan:kenya created in Registry

·       ☐ BPP participant subscribed in Registry

·       ☐ BAP participant subscribed in Registry

·       ☐ BPP subscriber URL set to http://bpp-onix-adapter:8081/bpp/receiver

·       ☐ BAP subscriber URL set to http://bap-onix-adapter:8081/bap/receiver

·       ☐ signingPublicKey and encrPublicKey verified for both participants

·       ☐ Valid From date verified as one day prior to current date for both participants

·       ☐ config/local-simple-routing.yaml updated

·       ☐ config/local-simple-routing-BAPCaller.yaml updated

·       ☐ config/local-simple-routing-BPPReceiver.yaml updated

·       ☐ Gateway restarted after routing changes

·       ☐ ONIX Adapter containers restarted after configuration changes

·       ☐ Search curl returned {"message":{"ack":{"status":"ACK"\}}}

·       ☐ Provider received the request at http://sample-bpp:3000/weather/1.1.0/search

·       ☐ Provider service received the exact request body sent by the BAP

·       ☐ End-to-end request flow verified successfully

<br>
