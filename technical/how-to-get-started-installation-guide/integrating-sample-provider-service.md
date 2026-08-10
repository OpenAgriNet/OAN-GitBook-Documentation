# Integrating Sample Provider Service

## Mock Provider Setup and ONIX Routing Configuration

### Overview

This guide explains how to clone and run the mock weather BPP provider, update the ONIX adapter routing configuration, restart the adapters, and verify the search flow using a sample curl request.

### 1) Clone the repository

git clone https://github.com/kanakkshk-deloitte/mock-bpp-provider

cd mock-bpp-provider

<br>

### 2) Build the Docker image

docker build -t sample-weather-provider .

<br>

### 3) Run the provider container

docker run -d --name weather-provider \\

&#x20; \--network beckn\_network \\

&#x20; -p 3002:3000 \\

&#x20; -e CALLBACK\_URL=http://bpp-onix-adapter:8081/bpp/caller/on\_search \\

&#x20; -e BPP\_ID=bpp-network \\

&#x20; -e BPP\_URI=http://bpp-onix-adapter:8081/bpp/caller \\

&#x20; sample-weather-provider

<br>

### 4) Check provider logs

docker logs -f weather-provider

<br>

### 5) In Beckn Onix update the routing configuration

Create the below files if not present

#### config/local-simple-routing-BAPReceiver.yaml

routingRules:

&#x20; \- domain: "weather-forecast:oan:kenya"

&#x20;   version: "0.0.1"

&#x20;   targetType: "url"

&#x20;   target:

&#x20;     url: "http://seeker-service:4030"

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

#### config/local-simple-routing-BPPCaller.yaml

routingRules:

&#x20; \- domain: "weather-forecast:oan:kenya"

&#x20;   version: "0.0.1"

&#x20;   targetType: "bap"

&#x20;   endpoints:

&#x20;     \- on\_search

&#x20;     \- on\_select

&#x20;     \- on\_init

&#x20;     \- on\_confirm

&#x20;     \- on\_status

&#x20;     \- on\_track

&#x20;     \- on\_cancel

&#x20;        \- on\_update

&#x20;     \- on\_rating

&#x20;     \- on\_support

<br>

#### config/local-simple-routing-BPPReceiver.yaml

Point the route to the weather provider service:

target:

&#x20; url: "http://weather-provider:3000/mobility"

<br>

If required, point this to the service or domain where the search endpoint is implemented, following the repository specifications.

### 6) Update config/local-simple.yaml

Make sure the following values are added or updated in your local file:

subscriberId: bap-network

<br>

config:

&#x20; routingConfig: ./config/local-simple-routing-BAPReceiver.yaml

<br>

subscriberId: bpp-network

<br>

config:

&#x20; routingConfig: ./config/local-simple-routing-BPPCaller.yaml

<br>

A sample full configuration is shown below.

appName: "onix-local"

log:

&#x20; level: debug

&#x20; destinations:

&#x20;   \- type: stdout

&#x20; contextKeys:

&#x20;   \- transaction\_id

&#x20;   \- message\_id

&#x20;   \- subscriber\_id

&#x20;   \- module\_id

plugins:

&#x20; otelsetup:

&#x20;   id: otelsetup

&#x20;   config:

&#x20;     serviceName: "beckn-onix"

&#x20;     serviceVersion: "1.0.0"

&#x20;     enableMetrics: "true"

&#x20;     environment: "development"

http:

&#x20; port: 8081

&#x20; timeout:

&#x20;   read: 30

&#x20;   write: 30

&#x20;   idle: 30

pluginManager:

&#x20; root: ./plugins

modules:

&#x20; \- name: bapTxnReceiver

&#x20;   path: /bap/receiver/

&#x20;   handler:

&#x20;     type: std

&#x20;     role: bap

&#x20;     subscriberId: bap-network

&#x20;     httpClientConfig:

&#x20;       maxIdleConns: 1000

