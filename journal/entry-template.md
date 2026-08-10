# OAN Kenya

**Date & time:** 2026-08-03, 10 AM EAT\
**Event type:** Milestone\
**Participants or owners:**

Infrastructure and hands-on implementation -- SAFIC team, led by Prof. Simon Wagura Ndiritu, Centre Director, who played a central role in turning the Sprint-1 vision into a working implementation. Joseph Gitonga (JT) provided overall technical leadership and implementation coordination. Michael Gichure (Technical Lead) led infrastructure readiness, environment setup, deployment coordination, access management, and overall technical execution. Peter Gicharu drove the ONIX deployment, provider onboarding, and integration activities, while Isaac Masinde and Braico Mwangi led the implementation and validation of the Seeker application and provider integrations. Charles Mwaniki supported provider onboarding, payload mapping, advisory service integration, telemetry, and observability activities. Together, the team deployed the network components, onboarded the initial providers, validated the first live transaction, and demonstrated the first end-to-end use case on the OAN Kenya network.

Program leadership -- COSS. Jagadish Babu, COO of Ek Step, set the founding vision. The COSS team -- Anand Varada, Thejaswini Anand, and Nirant -- coordinated across the broader OpenAgriNet ecosystem to ensure alignment on scope, priorities, and success criteria for the initial use case, while giving the delivery team the flexibility to make day-to-day decisions on the ground. This balance of clear strategic direction and a light-touch governance approach was a key factor in maintaining the project's pace.

Technical enablement and architecture guidance -- Deloitte. Tejash, Technical Architect, together with Kanak Kaushik and Rupal Singla, designed, operationalized, and supported the provider onboarding framework, payload mapping approach, implementation artifacts, and architecture guidance that enabled rapid deployment and validation of both Seeker and Provider components. The team provided hands-on onboarding support, troubleshooting assistance, integration guidance, and architecture walkthroughs throughout the sprint, enabling SAFIC to move from setup to a live end-to-end transaction within the first week.

Program discipline and delivery governance -- Deloitte. Nanda Kishore, Project Manager for OAN Kenya, led the sprint execution against a clear day-by-day delivery plan from the outset, ensuring that dependencies, risks, and open items were identified, tracked, and addressed daily rather than discovered late. When blockers emerged, he coordinated with stakeholders to resolve them the same day, preventing issues from carrying over and maintaining delivery momentum. Meenakshi Rampati provided program-level support as needed throughout the engagement.

_No single contribution alone would have been sufficient to achieve this outcome. It was the combination of ecosystem leadership, program governance, technical enablement, and hands-on implementation working together throughout the sprint that made this milestone possible. The achievement belongs to the entire OAN Kenya team._

## What happened

Smallholder farmers produce a significant share of Africa's food supply, yet access to timely weather information, market intelligence, and agricultural advisory services remains fragmented. Addressing this challenge requires more than standalone applications -- it requires a shared, open digital infrastructure that enables multiple organizations to discover, connect, and exchange information without bespoke integrations between every participant.

OpenAgriNet (OAN) is designed to provide that enabling layer. Built on the Beckn Protocol, OAN is a model-agnostic, AI-enabled, interoperable network that enables farmer-facing applications, AI agents, and service providers to interact through a common set of open standards. Rather than relying on a single platform, vendor, or AI model, OAN enables participants to securely discover and transact with any registered provider through a shared network architecture. OAN Kenya is one of the first implementations of this vision, demonstrating how weather and agricultural advisory services can be delivered through an open, scalable, and reusable ecosystem.

### SAFIC's role in Kenya's agri-food transformation

_Source: “How SAFIC is Spearheading Africa's Agri-food Transformation,” Strathmore University, 19 March 2025_

SAFIC -- the Strathmore Agri-Food Innovation Centre, led by Principal Investigator Prof. Simon Wagura Ndiritu -- is leading the Kenya-side implementation of OAN. Its mission focuses on translating research into commercially viable solutions that support SMEs across the agricultural value chain. SAFIC's work spans four pillars -- Data and Insight for Decision Support, Business Advisory, Market Intelligence, and Research and Innovation Brokerage -- with initiatives extending from county-level maize production research to supporting pastoralist off-takers exporting meat to GCC markets.

Prof. Ndiritu has identified a core challenge across the region: access to accurate and timely market intelligence remains difficult, with data often fragmented, outdated, or unavailable. OAN Kenya's weather-advisory use case directly addresses this gap by enabling timely information exchange through a shared, interoperable network rather than another one-off integration.

