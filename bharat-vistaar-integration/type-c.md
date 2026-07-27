# Type C

### Type C.1 — Independent State Beckn Network <a href="#type-c1--independent-state-beckn-network" id="type-c1--independent-state-beckn-network"></a>

#### Goal <a href="#goal" id="goal"></a>

The State stands up and operates its own independent Beckn network — its own Gateway and Registry — for its services, decoupled from the BharatVistaar (BV) / Centre network. This gives the State full technical and governance sovereignty over onboarding participants (BAPs and BPPs) within its own jurisdiction, rather than only exposing services into BV's network as in Type B.

#### Definition of Done <a href="#definition-of-done" id="definition-of-done"></a>

* State has deployed and is operating its own Beckn Gateway and Registry infrastructure (or has selected and configured an approved network facilitator).
* At least one State-owned BAP and one State-owned BPP are live and registered on this independent network.
* State services already exposed to BV under B.2/B.3 can optionally also be onboarded as participants on the State's own registry, without disrupting the existing BV integration.
* A dedicated State network governance/operations team is in place, responsible for participant onboarding, uptime, and protocol-policy compliance.

#### Action Items & Workflow <a href="#action-items--workflow" id="action-items--workflow"></a>

| Step | State Government — Action                                                                                                                                                                  | BharatVistaar Team — Action                                                                                                                                                     |
| ---- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1    | Stand up Beckn Gateway and Registry infrastructure (or select an approved network facilitator) and define network policy — subscriber onboarding rules, sectors covered, and access rules. | Share reference architecture, network policy templates, and onboarding checklists drawn from BV's own network operations experience.                                            |
| 2    | Onboard the State's own BAP and at least one BPP onto this new independent registry; test discovery and order flows end-to-end.                                                            | Provide technical advisory support and validate that the State's network conforms to the Beckn protocol specification, so that bi-directional exchange in C.2 remains feasible. |
| 3    | Publish the network participant policy and appoint a dedicated network governance/operations team.                                                                                         | Advise on governance structures based on precedent from other State or sector-level Beckn networks.                                                                             |

{% hint style="info" %}
This stage is a strategic step rather than a routine integration task — the State establishes full technical sovereignty over its own Beckn network stack, independent of BV infrastructure. It requires dedicated protocol-engineering and DevOps capacity on the State side, distinct from the API/content teams used in Type B.
{% endhint %}

### Type C.2 — Bi-directional Network Interoperability with Centre <a href="#type-c2--bi-directional-network-interoperability-with-centre" id="type-c2--bi-directional-network-interoperability-with-centre"></a>

#### Goal <a href="#goal-1" id="goal-1"></a>

The State's independent Beckn network (from C.1) and the Centre's BV network interoperate bi-directionally. Participants on either network can discover and transact with participants on the other — making BV one peer network among several, rather than the sole gateway to Centre schemes and services.

#### Definition of Done <a href="#definition-of-done-1" id="definition-of-done-1"></a>

* A network-to-network interoperability agreement — covering both technical and governance terms — is signed between State and Centre.
* Cross-network discovery works in both directions: Centre/BV BAPs can discover services on the State's registry, and State BAPs can discover Centre schemes and services on BV's registry.
* At least one end-to-end transaction has been completed in each direction (Centre → State and State → Centre) as proof of interoperability.
* A joint monitoring and support mechanism is in place to handle cross-network issues, including SLAs and escalation paths.

#### Action Items & Workflow <a href="#action-items--workflow-1" id="action-items--workflow-1"></a>

| Step | State Government — Action                                                                                                | BharatVistaar Team — Action                                                                                          |
| ---- | ------------------------------------------------------------------------------------------------------------------------ | -------------------------------------------------------------------------------------------------------------------- |
| 1    | Share the State network's registry endpoint and participant list with the Centre/BV network team for cross-registration. | Share BV/Centre's registry endpoint and onboard the State's registry as a peer / federated registry.                 |
| 2    | Test cross-network discovery and transaction flows for at least one use case in each direction.                          | Validate and sign off on protocol compliance and transaction integrity across both directions.                       |
| 3    | Formalise the network interoperability agreement with Centre — data governance, dispute resolution, and SLAs.            | Formalise the same agreement from the Centre side and establish a joint monitoring dashboard and escalation process. |

{% hint style="info" %}
This is the terminal stage of integration depth in this series. From here, the State operates as a full peer participant in the national Beckn ecosystem, with BV as one of potentially several interoperating networks — rather than as the State's sole point of integration.
{% endhint %}

### Summary: From Integrated Consumer to Network Peer <a href="#summary-from-integrated-consumer-to-network-peer" id="summary-from-integrated-consumer-to-network-peer"></a>

Where Type B deepens the State's integration _within_ BV's network — as a content and service provider that BV ingests and personalises — Type C changes the State's role entirely:

* **C.1** gives the State its own independent Beckn network stack (Gateway + Registry), with full autonomy over its own participants, separate from BV's infrastructure.
* **C.2** connects that independent network back to the Centre's network bi-directionally, so State and Centre systems can discover and transact with each other as peers.

Recommended next step

Type C should generally be pursued only after a State has substantial experience operating Beckn Provider services under Type B — the protocol familiarity and technical capacity built there materially de-risks standing up and governing an independent network in C.1.
