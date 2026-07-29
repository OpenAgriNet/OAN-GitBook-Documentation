# Type B.3 - Deep Personalisation

## Goal

BharatVistaar delivers hyper-personalised, real-time farm advisories — tailored to a specific farmer's land records, crop history, local conditions, and input usage — operating as an intelligent decision-support platform.

## Definition of Done

* BV can generate a farm-specific advisory using land record data for a named farmer.
* Crop-specific pest or disease advisory is generated using local KVK and soil health data.
* A consent and data governance framework is formally approved by the State Agriculture Department.
* Advisory accuracy is validated by KVK scientific officers for at least 3 representative crop-district combinations.
* A data sharing agreement is signed between State and Centre covering all integrated datasets.

## Action Items & Workflow

B.3 follows the same underlying Beckn Provider onboarding pattern as B.2 — the State exposes a service, BV registers and onboards it, and the AI layer is updated for state-scoped discovery. The key difference is scope: instead of only exchanging application or grievance status, the integrated service extends to sharing farmer-specific data (land records, crop history, local KVK and soil health inputs) with BV's AI layer, so it can generate hyper-personalised, farm-level advisories rather than status updates.

| Step | State Government — Action                                                                                                                                                                                                 | BharatVistaar Team — Action                                                                                                                                                              |
| ---- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1    | Create a Beckn Provider Service for each farmer-data source to be integrated (e.g. land records, crop history, soil health, KVK advisory feeds), and host it on State infrastructure — following the same pattern as B.2. | Onboard and register each State farmer-data Provider service into the BV Network, applying the same process used in B.2.                                                                 |
| 2    | Post onboarding, test the use case end-to-end and provide formal go-ahead / sign-off, with the added step of validating advisory accuracy through KVK scientific officers.                                                | Update the AI layer to consume farmer-specific data (not just status fields) and generate personalised, farm-level advisory output; continue to scope access to that State's users only. |
| 3    | Repeat Steps 1–2 for each additional farmer-data source until the full B.3 dataset scope is integrated.                                                                                                                   | Extend AI-layer logic and consent capture for each additional data source as it is onboarded.                                                                                            |
| 4    | Secure formal clearance from the State Agriculture Department for the consent and data-governance framework, and execute the State–Centre data sharing agreement.                                                         | Provide the Data Orchestration Layer such that no farmer data is persisted outside State systems; BV consumes data in-flight for advisory generation only.                               |

{% hint style="info" %}
This stage requires a dedicated State-side data officer and legal clearance from the State Agriculture Department. BV provides the Data Orchestration Layer — no data is stored outside State systems.
{% endhint %}
