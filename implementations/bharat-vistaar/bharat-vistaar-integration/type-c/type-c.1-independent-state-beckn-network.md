# Type C.1 - Independent State Beckn Network

## Goal

The State stands up and operates its own independent Beckn network — its own Gateway and Registry — for its services, decoupled from the BharatVistaar (BV) / Centre network. This gives the State full technical and governance sovereignty over onboarding participants (BAPs and BPPs) within its own jurisdiction, rather than only exposing services into BV's network as in Type B.

## Definition of Done

* State has deployed and is operating its own Beckn Gateway and Registry infrastructure (or has selected and configured an approved network facilitator).
* At least one State-owned BAP and one State-owned BPP are live and registered on this independent network.
* State services already exposed to BV under B.2/B.3 can optionally also be onboarded as participants on the State's own registry, without disrupting the existing BV integration.
* A dedicated State network governance/operations team is in place, responsible for participant onboarding, uptime, and protocol-policy compliance.

## Action Items & Workflow

| Step | State Government — Action                                                                                                                                                                  | BharatVistaar Team — Action                                                                                                                                                     |
| ---- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1    | Stand up Beckn Gateway and Registry infrastructure (or select an approved network facilitator) and define network policy — subscriber onboarding rules, sectors covered, and access rules. | Share reference architecture, network policy templates, and onboarding checklists drawn from BV's own network operations experience.                                            |
| 2    | Onboard the State's own BAP and at least one BPP onto this new independent registry; test discovery and order flows end-to-end.                                                            | Provide technical advisory support and validate that the State's network conforms to the Beckn protocol specification, so that bi-directional exchange in C.2 remains feasible. |
| 3    | Publish the network participant policy and appoint a dedicated network governance/operations team.                                                                                         | Advise on governance structures based on precedent from other State or sector-level Beckn networks.                                                                             |

{% hint style="info" %}
This stage is a strategic step rather than a routine integration task — the State establishes full technical sovereignty over its own Beckn network stack, independent of BV infrastructure. It requires dedicated protocol-engineering and DevOps capacity on the State side, distinct from the API/content teams used in Type B.
{% endhint %}