&#x20;       maxIdleConnsPerHost: 200

&#x20;       idleConnTimeout: 300s

&#x20;       responseHeaderTimeout: 5s

&#x20;     plugins:

&#x20;       registry:

&#x20;         id: registry

&#x20;         config:

&#x20;           url: http://registry:3030/subscribers

&#x20;           retry\_max: 3

&#x20;           retry\_wait\_min: 100ms

&#x20;           retry\_wait\_max: 500ms

&#x20;       keyManager:

&#x20;         id: simplekeymanager

&#x20;         config:

&#x20;           networkParticipant: bap-network

&#x20;           keyId: bap-network-key

&#x20;           signingPrivateKey: "B0e6TLBQRxQWx0rlXD2LvdIydNFeSR93I2Z13jPIGl8="

&#x20;           signingPublicKey: "6qOGduDX3gvFP7XWpPBnh0mlx+xvAVZRYfIsU3nZEH4="

&#x20;           encrPrivateKey: "B0e6TLBQRxQWx0rlXD2LvdIydNFeSR93I2Z13jPIGl8="

&#x20;           encrPublicKey: "6qOGduDX3gvFP7XWpPBnh0mlx+xvAVZRYfIsU3nZEH4="

&#x20;       cache:

&#x20;         id: cache

&#x20;         config:

&#x20;           addr: redis:6379

&#x20;       schemaValidator:

&#x20;         id: schemavalidator

&#x20;         config:

&#x20;           schemaDir: ./schemas

&#x20;       signValidator:

&#x20;         id: signvalidator

&#x20;       signer:

&#x20;         id: signer

&#x20;       router:

&#x20;         id: router

&#x20;         config:

&#x20;           routingConfig: ./config/local-simple-routing-BAPReceiver.yaml

&#x20;       checkPolicy:

&#x20;         id: opapolicychecker

&#x20;         config:

&#x20;           networkPolicyConfig: ./config/opa-network-policies.yaml

&#x20;           refreshInterval: "5m"

&#x20;       middleware:

&#x20;         \- id: reqpreprocessor

&#x20;           config:

&#x20;             contextKeys: transaction\_id,message\_id

&#x20;             role: bap

&#x20;     steps:

&#x20;       \- validateSign

&#x20;       \- checkPolicy

&#x20;       \- addRoute

&#x20;       \- signAck

<br>

&#x20; \- name: bapTxnCaller

&#x20;   path: /bap/caller/

&#x20;   handler:

&#x20;     type: std

&#x20;     role: bap

&#x20;     httpClientConfig:

&#x20;       maxIdleConns: 1000

&#x20;       maxIdleConnsPerHost: 200

&#x20;       idleConnTimeout: 300s

&#x20;       responseHeaderTimeout: 5s

&#x20;     plugins:

&#x20;       registry:

&#x20;         id: registry

&#x20;         config:

&#x20;           url: http://registry:3030/subscribers

&#x20;           retry\_max: 3

&#x20;           retry\_wait\_min: 100ms

&#x20;           retry\_wait\_max: 500ms

&#x20;       keyManager:

&#x20;         id: simplekeymanager

&#x20;         config:

&#x20;           networkParticipant: bap-network

&#x20;           keyId: bap-network-key

&#x20;           signingPrivateKey: "B0e6TLBQRxQWx0rlXD2LvdIydNFeSR93I2Z13jPIGl8="

&#x20;           signingPublicKey: "6qOGduDX3gvFP7XWpPBnh0mlx+xvAVZRYfIsU3nZEH4="

&#x20;           encrPrivateKey: "B0e6TLBQRxQWx0rlXD2LvdIydNFeSR93I2Z13jPIGl8="

&#x20;           encrPublicKey: "6qOGduDX3gvFP7XWpPBnh0mlx+xvAVZRYfIsU3nZEH4="

&#x20;       cache:

&#x20;         id: cache