### The milestone: first live end-to-end transaction

On 3 August 2026, the team successfully demonstrated the first fully working, end-to-end transaction on the OAN Kenya network. A search request travelled the complete path -- Seeker (BAP) → Adapter → Gateway → Provider (BPP) -- and returned with a live weather advisory response. This is the first time every layer of the network has worked together in one continuous flow, and it is the milestone this entry is meant to mark: the moment OAN Kenya stopped being a set of components and became a working network.

What makes this milestone particularly notable is the speed of execution. Within the first week of Sprint-1, the team moved from initial network setup to a live end-to-end transaction, bringing both the Seeker and Provider sides online and validating the first real network exchange. In practical terms, OAN Kenya went from zero to its first live transaction in just five working days.

This achievement demonstrated not only the technical viability of the solution, but also the effectiveness of the OpenAgriNet model in rapidly onboarding ecosystem participants through a shared, interoperable network architecture.

### Why this milestone matters

This milestone demonstrates that the foundational components of the OAN Kenya network are operational and working together as intended. The Seeker application, network infrastructure, and Weather Provider exchanged information in real time, validating the core architecture and onboarding model that future Advisory, Market Information, and other provider services can build upon.

The SAFIC team led the demonstration, walking the group through a real search request and observing the weather response return through the Gateway in real time. The team paused afterwards for a photo together -- marking the first milestone in what we hope will become a growing record of achievements across the program.

### SAFIC's Sprint-1 reflections

The 3 August stand-up closed with an open reflection on the first week -- not just what was built, but what the team learned along the way. Their reflections provide an important perspective on the milestone beyond the delivery metrics alone.

From unfamiliar to confident: Team members consistently highlighted that the first few days involved a steep learning curve. However, hands-on implementation, supported by clear onboarding documentation and architecture walkthroughs, quickly turned initial uncertainty into practical confidence. Several participants noted that building and troubleshooting the solution themselves helped transform theory into practical understanding.

Recognition from the technical team: The technical lead recognized the pace of implementation, noting that deploying both the Seeker and Provider sides and achieving an end-to-end transaction within the first week represented a significant achievement for a new team.

A notable aspect of this milestone was the SAFIC team's rapid implementation ownership. Within the first week, the team deployed the core network components, onboarded both the Seeker and Provider applications, and completed the first end-to-end validation. Their willingness to learn, troubleshoot, and iterate quickly was a key factor in achieving this milestone.

### How we got here: from zero to a live network in five days

What makes this milestone worth writing down is not just that it happened, but how it compares to what was originally planned. The agreed Sprint-1 charter laid out a 10-working-day plan, with Monday, 27 July as Day 1. Under that plan, Day 6 was scoped for routing and API configuration work -- “a successful test request routed from consumer to provider” -- with formal end-to-end testing reserved for Day 9 and the full demonstration milestone not due until Day 10.

Day 6 fell on Monday, 3 August. Instead of a routed test request, the team delivered a live, working, end-to-end demonstration -- with the search request, Gateway routing, and weather response operating together in a single flow. This milestone was achieved four working days ahead of the original Sprint-1 plan, demonstrating both the team's execution speed and the effectiveness of the collaborative implementation approach.

Planned: Day 10  |  Actual: Day 6  |  Acceleration: 4 working days ahead of plan

It is worth noting that the underlying work -- full network setup, use case finalization, and onboarding of both the Seeker and Provider applications -- was already completed by Day 4. The Day 6 demonstration was the visible proof point of groundwork that had been completed earlier.

## What happened

Smallholder farmers produce a significant share of Africa's food supply, yet access to timely weather information, market intelligence, and agricultural advisory services remains fragmented. Addressing this challenge requires more than standalone applications -- it requires a shared, open digital infrastructure that enables multiple organizations to discover, connect, and exchange information without bespoke integrations between every participant.&#x20;

OpenAgriNet (OAN) is designed to provide that enabling layer. Built on the Beckn Protocol, OAN is a model-agnostic, AI-enabled, interoperable network that enables farmer-facing applications, AI agents, and service providers to interact through a common set of open standards. Rather than relying on a single platform, vendor, or AI model, OAN enables participants to securely discover and transact with any registered provider through a shared network architecture. OAN Kenya is one of the first implementations of this vision, demonstrating how weather and agricultural advisory services can be delivered through an open, scalable, and reusable ecosystem.

