# Beckn ONIX Observability and Telemetry Setup

## Beckn ONIX Observability and Telemetry Setup

### 1. Overview

This document explains how to enable and validate the observability stack for Beckn ONIX, including Grafana, Prometheus, Loki, Jaeger, Zipkin, and the OpenTelemetry collectors for BAP, BPP, and new BPP onboarding.

It covers:

* Starting the observability stack
* Changing the Grafana port to avoid conflict with the OAN UI
* Creating separate BAP and BPP adapter policy/config files
* Enabling telemetry for seeker and weather provider adapters
* Extending telemetry setup for a new BPP such as knowledge-advisory

***

### 2. Start the Observability Stack

Go to:

cd /home/ubuntu/beckn-onix/install/network-observability/all-in-one

#### 2.1 Update Grafana Port

Because Grafana conflicts with the OAN UI port, update the Grafana port in:

install/network-observability/all-in-one/docker-compose.yml

Replace the Grafana service configuration with the following:

grafana:\
&#x20; image: grafana/grafana:latest\
&#x20; container\_name: grafana\
&#x20; environment:\
&#x20;   \- GF\_SECURITY\_ADMIN\_USER=admin\
&#x20;   \- GF\_SECURITY\_ADMIN\_PASSWORD=admin\
&#x20;   \- GF\_USERS\_ALLOW\_SIGN\_UP=false\
&#x20;   \- GF\_SERVER\_ROOT\_URL=http://localhost:3003\
&#x20; volumes:\
&#x20;   \- grafana\_data:/var/lib/grafana\
&#x20;   \- ./grafana/provisioning:/etc/grafana/provisioning:ro\
&#x20; ports:\
&#x20;   \- "3003:3000"\
&#x20; networks:\
&#x20;   \- observability\
&#x20; restart: unless-stopped\
&#x20; depends\_on:\
&#x20;   \- prometheus\
&#x20;   \- jaeger\
&#x20;   \- zipkin\
&#x20;   \- loki

#### 2.2 Start the Observability Services

Run:

docker compose up -d otel-collector-bap otel-collector-bpp otel-collector-network loki prometheus jaeger zipkin grafana

### 3. Correct the BAP and BPP Adapter Configuration

A previous issue occurred while starting the BPP. The setup should have used separate policy/config YAML files for BAP and BPP. We are now creating the BPP-specific file with no major functional changes.

#### 3.1 Update config/local-simple.yaml

In:

config/local-simple.yaml

update the otelsetup plugin as follows:

plugins:\
&#x20; otelsetup:\
&#x20;   id: otelsetup\
&#x20;   config:\
&#x20;     serviceName: "oan-seeker-onix"\
&#x20;     serviceVersion: "1.0.0"\
&#x20;     enableMetrics: "true"\
&#x20;     environment: "development"\
&#x20;     domain: "weather-forecast:oan:kenya"\
&#x20;     enableMetrics: "true"\
&#x20;     networkMetricsGranularity: "2min"\
&#x20;     networkMetricsFrequency: "4min"\
&#x20;     enableTracing: "true"\
&#x20;     enableLogs: "true"\
&#x20;     timeInterval: "5"\
&#x20;     auditFieldsConfig: "./config/audit-fields.yaml"\
&#x20;     otlpEndpoint: "otel-collector-bap:4317"\
&#x20;     cacheTTL: "3600"

#### 3.2 Create config/local-simple-bpp.yaml

Create a duplicate of:

config/local-simple.yaml

as:

config/local-simple-bpp.yaml

Use the same contents, with the following BPP-specific settings.

**Full BPP configuration**

