# Table of contents

* [OpenAgriNet(OAN)](README.md)

## Overview of OAN

* [Overview of OAN](overview-of-oan/README.md)
  * [What is OAN](overview-of-oan/what-is-oan.md)
  * [Getting Started](getting-started/getting-started.md)
  * [Use Case Identification](getting-started/use-case-identification.md)
  * [Beckn Protocol - Quick Overview](overview-of-oan/beckn-protocol-quick-overview.md)
  * [Use Cases](overview-of-oan/use-cases/README.md)
    * [Scheme Discovery](overview-of-oan/use-cases/scheme-discovery.md)
    * [Weather Advisory](overview-of-oan/use-cases/weather-advisory.md)

## Implementations

* [Implementations](implementations/README.md)
  * [MahaVISTAAR](implementations/mahavistaar.md)
  * [Bharat VISTAAR](implementations/bharat-vistaar/README.md)
    * [INTRODUCTION TO VISTAAR](implementations/bharat-vistaar/introduction-to-vistaar/vistaar-introduction.md)
      * [Roles You Can Play](implementations/bharat-vistaar/introduction-to-vistaar/roles-you-can-play.md)
    * [Use Cases](implementations/bharat-vistaar/use-cases.md)
    * [Market Integrations](<implementations/bharat-vistaar/Market Integrations.md>)
    * [Prerequisites to Set Up a Provider on BharatVISTAAR](implementations/bharat-vistaar/pre-requisites-to-setup-a-provider.md)
    * [Steps to Install a BPP (Beckn Provider Platform)](implementations/bharat-vistaar/steps-to-install-a-beckn-protocol-server-bpp.md)
    * [Bharat VISTAAR Integration](implementations/bharat-vistaar/bharat-vistaar-integration/README.md)
      * [Type A](implementations/bharat-vistaar/bharat-vistaar-integration/type-a.md)
      * [Type B](implementations/bharat-vistaar/bharat-vistaar-integration/type-b/README.md)
        * [Type B.1 - Published Info Integration](implementations/bharat-vistaar/bharat-vistaar-integration/type-b/type-b.1-published-info-integration.md)
        * [Type B.2 - API Integration and Personalisation](implementations/bharat-vistaar/bharat-vistaar-integration/type-b/type-b.2-api-integration-and-personalisation.md)
        * [Type B.3 - Deep Personalisation](implementations/bharat-vistaar/bharat-vistaar-integration/type-b/type-b.3-deep-personalisation.md)
        * [Type B - Operator's Manual](implementations/bharat-vistaar/bharat-vistaar-integration/type-b/type-b-operators-manual.md)
      * [Type C](implementations/bharat-vistaar/bharat-vistaar-integration/type-c/README.md)
        * [Type C.1 - Independent State Beckn Network](implementations/bharat-vistaar/bharat-vistaar-integration/type-c/type-c.1-independent-state-beckn-network.md)
        * [Type C.2 - Bi-directional Network Interoperability with Centre](implementations/bharat-vistaar/bharat-vistaar-integration/type-c/type-c.2-bi-directional-network-interoperability-with-centre.md)
  * [Portability Across States](implementations/portability-across-states.md)
  * [Reuse across Deployments](implementations/reuse-across-deployments.md)

## Deliberations

* [Deliberations](deliberations/README.md)
  * [Type-Tiering for Bharat VISTAAR Integration](deliberations/type-tiering-for-bharat-vistaar-integration.md)
  * [State-Agnostic AI Core](deliberations/state-agnostic-ai-core.md)
  * [AI as Orchestration, Not Source of Truth](deliberations/ai-as-orchestration-not-source-of-truth.md)
  * [Open Questions](deliberations/open-questions.md)

## Journal

* [Journal](journal/README.md)
  * [OAN Kenya](journal/entry-template.md)

## Celebrations

* [Celebrations](celebrations/README.md)
  * [Critical Win Template](celebrations/critical-win-template.md)

***

* [Frameworks ](frameworks/README.md)
  * [Frameworks for OAN](frameworks/frameworks-for-oan.md)

## Technical

* [AI Layer](ai-layer/README.md)
  * [AI as an Orchestration Layer](ai-layer/ai-as-an-orchestration-layer.md)
  * [Positioning Within the OAN Architecture](ai-layer/positioning-within-the-oan-architecture.md)
  * [Agentic Architecture](ai-layer/agentic-architecture.md)
  * [High-Level AI Workflow](ai-layer/high-level-ai-workflow/README.md)
    * [Input Processing](ai-layer/high-level-ai-workflow/input-processing.md)
  * [Agent-Based and Tool-Driven Design](ai-layer/agent-based-and-tool-driven-design/README.md)
    * [Tool Invocation & Term Identification](ai-layer/agent-based-and-tool-driven-design/tool-invocation-and-term-identification.md)
    * [Tool-Driven Query Processing](ai-layer/agent-based-and-tool-driven-design/tool-driven-query-processing.md)
    * [LLM Call Sequencing](ai-layer/agent-based-and-tool-driven-design/llm-call-sequencing.md)
  * [Supporting Multilingual and Multimodal Access](ai-layer/supporting-multilingual-and-multimodal-access.md)
  * [Data Ingestion Pipeline](ai-layer/data-ingestion-pipeline/README.md)
    * [Data Sources](ai-layer/data-ingestion-pipeline/data-sources.md)
    * [Environment and Credential Setup](ai-layer/data-ingestion-pipeline/environment-and-credential-setup.md)
    * [Types of Data Ingested](ai-layer/data-ingestion-pipeline/types-of-data-ingested.md)
    * [Document Processing and OCR](ai-layer/data-ingestion-pipeline/document-processing-and-ocr.md)
    * [Chunking and Human Intervention](ai-layer/data-ingestion-pipeline/chunking-and-human-intervention.md)
    * [Video Processing](ai-layer/data-ingestion-pipeline/video-processing.md)
    * [Batch Processing & Reusable Utilities](ai-layer/data-ingestion-pipeline/batch-processing-and-reusable-utilities.md)
  * [Vector Database and Search](ai-layer/vector-database-and-search/README.md)
    * [Similarity Search](ai-layer/vector-database-and-search/similarity-search.md)
    * [Deduplication and IDs](ai-layer/vector-database-and-search/deduplication-and-ids.md)
    * [Dataset Schema](ai-layer/vector-database-and-search/dataset-schema.md)
    * [Indexing and Performance](ai-layer/vector-database-and-search/indexing-and-performance.md)
  * [Monitoring & Logs](ai-layer/monitoring-and-logs.md)
  * [Prompt-Driven Governance & Policy Control](ai-layer/prompt-driven-governance-and-policy-control.md)
  * [Reference Implementation](ai-layer/reference-implementation/README.md)
    * [Key Components in Call Flow](ai-layer/reference-implementation/key-components-in-call-flow.md)
    * [Call Flow](ai-layer/reference-implementation/call-flow.md)
    * [Architectural Characteristics](ai-layer/reference-implementation/architectural-characteristics.md)
    * [Operational Notes](ai-layer/reference-implementation/operational-notes.md)
* [Onboarding Steps](onboarding-steps/README.md)
  * [Pre-requisites](onboarding-steps/pre-requisites.md)
  * [Installation Steps - Seeker](onboarding-steps/installation-steps-seeker.md)
  * [Installation Steps - Provider](onboarding-steps/installation-steps-provider.md)
* [JWT](ui/jwt.md)
* [Keycloak](ui/keycloak.md)
* [Repository Links](repository-links.md)
* [CONTRIBUTING.md](contributing-md.md)
* [LICENSE](license.md)
* [CODE\_OF\_CONDUCT.md](code_of_conduct-md.md)
* [How to get started: Installation Guide](technical/how-to-get-started-installation-guide/README.md)
  * [ONIX: Set Up](technical/how-to-get-started-installation-guide/onix-set-up.md)
  * [BPP Onboarding](technical/how-to-get-started-installation-guide/bpp-onboarding.md)
  * [Integrating Sample Provider Service](technical/how-to-get-started-installation-guide/integrating-sample-provider-service.md)
  * [Seeker Onboarding](technical/how-to-get-started-installation-guide/seeker-onboarding.md)
  * [Beckn ONIX Provider Onboarding](technical/how-to-get-started-installation-guide/beckn-onix-provider-onboarding.md)
  * [Beckn Payload Mappers](technical/how-to-get-started-installation-guide/beckn-payload-mappers.md)
  * [Beckn ONIX Observability and Telemetry Setup](technical/how-to-get-started-installation-guide/beckn-onix-observability-and-telemetry-setup.md)