## Decisions and outcomes



* Registry deployed and operational&#x20;
* Gateway deployed and operational&#x20;
* Seeker application deployed and connected&#x20;
* Weather Provider onboarded successfully&#x20;
* End-to-end Weather use case demonstrated live, in front of the team&#x20;
* First live transaction completed through the OAN Kenya network&#x20;
* Sprint-1 Milestone 1 formally marked complete, establishing a validated, repeatable foundation for onboarding additional providers onto the OAN Kenya ecosystem&#x20;

## Follow-ups

While this milestone focused on a single Weather use case, it validated the architecture, onboarding approach, and operational model that future providers will build upon. The next phase will focus on Agricultural Advisory services, telemetry, and onboarding additional providers using the same repeatable framework. With the network, Seeker experience, and provider integration model now validated, the team is well positioned to scale onboarding activities and accelerate delivery of the remaining Sprint-1 objectives.

Key lessons carried forward as follow-up practice:

<table data-header-hidden><thead><tr><th valign="top"></th><th valign="top"></th></tr></thead><tbody><tr><td valign="top">Lesson</td><td valign="top">Applied as</td></tr><tr><td valign="top">Keep the scope deliberately narrow for a first milestone -- proving one use case end-to-end made it possible to find and fix problems quickly instead of debugging several moving parts at once.</td><td valign="top">Continue onboarding additional providers one at a time using the same repeatable framework, rather than in parallel.</td></tr><tr><td valign="top">Resolve blockers the moment they surface -- the team used the working WhatsApp group to flag and close issues in real time rather than waiting for the next scheduled call.</td><td valign="top">Keep same-day blocker resolution as the standing practice for the next milestone, not just this one.</td></tr><tr><td valign="top">Good documentation is a force multiplier for a new team -- clear, step-by-step build documentation and an upfront architecture walkthrough meant the team didn't have to learn by trial and error.</td><td valign="top">Invest in documentation and a walkthrough ahead of every new milestone, not just this one.</td></tr><tr><td valign="top">Give the team closest to the work the autonomy to make decisions and execute -- clear goals from program leadership, paired with trust in the delivery team's day-to-day judgment, kept decisions moving.</td><td valign="top">Preserve the same light-touch governance model as onboarding scales.</td></tr><tr><td valign="top">Celebrate the first one properly -- taking time for a team photo and writing this entry is how a program builds a record it can look back on.</td><td valign="top">Keep documenting each milestone this way going forward.</td></tr></tbody></table>

&#x20;

Concrete follow-up actions:

<table data-header-hidden><thead><tr><th valign="top"></th><th valign="top"></th><th valign="top"></th></tr></thead><tbody><tr><td valign="top">Action</td><td valign="top">Owner</td><td valign="top">Due date</td></tr><tr><td valign="top">Onboard the Agricultural Advisory provider using the same repeatable onboarding framework validated in this milestone</td><td valign="top">SAFIC</td><td valign="top">04-08-2026</td></tr><tr><td valign="top">Complete telemetry and observability setup</td><td valign="top">SAFIC / Open AgriNet</td><td valign="top">06-08-2026</td></tr><tr><td valign="top">Continue onboarding additional providers onto OAN Kenya using the same framework</td><td valign="top">SAFIC &#x26; Open AgriNet</td><td valign="top">07-08-2026</td></tr><tr><td valign="top">Carry forward the five key lessons above into planning for the next milestone</td><td valign="top">Deloitte PMO</td><td valign="top">10-08-2026</td></tr></tbody></table>

## &#x20;

## References

•     Source cited in the original entry: “How SAFIC is Spearheading Africa's Agri-food Transformation,” Strathmore University, 19 March 2025

•     Network flow diagram (embedded above) -- Seeker → Adapter → Gateway → Provider path

•     Screenshot from the 3 August stand-up call where the milestone was reviewed (embedded above)



### Glossary of terms

•     SAFIC: Strathmore Agri-Food Innovation Centre, the lead implementation partner for OAN Kenya.

•     Beckn Protocol: The open standard that allows independent applications and service providers to exchange information through a common network.

•     Seeker (BAP): The application through which a user initiates a request to discover an available service.

•     Provider (BPP): The provider-side application or service that responds to requests such as weather advisories or agricultural information.

•     Gateway: The network component that routes requests between participating applications and services.
