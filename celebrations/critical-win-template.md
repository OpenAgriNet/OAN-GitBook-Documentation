# OAN Kenya

**Date:** 2026-08-10\
**People and teams:**

**Infrastructure & implementation — SAFIC:** Led by Prof. Simon Wagura Ndiritu, SAFIC drove the hands-on implementation that turned the Sprint-1 vision into a working deployment. Joseph Gitonga provided technical leadership and coordination; Michael Gichure led infrastructure and deployment readiness; Peter Gicharu drove ONIX deployment and provider onboarding; Isaac Masinde and Braico Mwangi led Seeker implementation and validation; and Charles Mwaniki supported onboarding, integrations, payload mapping, telemetry, and observability. Together, the team deployed the network, onboarded providers, validated the first live transaction, and demonstrated the first end-to-end use case on the OAN Kenya network.

**Program leadership — COSS:** Jagadish Babu, COO of Ek Step, set the founding vision. Anand Varada, Thejaswini Anand, and Nirant coordinated the broader OAN ecosystem, aligning scope, priorities, and success criteria while enabling the delivery team to move quickly and make decisions on the ground.

**Technical enablement & architecture — Deloitte:** Tejash, Kanak Kaushik, and Rupal Singla provided architecture guidance, provider onboarding, payload mapping, integration support, troubleshooting, and implementation artifacts that enabled rapid deployment and validation of the Seeker and Provider components.

**Program delivery & governance — Deloitte:** Nanda Kishore led sprint execution through a clear day-by-day delivery plan, proactively managing dependencies, risks, and blockers. Meenakshi Rampati provided program-level support throughout the engagement.

_This milestone was the result of the entire OAN Kenya team working together—combining ecosystem leadership, program governance, technical enablement, and hands-on implementation to move from vision to a live end-to-end transaction._

## The win

### The milestone: first live end-to-end transaction

On 3 August 2026, the team successfully demonstrated the first fully working, end-to-end transaction on the OAN Kenya network. A search request travelled the complete path -- Seeker (BAP) → Adapter → Gateway → Provider (BPP) -- and returned with a live weather advisory response. This is the first time every layer of the network has worked together in one continuous flow, and it is the milestone this entry is meant to mark: the moment OAN Kenya stopped being a set of components and became a working network.

What makes this milestone particularly notable is the speed of execution. Within the first week of Sprint-1, the team moved from initial network setup to a live end-to-end transaction, bringing both the Seeker and Provider sides online and validating the first real network exchange. In practical terms, OAN Kenya went from zero to its first live transaction in just five working days.

This achievement demonstrated not only the technical viability of the solution, but also the effectiveness of the OpenAgriNet model in rapidly onboarding ecosystem participants through a shared, interoperable network architecture.

<figure><img src="../.gitbook/assets/image (15).png" alt=""><figcaption></figcaption></figure>

_The path of a single search request through the OAN Kenya network: from the Seeker application to the Weather Provider and back_

## Why it matters

This milestone demonstrates that the foundational components of the OAN Kenya network are operational and working together as intended. The Seeker application, network infrastructure, and Weather Provider exchanged information in real time, validating the core architecture and onboarding model that future Advisory, Market Information, and other provider services can build upon.

What makes this milestone worth writing down is not just that it happened, but how it compares to what was originally planned. The agreed Sprint-1 charter laid out a 10-working-day plan, with Monday, 27 July as Day 1. Under that plan, Day 6 was scoped for routing and API configuration work - “a successful test request routed from consumer to provider” with formal end-to-end testing reserved for Day 9 and the full demonstration milestone not due until Day 10.

Day 6: Instead of a routed test request, the team delivered a live, working, end-to-end demonstration with the search request, Gateway routing, and weather response operating together in a single flow. This milestone was achieved four working days ahead of the original Sprint-1 plan, demonstrating both the team's execution speed and the effectiveness of the collaborative implementation approach.

Planned: Day 10 |  Actual: Day 6 | Acceleration: 4 working days ahead of plan

It is worth noting that the underlying work - full network setup, use case finalization, and onboarding of both the Seeker and Provider applications was already completed by Day 4. The Day 6 demonstration was the visible proof point of groundwork that had been completed earlier.

## Notes

While this milestone focused on a single Weather use case, it validated the architecture, onboarding approach, and operational model that future providers will build upon. The next phase will focus on Agricultural Advisory services, telemetry, and onboarding additional providers using the same repeatable framework. With the network, Seeker experience, and provider integration model now validated, the team is well positioned to scale onboarding activities and accelerate delivery of the remaining Sprint-1 objectives.

Key lessons carried forward as follow-up practice:

<table data-header-hidden><thead><tr><th valign="top"></th><th valign="top"></th></tr></thead><tbody><tr><td valign="top">Lesson</td><td valign="top">Applied as</td></tr><tr><td valign="top">Keep the scope deliberately narrow for a first milestone -- proving one use case end-to-end made it possible to find and fix problems quickly instead of debugging several moving parts at once.</td><td valign="top">Continue onboarding additional providers one at a time using the same repeatable framework, rather than in parallel.</td></tr><tr><td valign="top">Resolve blockers the moment they surface -- the team used the working WhatsApp group to flag and close issues in real time rather than waiting for the next scheduled call.</td><td valign="top">Keep same-day blocker resolution as the standing practice for the next milestone, not just this one.</td></tr><tr><td valign="top">Good documentation is a force multiplier for a new team -- clear, step-by-step build documentation and an upfront architecture walkthrough meant the team didn't have to learn by trial and error.</td><td valign="top">Invest in documentation and a walkthrough ahead of every new milestone, not just this one.</td></tr><tr><td valign="top">Give the team closest to the work the autonomy to make decisions and execute -- clear goals from program leadership, paired with trust in the delivery team's day-to-day judgment, kept decisions moving.</td><td valign="top">Preserve the same light-touch governance model as onboarding scales.</td></tr><tr><td valign="top">Celebrate the first one properly -- taking time for a team photo and writing this entry is how a program builds a record it can look back on.</td><td valign="top">Keep documenting each milestone this way going forward.</td></tr></tbody></table>