appName: "onix-local"\
log:\
&#x20; level: debug\
&#x20; destinations:\
&#x20;   \- type: stdout\
&#x20; contextKeys:\
&#x20;   \- transaction\_id\
&#x20;   \- message\_id\
&#x20;   \- subscriber\_id\
&#x20;   \- module\_id\
plugins:\
&#x20; otelsetup:\
&#x20;   id: otelsetup\
&#x20;   config:\
&#x20;     serviceName: "weather-bpp-onix"\
&#x20;     serviceVersion: "1.0.0"\
&#x20;     enableMetrics: "true"\
&#x20;     environment: "development"\
&#x20;     domain: "weather-forecast:oan:kenya"\
&#x20;     enableMetrics: "true"\
&#x20;     networkMetricsGranularity: "2min"\
&#x20;     networkMetricsFrequency: "4min"\
&#x20;     enableTracing: "true"\
&#x20;     enableLogs: "true"\
&#x20;     timeInterval: "5"\
&#x20;     auditFieldsConfig: "./config/audit-fields.yaml"\
&#x20;     otlpEndpoint: "otel-collector-bpp:4317"\
&#x20;     cacheTTL: "3600"\
http:\
&#x20; port: 8081\
&#x20; timeout:\
&#x20;   read: 30\
&#x20;   write: 30\
&#x20;   idle: 30\
pluginManager:\
&#x20; root: ./plugins\
modules:\
&#x20; \- name: bapTxnReceiver\
&#x20;   path: /bap/receiver/\
&#x20;   handler:\
&#x20;     type: std\
&#x20;     role: bap\
&#x20;     subscriberId: bap-network\
&#x20;     httpClientConfig:\
&#x20;       maxIdleConns: 1000\
&#x20;       maxIdleConnsPerHost: 200\
&#x20;       idleConnTimeout: 300s\
&#x20;       responseHeaderTimeout: 5s\
&#x20;     plugins:\
&#x20;       registry:\
&#x20;         id: registry\
&#x20;         config:\
&#x20;           url: http://registry:3030/subscribers\
&#x20;           retry\_max: 3\
&#x20;           retry\_wait\_min: 100ms\
&#x20;           retry\_wait\_max: 500ms\
&#x20;       keyManager:\
&#x20;         id: simplekeymanager\
&#x20;         config:\
&#x20;           networkParticipant: bap-network\
&#x20;           keyId: bap-network-key\
&#x20;           signingPrivateKey: "B0e6TLBQRxQWx0rlXD2LvdIydNFeSR93I2Z13jPIGl8="\
&#x20;           signingPublicKey: "6qOGduDX3gvFP7XWpPBnh0mlx+xvAVZRYfIsU3nZEH4="\
&#x20;           encrPrivateKey: "B0e6TLBQRxQWx0rlXD2LvdIydNFeSR93I2Z13jPIGl8="\
&#x20;           encrPublicKey: "6qOGduDX3gvFP7XWpPBnh0mlx+xvAVZRYfIsU3nZEH4="\
&#x20;       cache:\
&#x20;         id: cache\
&#x20;         config:\
&#x20;           addr: redis:6379\
&#x20;       schemaValidator:\
&#x20;         id: schemavalidator\
&#x20;         config:\
&#x20;           schemaDir: ./schemas\
&#x20;       signValidator:\
&#x20;         id: signvalidator\
&#x20;       signer:\
&#x20;         id: signer\
&#x20;       router:\
&#x20;         id: router\
&#x20;         config:\
&#x20;           routingConfig: ./config/local-simple-routing-BAPReceiver.yaml\
&#x20;       checkPolicy:\
&#x20;         id: opapolicychecker\
&#x20;         config:\
&#x20;           networkPolicyConfig: ./config/opa-network-policies.yaml\
&#x20;           refreshInterval: "5m"\
&#x20;       middleware:\
&#x20;         \- id: reqpreprocessor\
&#x20;           config:\
&#x20;             contextKeys: transaction\_id,message\_id\
&#x20;             role: bap\
&#x20;     steps:\
&#x20;       \- validateSign\
&#x20;       \- checkPolicy\
&#x20;       \- addRoute\
&#x20;       \- signAck\
&#x20;\
&#x20; \- name: bapTxnCaller\
&#x20;   path: /bap/caller/\
&#x20;   handler:\
&#x20;     type: std\
&#x20;     role: bap\
&#x20;     httpClientConfig:\
&#x20;       maxIdleConns: 1000\
&#x20;       maxIdleConnsPerHost: 200\
&#x20;       idleConnTimeout: 300s\
&#x20;       responseHeaderTimeout: 5s\
&#x20;     plugins:\
&#x20;       registry:\
&#x20;         id: registry\
&#x20;         config:\
&#x20;           url: http://registry:3030/subscribers\
&#x20;           retry\_max: 3\
&#x20;           retry\_wait\_min: 100ms\
&#x20;           retry\_wait\_max: 500ms\
&#x20;       keyManager:\
&#x20;         id: simplekeymanager\
&#x20;         config:\
&#x20;           networkParticipant: bap-network\
&#x20;           keyId: bap-network-key\
&#x20;           signingPrivateKey: "B0e6TLBQRxQWx0rlXD2LvdIydNFeSR93I2Z13jPIGl8="\
&#x20;           signingPublicKey: "6qOGduDX3gvFP7XWpPBnh0mlx+xvAVZRYfIsU3nZEH4="\
&#x20;           encrPrivateKey: "B0e6TLBQRxQWx0rlXD2LvdIydNFeSR93I2Z13jPIGl8="\
&#x20;           encrPublicKey: "6qOGduDX3gvFP7XWpPBnh0mlx+xvAVZRYfIsU3nZEH4="\
&#x20;       cache:\
&#x20;         id: cache\
&#x20;         config:\
&#x20;           addr: redis:6379\
&#x20;       router:\
&#x20;         id: router\
&#x20;         config:\
&#x20;           routingConfig: ./config/local-simple-routing-BAPCaller.yaml\
&#x20;       signer:\
&#x20;         id: signer\
&#x20;       middleware:\
&#x20;         \- id: reqpreprocessor\
&#x20;           config:\
&#x20;             contextKeys: transaction\_id,message\_id\
&#x20;             role: bap\
&#x20;     steps:\
&#x20;       \- addRoute\
&#x20;       \- sign\
&#x20;\
&#x20; \- name: bppTxnReceiver\
&#x20;   path: /bpp/receiver/\
&#x20;   handler:\
&#x20;     type: std\
&#x20;     role: bpp\
&#x20;     httpClientConfig:\
&#x20;       maxIdleConns: 1000\
&#x20;       maxIdleConnsPerHost: 200\
&#x20;       idleConnTimeout: 300s\
&#x20;       responseHeaderTimeout: 5s\
&#x20;     plugins:\
&#x20;       registry:\
&#x20;         id: registry\
&#x20;         config:\
&#x20;           url: http://registry:3030/subscribers\
&#x20;           retry\_max: 3\
&#x20;           retry\_wait\_min: 100ms\
&#x20;           retry\_wait\_max: 500ms\
&#x20;       keyManager:\
&#x20;         id: simplekeymanager\
&#x20;         config:\
&#x20;           networkParticipant: bpp-network\
&#x20;           keyId: bpp-network-key\
&#x20;           signingPrivateKey: "csDesDASs5rb18s/etNea5IW3ruNGDi5Ksedk6iNsFw="\
&#x20;           signingPublicKey: "8izVJpQUU01xpjAPJOho+bg9ViB3u5sMl/CDe1x7uXE="\
&#x20;           encrPrivateKey: "csDesDASs5rb18s/etNea5IW3ruNGDi5Ksedk6iNsFw="\
&#x20;           encrPublicKey: "8izVJpQUU01xpjAPJOho+bg9ViB3u5sMl/CDe1x7uXE="\
&#x20;       cache:\
&#x20;         id: cache\
&#x20;         config:\
&#x20;           addr: redis:6379\
&#x20;       schemaValidator:\
&#x20;         id: schemavalidator\
&#x20;         config:\
&#x20;           schemaDir: ./schemas\
&#x20;       signValidator:\
&#x20;         id: signvalidator\
&#x20;       signer:\
&#x20;         id: signer\
&#x20;       router:\
&#x20;         id: router\
&#x20;         config:\
&#x20;           routingConfig: ./config/local-simple-routing-BPPReceiver.yaml\
&#x20;       checkPolicy:\
&#x20;         id: opapolicychecker\
&#x20;         config:\
&#x20;           networkPolicyConfig: ./config/opa-network-policies.yaml\
&#x20;           refreshInterval: "5m"\
&#x20;     steps:\
&#x20;       \- validateSign\
&#x20;       \- checkPolicy\
&#x20;       \- addRoute\
&#x20;       \- signAck\
&#x20;\
&#x20; \- name: bppTxnCaller\
&#x20;   path: /bpp/caller/\
&#x20;   handler:\
&#x20;     type: std\
&#x20;     role: bpp\
&#x20;     subscriberId: bpp-network\
&#x20;     httpClientConfig:\
&#x20;       maxIdleConns: 1000\
&#x20;       maxIdleConnsPerHost: 200\
&#x20;       idleConnTimeout: 300s\
&#x20;       responseHeaderTimeout: 5s\
&#x20;     plugins:\
&#x20;       registry:\
&#x20;         id: registry\
&#x20;         config:\
&#x20;           url: http://registry:3030/subscribers\
&#x20;           retry\_max: 3\
&#x20;           retry\_wait\_min: 100ms\
&#x20;           retry\_wait\_max: 500ms\
&#x20;       keyManager:\
&#x20;         id: simplekeymanager\
&#x20;         config:\
&#x20;           subscriberId: bpp-network\
&#x20;           networkParticipant: bpp-network\
&#x20;           keyId: bpp-network-key\
&#x20;           signingPrivateKey: "csDesDASs5rb18s/etNea5IW3ruNGDi5Ksedk6iNsFw="\
&#x20;           signingPublicKey: "8izVJpQUU01xpjAPJOho+bg9ViB3u5sMl/CDe1x7uXE="\
&#x20;           encrPrivateKey: "csDesDASs5rb18s/etNea5IW3ruNGDi5Ksedk6iNsFw="\
&#x20;           encrPublicKey: "8izVJpQUU01xpjAPJOho+bg9ViB3u5sMl/CDe1x7uXE="\
&#x20;       cache:\
&#x20;         id: cache\
&#x20;         config:\
&#x20;           addr: redis:6379\
&#x20;       router:\
&#x20;         id: router\
&#x20;         config:\
&#x20;           routingConfig: ./config/local-simple-routing-BPPCaller.yaml\
&#x20;       signer:\
&#x20;         id: signer\
&#x20;     steps:\
&#x20;       \- addRoute\
&#x20;       \- sign

