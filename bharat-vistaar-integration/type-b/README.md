# Type B

Type B.1 — Published Info Integration

#### Goal <a href="#goal" id="goal"></a>

Citizens can access both Central and State scheme information, departmental guidelines, and agro-advisory content through a single BharatVistaar conversational interface, in regional languages.

#### Definition of Done <a href="#definition-of-done" id="definition-of-done"></a>

* State scheme documents and advisories are ingested and live on BV.
* Farmers can query a state-specific scheme by name and receive accurate, current information.
* State content is updated on BV within 30 days of any scheme notification or revision.
* Language quality of state content is validated by state-nominated language experts.

#### Action Items & Workflow <a href="#action-items--workflow" id="action-items--workflow"></a>

| Step | State Government — Action                                                                                                         | BharatVistaar Team — Action                                                                                     |
| ---- | --------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------- |
| 1    | Consolidate the list of schemes and their details in the format prescribed by BV, and share the file with the BV team for upload. | Upload the scheme information provided by the State Government into the BV content repository.                  |
| 2    | Test scheme discovery and the resulting advisory output on BV after each upload, to confirm accuracy and completeness.            | Support the State's testing cycle and log any discrepancies raised for correction.                              |
| 3    | Repeat Steps 1–2 for every new or revised scheme, so BV content stays current on a rolling basis.                                 | Continue ingesting incremental scheme updates as they are received from the State.                              |
| 4    | Test language accuracy and terminology in the regional-language output, and share structured feedback with the BV team.           | Address language and terminology issues raised by the State, and optimise regional-language output accordingly. |

{% hint style="info" %}
No API development required. The State needs to provide a PMU and a single point of contact (SPOC) to coordinate integration. The BV Team handles content ingestion and indexing end-to-end.
{% endhint %}

### Type B.2 — API Integration + Personalisation <a href="#type-b2--api-integration--personalisation" id="type-b2--api-integration--personalisation"></a>

#### Goal <a href="#goal-1" id="goal-1"></a>

BharatVistaar can provide personalised, consent-based responses to farmers — checking scheme eligibility, application status, benefit disbursement, and grievance status — by using live State system APIs.

#### Definition of Done <a href="#definition-of-done-1" id="definition-of-done-1"></a>

* A minimum of 3 State APIs are live on BV: application tracking, benefit status, and grievance redressal.
* A farmer can check their own scheme application status via BV using their registered mobile number.
* The State tech team has completed API testing and sign-off on all integrated endpoints.
* A consent mechanism is functional — the farmer explicitly authorises data use before a personalised query is served.

#### Action Items & Workflow <a href="#action-items--workflow-1" id="action-items--workflow-1"></a>

| Step | State Government — Action                                                                                                                                                                                       | BharatVistaar Team — Action                                                                                                                                                                     |
| ---- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1    | Create a Beckn Provider Service for each State service to be exposed for integration (for example, if the State has a grievance system, expose it as a Beckn Provider API and host it on State infrastructure). | Onboard and register the State's Provider service(s) into the BV Network.                                                                                                                       |
| 2    | Post onboarding, test the use case end-to-end through the State's integrated system and provide formal go-ahead / sign-off.                                                                                     | Update the AI layer in BV to enable discovery and use of the new service. Abstract the service so it is scoped only to that State's users, and not exposed to other states or general BV users. |
| 3    | Repeat Steps 1–2 for each additional State service (e.g. application tracking, benefit disbursement, grievance redressal) until at least 3 APIs are live.                                                       | Extend the AI layer configuration and consent-capture flow for each additional service as it is onboarded.                                                                                      |

{% hint style="info" %}
The State needs to provide a dedicated technical team and PMU to coordinate integration. BV provides the integration specifications and a sandbox environment for testing.
{% endhint %}

### Type B.3 - Deep Personalisation <a href="#type-b3--deep-personalisation" id="type-b3--deep-personalisation"></a>

#### Goal <a href="#goal-2" id="goal-2"></a>

BharatVistaar delivers hyper-personalised, real-time farm advisories — tailored to a specific farmer's land records, crop history, local conditions, and input usage — operating as an intelligent decision-support platform.

#### Definition of Done <a href="#definition-of-done-2" id="definition-of-done-2"></a>

* BV can generate a farm-specific advisory using land record data for a named farmer.
* Crop-specific pest or disease advisory is generated using local KVK and soil health data.
* A consent and data governance framework is formally approved by the State Agriculture Department.
* Advisory accuracy is validated by KVK scientific officers for at least 3 representative crop-district combinations.
* A data sharing agreement is signed between State and Centre covering all integrated datasets.

#### Action Items & Workflow <a href="#action-items--workflow-2" id="action-items--workflow-2"></a>

B.3 follows the same underlying Beckn Provider onboarding pattern as B.2 — the State exposes a service, BV registers and onboards it, and the AI layer is updated for state-scoped discovery. The key difference is scope: instead of only exchanging application or grievance status, the integrated service extends to sharing farmer-specific data (land records, crop history, local KVK and soil health inputs) with BV's AI layer, so it can generate hyper-personalised, farm-level advisories rather than status updates.

| Step | State Government — Action                                                                                                                                                                                                 | BharatVistaar Team — Action                                                                                                                                                              |
| ---- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1    | Create a Beckn Provider Service for each farmer-data source to be integrated (e.g. land records, crop history, soil health, KVK advisory feeds), and host it on State infrastructure — following the same pattern as B.2. | Onboard and register each State farmer-data Provider service into the BV Network, applying the same process used in B.2.                                                                 |
| 2    | Post onboarding, test the use case end-to-end and provide formal go-ahead / sign-off, with the added step of validating advisory accuracy through KVK scientific officers.                                                | Update the AI layer to consume farmer-specific data (not just status fields) and generate personalised, farm-level advisory output; continue to scope access to that State's users only. |
| 3    | Repeat Steps 1–2 for each additional farmer-data source until the full B.3 dataset scope is integrated.                                                                                                                   | Extend AI-layer logic and consent capture for each additional data source as it is onboarded.                                                                                            |
| 4    | Secure formal clearance from the State Agriculture Department for the consent and data-governance framework, and execute the State–Centre data sharing agreement.                                                         | Provide the Data Orchestration Layer such that no farmer data is persisted outside State systems; BV consumes data in-flight for advisory generation only.                               |

{% hint style="info" %}
This stage requires a dedicated State-side data officer and legal clearance from the State Agriculture Department. BV provides the Data Orchestration Layer — no data is stored outside State systems.
{% endhint %}

### Summary: Increasing Depth of Integration <a href="#summary-increasing-depth-of-integration" id="summary-increasing-depth-of-integration"></a>

Across B.1 → B.2 → B.3, the pattern of ownership stays consistent — the State exposes and validates, BV ingests and personalises — but the depth of technical integration and governance rises at each stage:

* **B.1** is content-only: no API or Beckn integration is required from the State; BV handles ingestion and indexing.
* **B.2** introduces live API integration via Beckn Provider services for status-type queries, gated by farmer consent.
* **B.3** extends the same Beckn integration pattern to farmer-specific data, requiring formal data-governance approval and a State-side data officer before go-live.

Recommended next step

Confirm PMU/SPOC nominees for each stage with the State, and sequence B.1 completion before initiating B.2 Beckn Provider development, since B.1's content pipeline and language validation loop de-risks the later stages