&#x20;         config:

&#x20;           addr: redis:6379

&#x20;       router:

&#x20;         id: router

&#x20;         config:

&#x20;           routingConfig: ./config/local-simple-routing-BAPCaller.yaml

&#x20;       signer:

&#x20;         id: signer

&#x20;       middleware:

&#x20;         \- id: reqpreprocessor

&#x20;           config:

&#x20;             contextKeys: transaction\_id,message\_id

&#x20;             role: bap

&#x20;     steps:

&#x20;       \- addRoute

&#x20;       \- sign

<br>

&#x20; \- name: bppTxnReceiver

&#x20;   path: /bpp/receiver/

&#x20;   handler:

&#x20;     type: std

&#x20;     role: bpp

&#x20;     httpClientConfig:

&#x20;       maxIdleConns: 1000

&#x20;       maxIdleConnsPerHost: 200

&#x20;       idleConnTimeout: 300s

&#x20;       responseHeaderTimeout: 5s

&#x20;     plugins:

&#x20;       registry:

&#x20;         id: registry

&#x20;         config:

&#x20;           url: http://registry:3030/subscribers

&#x20;           retry\_max: 3

&#x20;           retry\_wait\_min: 100ms

&#x20;           retry\_wait\_max: 500ms

&#x20;       keyManager:

&#x20;         id: simplekeymanager

&#x20;         config:

&#x20;           networkParticipant: bpp-network

&#x20;           keyId: bpp-network-key

&#x20;           signingPrivateKey: "csDesDASs5rb18s/etNea5IW3ruNGDi5Ksedk6iNsFw="

&#x20;           signingPublicKey: "8izVJpQUU01xpjAPJOho+bg9ViB3u5sMl/CDe1x7uXE="

&#x20;           encrPrivateKey: "csDesDASs5rb18s/etNea5IW3ruNGDi5Ksedk6iNsFw="

&#x20;           encrPublicKey: "8izVJpQUU01xpjAPJOho+bg9ViB3u5sMl/CDe1x7uXE="

&#x20;       cache:

&#x20;         id: cache

&#x20;         config:

&#x20;           addr: redis:6379

&#x20;       schemaValidator:

&#x20;         id: schemavalidator

&#x20;         config:

&#x20;           schemaDir: ./schemas

&#x20;       signValidator:

&#x20;         id: signvalidator

&#x20;       signer:

&#x20;         id: signer

&#x20;       router:

&#x20;         id: router

&#x20;         config:

&#x20;           routingConfig: ./config/local-simple-routing-BPPReceiver.yaml

&#x20;       checkPolicy:

&#x20;         id: opapolicychecker

&#x20;         config:

&#x20;           networkPolicyConfig: ./config/opa-network-policies.yaml

&#x20;           refreshInterval: "5m"

&#x20;     steps:

&#x20;       \- validateSign

&#x20;       \- checkPolicy

&#x20;       \- addRoute

&#x20;       \- signAck

<br>

&#x20; \- name: bppTxnCaller

&#x20;   path: /bpp/caller/

&#x20;   handler:

&#x20;     type: std

&#x20;     role: bpp

&#x20;     subscriberId: bpp-network

&#x20;     httpClientConfig:

&#x20;       maxIdleConns: 1000

&#x20;       maxIdleConnsPerHost: 200

&#x20;       idleConnTimeout: 300s

&#x20;       responseHeaderTimeout: 5s

&#x20;     plugins:

&#x20;       registry:

&#x20;         id: registry

&#x20;         config:

&#x20;           url: http://registry:3030/subscribers

&#x20;           retry\_max: 3

&#x20;           retry\_wait\_min: 100ms

&#x20;           retry\_wait\_max: 500ms

&#x20;       keyManager:

&#x20;         id: simplekeymanager

&#x20;         config:

&#x20;           networkParticipant: bpp-network

&#x20;           keyId: bpp-network-key