The only change here is:

otlpEndpoint: "otel-collector-bpp:4317"

serviceName: "weather-bpp-onix"&#x20;

### 4. Restart the BAP and BPP ONIX Adapters

These adapters are used by seeker (OAN UI) and the weather provider BPP.

Stop and remove the existing containers:

docker stop bap-onix-adapter\
docker rm bap-onix-adapter\
docker stop bpp-onix-adapter\
docker rm bpp-onix-adapter

Run the BPP adapter:

docker run -p 8081:8081 \\\
&#x20; -v $(pwd)/config:/app/config \\\
&#x20; -v $(pwd)/schemas:/app/schemas \\\
&#x20; -e CONFIG\_FILE="/app/config/local-simple-bpp.yaml" \\\
&#x20; -e OTEL\_EXPORTER\_OTLP\_INSECURE="true" \\\
&#x20; \--network=beckn\_network \\\
&#x20; -d \\\
&#x20; \--name bpp-onix-adapter \\\
&#x20; fidedocker/onix-adapter

Run the BAP adapter:

docker run -p 8080:8081 \\\
&#x20; -v $(pwd)/config:/app/config \\\
&#x20; -v $(pwd)/schemas:/app/schemas \\\
&#x20; -e CONFIG\_FILE="/app/config/local-simple.yaml" \\\
&#x20; -e OTEL\_EXPORTER\_OTLP\_INSECURE="true" \\\
&#x20; \--network=beckn\_network \\\
&#x20; -d \\\
&#x20; \--name bap-onix-adapter \\\
&#x20; fidedocker/onix-adapter

