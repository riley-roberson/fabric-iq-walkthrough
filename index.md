---
title: Home
layout: default
nav_order: 1
---

# Fabric IQ Walkthrough

A contractor's guide to building the **Fabric IQ stack** — from Power BI semantic model through Ontology, Data Agent, and Operations Agent — grounded in official Microsoft Learn documentation.
{: .fs-6 .fw-300 }

---

## Project context

This project uses a **Power BI semantic model** as the foundational data source for building out the Fabric IQ stack: **Ontology → Data Agent → Operations Agent**. The semantic model provides the trusted KPIs, dimensions, and measures that feed the ontology's shared business vocabulary, which in turn grounds both the data agent (conversational Q&A) and the operations agent (real-time autonomous actions).

## How to use this guide

Follow the phases in order. Each phase builds on the previous:

| Phase | What you'll do | Page |
|:------|:---------------|:-----|
| **Phase 1** | Enable tenant settings and workspace prerequisites | [Prerequisites]({% link phase-1-prerequisites.md %}) |
| **Phase 2** | Define entity types, bind data, create relationships | [Ontology]({% link phase-2-ontology.md %}) |
| **Phase 3** | Create a conversational Q&A agent over your data | [Data Agent]({% link phase-3-data-agent.md %}) |
| **Phase 4** | Set up real-time monitoring and autonomous actions | [Operations Agent]({% link phase-4-operations-agent.md %}) |

## Quick links

- [What is Fabric IQ?]({% link what-is-fabric-iq.md %}) — Overview of the IQ workload and its items
- [Architecture comparison]({% link architecture.md %}) — Data Agent vs Operations Agent
- [End-to-end flow]({% link walkthrough-summary.md %}) — Visual diagram of the full stack
- [Reference links]({% link references.md %}) — All official Microsoft Learn sources

All content in this guide is sourced from official Microsoft Learn documentation.
{: .note }