&#x20;           signingPrivateKey: "csDesDASs5rb18s/etNea5IW3ruNGDi5Ksedk6iNsFw="

&#x20;           signingPublicKey: "8izVJpQUU01xpjAPJOho+bg9ViB3u5sMl/CDe1x7uXE="

&#x20;           encrPrivateKey: "csDesDASs5rb18s/etNea5IW3ruNGDi5Ksedk6iNsFw="

&#x20;           encrPublicKey: "8izVJpQUU01xpjAPJOho+bg9ViB3u5sMl/CDe1x7uXE="

&#x20;       cache:

&#x20;         id: cache

&#x20;         config:

&#x20;           addr: redis:6379

&#x20;       router:

&#x20;         id: router

&#x20;         config:

&#x20;           routingConfig: ./config/local-simple-routing-BPPCaller.yaml

&#x20;       signer:

&#x20;         id: signer

&#x20;     steps:

&#x20;       \- addRoute

&#x20;       \- sign

<br>

### 7) Restart the adapters

docker restart bap-onix-adapter bpp-onix-adapter

<br>

### 8) Test the search flow

Run the following request:

curl -X POST http://localhost:8080/bap/caller/search \\

&#x20; -H "Content-Type: application/json" \\

&#x20; -d '{

&#x20;   "context": {

&#x20;     "domain": "weather-forecast:oan:kenya",

&#x20;     "country": "IND",

&#x20;     "city": "std:080",

&#x20;     "action": "search",

&#x20;     "version": "0.0.1",

&#x20;     "bap\_id": "bap-network",

&#x20;     "bap\_uri": "http://bap-onix-adapter:8081/bap/receiver",

&#x20;     "transaction\_id": "550e8400-e29b-41d4-a716-446655440000",

&#x20;     "message\_id": "550e8400-e29b-41d4-a716-446655440001",

&#x20;     "timestamp": "2023-06-15T09:30:00.000Z",

&#x20;     "ttl": "PT30S"

&#x20;   },

&#x20;   "message": {

&#x20;     "intent": {

&#x20;       "fulfillment": {

&#x20;         "stops": \[{

&#x20;           "location": {

&#x20;             "gps": "12.9715987,77.5945627"

&#x20;           }

&#x20;         }]

&#x20;       }

&#x20;     }

&#x20;   }

&#x20; }'

<br>

### 9) Check adapter logs

docker logs -f bap-onix-adapter

<br>

### 10) Expected log output

Validate that you can see output similar to the following:

{"level":"info","transaction\_id":"550e8400-e29b-41d4-a716-446655440000","message\_id":"","subscriber\_id":"bap-network","module\_id":"bapTxnReceiver","time":"2026-07-29T10:31:35Z","message":"Forwarding request to URL: http://seeker-service:4030/on\_search"}

<br>

