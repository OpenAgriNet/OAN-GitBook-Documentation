# Type-Tiering for Bharat VISTAAR Integration

## Context

States onboarding onto Bharat VISTAAR vary widely in existing technical capacity — some can expose live APIs and take on data-governance obligations from day one, others can only hand over content for BV to ingest. A single integration path would either be too heavy for less-ready states or too shallow to unlock personalisation for more-ready ones.

## Decision

Integration depth is split into two tracks, each staged as a ladder rather than a single all-or-nothing integration:

* **Type B** (State exposes content/services *into* BV's network) — B.1 content-only, B.2 live API + consent-gated personalisation, B.3 farmer-specific data with formal data-governance sign-off.
* **Type C** (State operates its *own* Beckn network) — C.1 stands up an independent Gateway/Registry, C.2 makes that network bi-directionally interoperable with the Centre/BV network.

Each tier has its own Definition of Done and a State/BharatVistaar-Team action-item split, so a state can stop at whichever tier matches its current readiness without being blocked on the next one.

## Why it matters

* A state's integration effort and governance burden scale with the tier it takes on — B.1 requires no API work at all, while B.3/C.2 require a data officer and formal Centre–State agreements.
* Type C is explicitly sequenced after Type B experience, since operating an independent network is de-risked by first having run Beckn Provider services under BV's network.
* Recording the tiers separately (rather than as one combined "integration guide") makes each stage's own scope, sign-off criteria, and hint/caveat text discoverable on its own page.
