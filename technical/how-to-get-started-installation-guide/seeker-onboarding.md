# Seeker Onboarding

## OAN Seeker Onboarding

### 1) Clone and configure bharat-oan-api

git clone https://github.com/kanakkshk-deloitte/bharat-oan-api

cd bharat-oan-api

cp .env.example .env

<br>

Update BEDROCK\_API\_KEY in the .env file.

<br>

For OpenAI, update the below keys in .env

LLM\_AGRINET\_MODEL\_NAME=gpt-4.1-mini

LLM\_MODERATION\_MODEL\_NAME=gpt-4.1-mini

LLM\_PROVIDER=openai

OPENAI\_API\_KEY=\*\*

Generate JWT keys:

openssl genpkey -algorithm RSA -out jwt\_private\_key.pem -pkeyopt rsa\_keygen\_bits:2048

openssl rsa -pubout -in jwt\_private\_key.pem -out jwt\_public\_key.pem

<br>

Start the services:

docker compose up -d

<br>

If you modify any code, use the below command to rebuild and restart the docker container\
<br>

cd bharat-oan-api

docker compose up -d --force-recreate --build

\
<br>

***

### 2) Clone and start OAN UI

Repository:

git clone [https://github.com/kanakkshk-deloitte/OAN-UI](https://github.com/kanakkshk-deloitte/OAN-UI)

<br>

Update the publicKeyPEM value in src/contexts/AuthContext.tsx with the contents of jwt\_public\_key.pem generated in the previous step.&#x20;

<br>

&#x20;docker build -t oan-ui -f Dockerfile.local .

&#x20;docker run -d --network beckn\_network -p 3004:3004 --name oan-ui oan-ui

Open the UI in the browser:

[http://localhost:3004](http://localhost:3004)

<br>

Configure an Nginx reverse proxy to make the above UI accessible via a domain name.&#x20;

\
<br>

***

### 3) Update and run mock-bpp-provider

This repository was cloned earlier, pull the latest changes first:

cd mock-bpp-provider

git pull

<br>

Build the image:

docker build -t sample-weather-provider .

<br>

Restart the container:

docker stop weather-provider

docker rm weather-provider

<br>

Run the provider again:

docker run -d --name weather-provider \\

&#x20; \--network beckn\_network \\

&#x20; -p 3002:3000 \\

&#x20; -e CALLBACK\_URL=http://bpp-onix-adapter:8081/bpp/caller/on\_search \\

&#x20; -e BPP\_ID=bpp-network \\

&#x20; -e BPP\_URI=http://bpp-onix-adapter:8081/bpp/caller \\

&#x20; sample-weather-provider

<br>

***

### 4) Update Beckn ONIX routing

Edit:

beckn-onix/config/local-simple-routing-BAPReceiver.yaml

<br>

Set the target URL to:

target:

&#x20; url: http://sva\_app:8000/api/bap-webhook

<br>

And restart the bap onix adapter:

docker restart bap-onix-adapter

***

### 5) Test from the UI

Use this query in the UI:

\<below query is to test within kenya>

what is the current weather at latitude:1.2921 longitude:36.8219 &#x20;

<br>

\<below query to test within india>

what is the weather at latitude 12.9716° N and longitude 77.5946° E ?&#x20;

<br>

### 6) Checklist

<br>

| Status | Task                                                           | Verification                                                     |
| ------ | -------------------------------------------------------------- | ---------------------------------------------------------------- |
| ☐      | Clone the bharat-oan-api repository                            | Repository cloned successfully                                   |
| ☐      | Create .env                                                    | .env.example copied to .env                                      |
| ☐      | Configure BEDROCK\_API\_KEY                                    | Valid API key added                                              |
| ☐      | Generate JWT private key                                       | jwt\_private\_key.pem created                                    |
| ☐      | Generate JWT public key                                        | jwt\_public\_key.pem created                                     |
| ☐      | Start backend services                                         | docker compose up -d completed successfully                      |
| ☐      | Verify backend containers                                      | docker ps shows all services running                             |
| ☐      | Clone the OAN-UI repository                                    | Repository cloned successfully                                   |
| ☐      | Create .env.local                                              | .env.example copied to .env.local                                |
| ☐      | Install UI dependencies                                        | npm install completed successfully                               |
| ☐      | Start the UI                                                   | npm run dev executed successfully                                |
| ☐      | Verify UI accessibility                                        | http://localhost:3004 opens successfully                         |
| ☐      | Pull latest mock-bpp-provider changes                          | git pull completed successfully                                  |
| ☐      | Build Weather Provider image                                   | sample-weather-provider image created                            |
| ☐      | Stop existing Weather Provider                                 | Existing container stopped                                       |
| ☐      | Remove existing Weather Provider                               | Existing container removed                                       |
| ☐      | Start new Weather Provider container                           | Container starts successfully on beckn\_network                  |
| ☐      | Verify Weather Provider logs                                   | No startup errors in docker logs -f weather-provider             |
| ☐      | Update beckn-onix/config/local-simple-routing-BAPReceiver.yaml | Target URL updated to http://sva\_app:8000/api/bap-webhook       |
| ☐      | Restart ONIX adapter (if required)                             | Updated routing configuration loaded                             |
| ☐      | Verify ONIX adapter logs                                       | No routing or configuration errors                               |
| ☐      | Open the OAN UI                                                | UI loads without errors                                          |
| ☐      | Execute test query                                             | what is the current weather at latitude:1.2921 longitude:36.8219 |
| ☐      | Verify AI response                                             | Weather response is returned successfully                        |

<br>
