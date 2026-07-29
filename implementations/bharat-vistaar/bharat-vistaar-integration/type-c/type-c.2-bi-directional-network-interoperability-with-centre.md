# Type C.2 - Bi-directional Network Interoperability with Centre

## Goal

The State's independent Beckn network (from C.1) and the Centre's BV network interoperate bi-directionally. Participants on either network can discover and transact with participants on the other — making BV one peer network among several, rather than the sole gateway to Centre schemes and services.

## Definition of Done

* A network-to-network interoperability agreement — covering both technical and governance terms — is signed between State and Centre.
* Cross-network discovery works in both directions: Centre/BV BAPs can discover services on the State's registry, and State BAPs can discover Centre schemes and services on BV's registry.
* At least one end-to-end transaction has been completed in each direction (Centre → State and State → Centre) as proof of interoperability.
* A joint monitoring and support mechanism is in place to handle cross-network issues, including SLAs and escalation paths.

## Action Items & Workflow

| Step | State Government — Action                                                                                                | BharatVistaar Team — Action                                                                                          |
| ---- | ------------------------------------------------------------------------------------------------------------------------ | -------------------------------------------------------------------------------------------------------------------- |
| 1    | Share the State network's registry endpoint and participant list with the Centre/BV network team for cross-registration. | Share BV/Centre's registry endpoint and onboard the State's registry as a peer / federated registry.                 |
| 2    | Test cross-network discovery and transaction flows for at least one use case in each direction.                          | Validate and sign off on protocol compliance and transaction integrity across both directions.                       |
| 3    | Formalise the network interoperability agreement with Centre — data governance, dispute resolution, and SLAs.            | Formalise the same agreement from the Centre side and establish a joint monitoring dashboard and escalation process. |

{% hint style="info" %}
This is the terminal stage of integration depth in this series. From here, the State operates as a full peer participant in the national Beckn ecosystem, with BV as one of potentially several interoperating networks — rather than as the State's sole point of integration.
{% endhint %}
