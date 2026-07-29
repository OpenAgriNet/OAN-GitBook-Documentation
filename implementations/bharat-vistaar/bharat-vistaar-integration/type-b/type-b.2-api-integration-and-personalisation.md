# Type B.2 - API Integration and Personalisation

## Goal

BharatVistaar can provide personalised, consent-based responses to farmers — checking scheme eligibility, application status, benefit disbursement, and grievance status — by using live State system APIs.

## Definition of Done

* A minimum of 3 State APIs are live on BV: application tracking, benefit status, and grievance redressal.
* A farmer can check their own scheme application status via BV using their registered mobile number.
* The State tech team has completed API testing and sign-off on all integrated endpoints.
* A consent mechanism is functional — the farmer explicitly authorises data use before a personalised query is served.

## Action Items & Workflow

| Step | State Government — Action                                                                                                                                                                                       | BharatVistaar Team — Action                                                                                                                                                                     |
| ---- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1    | Create a Beckn Provider Service for each State service to be exposed for integration (for example, if the State has a grievance system, expose it as a Beckn Provider API and host it on State infrastructure). | Onboard and register the State's Provider service(s) into the BV Network.                                                                                                                       |
| 2    | Post onboarding, test the use case end-to-end through the State's integrated system and provide formal go-ahead / sign-off.                                                                                     | Update the AI layer in BV to enable discovery and use of the new service. Abstract the service so it is scoped only to that State's users, and not exposed to other states or general BV users. |
| 3    | Repeat Steps 1–2 for each additional State service (e.g. application tracking, benefit disbursement, grievance redressal) until at least 3 APIs are live.                                                       | Extend the AI layer configuration and consent-capture flow for each additional service as it is onboarded.                                                                                      |

{% hint style="info" %}
The State needs to provide a dedicated technical team and PMU to coordinate integration. BV provides the integration specifications and a sandbox environment for testing.
{% endhint %}