{"level":"info","transaction\_id":"550e8400-e29b-41d4-a716-446655440000","message\_id":"msg-1785321095755","subscriber\_id":"bap-network","module\_id":"bapTxnReceiver","method":"POST","url":"http://seeker-service:4030/on\_search","body":"{\\"context\\":{\\"domain\\":\\"weather-forecast:oan:kenya\\",\\"country\\":\\"IND\\",\\"city\\":\\"std:080\\",\\"action\\":\\"on\_search\\",\\"version\\":\\"0.0.1\\",\\"bap\_id\\":\\"bap-network\\",\\"bap\_uri\\":\\"http://bap-onix-adapter:8081/bap/receiver\\",\\"transaction\_id\\":\\"550e8400-e29b-41d4-a716-446655440000\\",\\"message\_id\\":\\"msg-1785321095755\\",\\"timestamp\\":\\"2026-07-29T10:31:35.755Z\\",\\"ttl\\":\\"PT30S\\",\\"bpp\_id\\":\\"bpp-network\\",\\"bpp\_uri\\":\\"http://bpp-onix-adapter:8081/bpp/caller\\"},\\"message\\":{\\"catalog\\":{\\"descriptor\\":{\\"name\\":\\"Mock Weather Advisory Catalog\\"},\\"providers\\":\[{\\"id\\":\\"provider-weather-1\\",\\"descriptor\\":{\\"name\\":\\"Mock Met Service\\",\\"short\_desc\\":\\"Live temperature: 27.2°C\\"},\\"fulfillments\\":\[{\\"id\\":\\"weather-location-1\\",\\"stops\\":\[{\\"location\\":{\\"gps\\":\\"12.9715987,77.5945627\\",\\"lat\\":\\"12.9715987\\",\\"lon\\":\\"77.5945627\\"\}}]}],\\"items\\":\[{\\"id\\":\\"advisory-rain-001\\",\\"descriptor\\":{\\"name\\":\\"Heavy Rain Advisory\\",\\"short\_desc\\":\\"Orange alert for heavy rainfall in next 6 hours.\\",\\"long\_desc\\":\\"Live temperature: 27.2°C\\"\}},{\\"id\\":\\"advisory-heat-002\\",\\"descriptor\\":{\\"name\\":\\"Heatwave Advisory\\",\\"short\_desc\\":\\"Avoid outdoor exposure from 12:00 to 16:00.\\",\\"long\_desc\\":\\"Live temperature: 27.2°C\\"\}}]\}}]\}}}","remoteAddr":"172.18.0.7:52612","time":"2026-07-29T10:31:35Z","message":"HTTP Request"}

\
<br>

### Checklist

<br>

| Status | Task                                                | Verification                                                                                                            |
| ------ | --------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------- |
| ☐      | Clone the Mock BPP Provider repository              | Repository cloned successfully                                                                                          |
| ☐      | Build the Docker image                              | sample-weather-provider image created                                                                                   |
| ☐      | Run the Weather Provider container                  | Container is running on beckn\_network                                                                                  |
| ☐      | Verify Weather Provider logs                        | docker logs -f weather-provider shows no startup errors                                                                 |
| ☐      | Update config/local-simple-routing-BAPReceiver.yaml | Domain weather-forecast:oan:kenya added with target http://seeker-service:4030                                          |
| ☐      | Update config/local-simple-routing-BPPCaller.yaml   | Routing rule for weather-forecast:oan:kenya configured                                                                  |
| ☐      | Update config/local-simple-routing-BPPReceiver.yaml | Target URL set to http://weather-provider:3000/mobility (or appropriate service endpoint)                               |
| ☐      | Update config/local-simple.yaml                     | subscriberId: bap-network added for bapTxnReceiver                                                                      |
| ☐      | Update config/local-simple.yaml                     | routingConfig: ./config/local-simple-routing-BAPReceiver.yaml configured                                                |
| ☐      | Update config/local-simple.yaml                     | subscriberId: bpp-network added for bppTxnCaller                                                                        |
| ☐      | Update config/local-simple.yaml                     | routingConfig: ./config/local-simple-routing-BPPCaller.yaml configured                                                  |
| ☐      | Restart ONIX adapters                               | docker restart bap-onix-adapter bpp-onix-adapter completed successfully                                                 |
| ☐      | Verify ONIX adapters are running                    | docker ps shows both adapters in Up state                                                                               |
| ☐      | Execute Search API                                  | curl request returns HTTP 200/202                                                                                       |
| ☐      | Check BAP ONIX logs                                 | docker logs -f bap-onix-adapter                                                                                         |
| ☐      | Verify request forwarding                           | Log contains "Forwarding request to URL:[ http://seeker-service:4030/on\_search](http://seeker-service:4030/on_search)" |
| ☐      | Verify on\_search request                           | HTTP POST sent to http://seeker-service:4030/on\_search                                                                 |
| ☐      | Verify catalog response                             | Response contains Mock Weather Advisory Catalog                                                                         |

<br>
