# Type B.1 - Published Info Integration

## Goal

Citizens can access both Central and State scheme information, departmental guidelines, and agro-advisory content through a single BharatVistaar conversational interface, in regional languages.

## Definition of Done

* State scheme documents and advisories are ingested and live on BV.
* Farmers can query a state-specific scheme by name and receive accurate, current information.
* State content is updated on BV within 30 days of any scheme notification or revision.
* Language quality of state content is validated by state-nominated language experts.

## Action Items & Workflow

| Step | State Government — Action                                                                                                         | BharatVistaar Team — Action                                                                                     |
| ---- | --------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------- |
| 1    | Consolidate the list of schemes and their details in the format prescribed by BV, and share the file with the BV team for upload. | Upload the scheme information provided by the State Government into the BV content repository.                  |
| 2    | Test scheme discovery and the resulting advisory output on BV after each upload, to confirm accuracy and completeness.            | Support the State's testing cycle and log any discrepancies raised for correction.                              |
| 3    | Repeat Steps 1–2 for every new or revised scheme, so BV content stays current on a rolling basis.                                 | Continue ingesting incremental scheme updates as they are received from the State.                              |
| 4    | Test language accuracy and terminology in the regional-language output, and share structured feedback with the BV team.           | Address language and terminology issues raised by the State, and optimise regional-language output accordingly. |

{% hint style="info" %}
No API development required. The State needs to provide a PMU and a single point of contact (SPOC) to coordinate integration. The BV Team handles content ingestion and indexing end-to-end.
{% endhint %}
