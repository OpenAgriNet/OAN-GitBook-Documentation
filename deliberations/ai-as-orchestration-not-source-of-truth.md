# AI as Orchestration, Not Source of Truth

## Context

A conversational AI layer sitting in front of agricultural advisory, scheme, and pricing data could either generate answers itself from what it has learned, or be constrained to only relay information it retrieves from verified systems. For a farmer-facing agricultural network, an unconstrained model risks presenting a hallucinated price, scheme eligibility rule, or pest treatment as if it were fact.

## Decision

The OAN AI module is designed so that AI never acts as a source of truth. It only decides which systems or data sources to invoke and how to present the resulting information — every factual response is:

* Grounded in a knowledge base sourced from trusted institutions (for advisory and schemes), or
* Backed by live API/tool calls (for weather, prices, status checks, and grievances), and
* Constrained by domain-specific guardrails.

## Why it matters

* Responses stay verifiable and auditable, since every factual claim traces back to a registered tool call or an institutional document rather than to the model's own generation.
* It structurally rules out a class of failure — hallucinated or speculative agricultural advice — that would otherwise be the highest-consequence failure mode for this domain.
* It keeps the AI layer swappable/LLM-agnostic (see [Reuse across Deployments](../implementations/reuse-across-deployments.md)): because the model's job is orchestration rather than knowledge, the underlying LLM/SLM can be changed without changing what the system is allowed to assert.