### 5. View Grafana Dashboards

Now you can either tunnel the Grafana port 3003 or create a domain for Grafana.

#### 5.1 Access Grafana

1. Open Grafana.
2. Log in using:

Username: admin\
Password: admin

3. Click Dashboards.
4. View the readily available dashboards.

#### 5.2 Drilldown Views

In Grafana:

* Go to Drilldown > Metrics to view detailed metrics.
* Go to Drilldown > Logs to view container logs.
* Go to Explore.
* Select Jaeger as the data source.
* Select a service.
* Click Run Query.

### 6. View Jaeger

You can tunnel the Jaeger port 16686 or create a domain for Jaeger.

This allows you to inspect traces generated by the ONIX adapters and the observability stack.

***

### 7. Enable Telemetry for a New BPP

The same telemetry pattern can be extended to newly onboarded BPPs such as knowledge-advisory.

#### 7.1 Create a New Collector Directory

Duplicate:

install/network-observability/all-in-one/otel-collector-bpp

and rename it to:

install/network-observability/all-in-one/otel-collector-knowledge-bpp

#### 7.2 Update the New Collector Config

Update:

install/network-observability/all-in-one/otel-collector-knowledge-bpp/config.yaml

Change:

observability: otel-collector-bpp

to:

observability: otel-collector-knowledge-bpp

and change:

service\_name: beckn-one-bpp

to:

service\_name: knowledge-bpp

***

### 8. Add the New Collector to docker-compose.yml

In:

install/network-observability/all-in-one/docker-compose.yml

add the following service:

otel-collector-knowledge-bpp:\
&#x20; image: otel/opentelemetry-collector-contrib:latest\
&#x20; container\_name: otel-collector-knowledge-bpp\
&#x20; command: \["--config=/etc/otel/config.yaml"]\
&#x20; volumes:\
&#x20;   \- ./otel-collector-knowledge-bpp/config.yaml:/etc/otel/config.yaml:ro\
&#x20; ports:\
&#x20;   \- "4331:4317"\
&#x20;   \- "4332:4318"\
&#x20;   \- "8892:8891"\
&#x20; networks:\
&#x20;   \- observability\
&#x20;   \- beckn\_network\
&#x20; restart: unless-stopped\
&#x20; depends\_on:\
&#x20;   \- otel-collector-network

Start the new collector:

docker compose up -d otel-collector-knowledge-bpp

### 9. Update the Knowledge BPP ONIX Configuration

Go to the cloned Beckn ONIX setup for the knowledge BPP and update the telemetry configuration.

#### 9.1 Update config/local-simple.yaml

Update the otelsetup block as follows:

otelsetup:\
&#x20; id: otelsetup\
&#x20; config:\
&#x20;   serviceName: "knowledge-bpp-onix"\
&#x20;   serviceVersion: "1.0.0"\
&#x20;   enableMetrics: "true"\
&#x20;   environment: "development"\
&#x20;   domain: "knowledge-advisory:oan:kenya"\
&#x20;   enableMetrics: "true"\
&#x20;   networkMetricsGranularity: "2min"\
&#x20;   networkMetricsFrequency: "4min"\
&#x20;   enableTracing: "true"\
&#x20;   enableLogs: "true"\
&#x20;   timeInterval: "5"\
&#x20;   auditFieldsConfig: "./config/audit-fields.yaml"\
&#x20;   otlpEndpoint: "otel-collector-knowledge-bpp:4317"\
&#x20;   cacheTTL: "3600"

### 10. Update install/docker-compose-adapter.yml

In:

install/docker-compose-adapter.yml

update the ONIX adapter service as follows:

onix-adapter:\
&#x20; image: fidedocker/onix-adapter\
&#x20; container\_name: bpp-k-adapter\
&#x20; platform: linux/amd64\
&#x20; networks:\
&#x20;   \- beckn\_network\
&#x20; ports:\
&#x20;   \- "8083:8081"\
&#x20; environment:\
&#x20;   CONFIG\_FILE: "/app/config/local-simple.yaml"\
&#x20;   VAULT\_ADDR: http://vault:8200\
&#x20;   VAULT\_TOKEN: root\
&#x20;   REDIS\_ADDR: redis:6379\
&#x20;   RABBITMQ\_ADDR: rabbitmq:5672\
&#x20;   RABBITMQ\_USER: admin\
&#x20;   RABBITMQ\_PASS: admin123\
&#x20;   OTEL\_EXPORTER\_OTLP\_INSECURE: "true"\
&#x20; volumes:\
&#x20;   \- ../config:/app/config\
&#x20;   \- ../schemas:/app/schemas\
&#x20; command: \["./server", "--config=/app/config/local-simple.yaml"]

The important addition is:

OTEL\_EXPORTER\_OTLP\_INSECURE: "true"

Restart the service:

docker compose -f docker-compose-adapter.yml up -d --force-recreate onix-adapter

### 11. Validation Checklist

After completing the setup, verify the following:

1. Grafana is accessible on port 3003.
2. Grafana login works with admin / admin.
3. Dashboards are visible under Dashboards.
4. Metrics are visible under Drilldown > Metrics.
5. Logs are visible under Drilldown > Logs.
6. Jaeger is reachable on port 16686.
7. BAP telemetry is pointing to otel-collector-bap:4317.
8. BPP telemetry is pointing to otel-collector-bpp:4317.
9. Knowledge BPP telemetry is pointing to otel-collector-knowledge-bpp:4317.
10. The new ONIX adapter starts successfully after recreation.
