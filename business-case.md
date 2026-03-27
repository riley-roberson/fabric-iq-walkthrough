---
title: Business Case
layout: default
nav_order: 10
---

# Making the Business Case for Fabric IQ

How to pitch Fabric IQ to leadership, build internal advocacy, and get funding for a pilot.

---

## The elevator pitch

> "We already pay for Microsoft Fabric. Fabric IQ lets us turn our existing data into a shared business language that AI agents can understand. Instead of analysts writing queries, people ask questions in plain English. Instead of dashboards no one checks, agents monitor data 24/7 and alert us when something needs attention. We're not buying new infrastructure — we're unlocking what we already have."

---

## Who to talk to (and what they care about)

| Stakeholder | What they care about | Your talking point |
|:------------|:---------------------|:-------------------|
| **CFO / Finance** | Cost, ROI, headcount efficiency | "No new licensing — runs on existing Fabric capacity. Reduces time-to-insight from days to seconds." |
| **CIO / IT** | Security, governance, integration | "Built on Entra ID, OneLake, and existing permissions. No new data copies. No third-party AI keys." |
| **CDO / Data** | Data quality, consistency, adoption | "The ontology creates one shared vocabulary. Every team uses the same definitions for the same metrics." |
| **Line-of-business VP** | Speed, decisions, competitive advantage | "Your team gets answers in seconds instead of filing a ticket. The ops agent catches problems before customers do." |
| **CISO / Security** | Data residency, access control, audit trail | "All queries run under existing Entra ID permissions. Transparency steps log every query. No data leaves OneLake." |

---

## Common objections and responses

### "We already have Power BI dashboards"

Dashboards answer questions someone already thought to ask. The data agent answers questions nobody anticipated. The operations agent watches data when nobody's looking. Power BI is the foundation — IQ makes it smarter.

### "AI hallucinations will give us wrong answers"

The data agent doesn't guess — it generates SQL, DAX, or KQL against your actual data and shows you the query. If the answer is wrong, you can see exactly why and fix the instructions. It's more transparent than a human analyst who doesn't show their work.

### "We don't have the budget"

Fabric IQ runs on existing Fabric capacity. If you already have F4 or above, you can start today. The ontology, data agent, and graph are all included. The only incremental cost is the CU consumption, which you can monitor and control.

### "Our data isn't ready"

If you have a Power BI semantic model with measures and dimensions, your data is ready enough. The ontology imports entity types directly from your semantic model. Start with one domain — you don't need a perfect enterprise data model to get value.

### "This is just a preview — too risky for production"

Use it for internal teams first. The data agent is read-only — it can't modify data. Start with a pilot of 5-10 users on a non-critical domain. By the time it's GA, you'll have instructions tuned, users trained, and a proven value case.

### "Our team doesn't have the skills"

If your team can build a Power BI report, they can configure a data agent. The ontology is visual and no-code. The operations agent setup is guided. The hardest part is writing good instructions — and that's a business knowledge problem, not a technical one.

---

## Building a pilot proposal

### Scope it small

Pick **one business domain** with these characteristics:
- An existing Power BI semantic model that's already trusted
- A team that currently files frequent data requests
- At least one metric that needs continuous monitoring (for the operations agent)
- A business sponsor who will champion the results

### 30-day pilot plan

| Week | Milestone |
|:-----|:----------|
| **Week 1** | Enable tenant settings. Create ontology from semantic model. Bind data, define relationships. |
| **Week 2** | Build data agent. Write instructions. Test with 3-5 pilot users. Iterate on instructions daily. |
| **Week 3** | Set up operations agent (if eventhouse data exists). Define 2-3 monitoring rules. Connect to Teams. |
| **Week 4** | Measure results. Document wins. Present to stakeholders. Decide on expansion. |

### Metrics to track

| Metric | How to measure | Target |
|:-------|:---------------|:-------|
| Time-to-answer | Compare data request ticket time vs agent response time | 90% reduction |
| Analyst utilization | Hours spent on ad-hoc requests before/after | 50% reduction |
| Agent accuracy | Correct answers / total questions asked | >85% in week 4 |
| User adoption | Weekly active users of the data agent | 80% of pilot group |
| Detection speed (ops agent) | Time from event to alert | Minutes vs hours/days |

---

## The strategic argument

### From reporting to intelligence

```
Traditional:  Data → Dashboard → Human notices → Human decides → Human acts
                     (hours/days)   (maybe)        (eventually)

Fabric IQ:    Data → Ontology → Agent monitors → Agent alerts → Human approves → Action
                     (seconds)    (always)        (immediately)   (one click)
```

### Compounding value

The ontology is the investment that keeps paying off:
1. **First value**: Data agent answers ad-hoc questions (weeks 1-2)
2. **Second value**: Operations agent monitors and alerts (week 3)
3. **Third value**: Consistent terminology across all new reports and models
4. **Fourth value**: Graph queries for relationship analysis (impact chains, dependencies)
5. **Fifth value**: Foundation for future AI agents and copilots that understand your business

Each new data source, agent, or report that connects to the ontology gets the shared vocabulary for free.

---

## Presentation template

Use this structure for a 15-minute leadership pitch:

1. **The problem** (2 min): "Our analysts spend X hours/week answering ad-hoc data questions. Y critical alerts were missed last quarter because no one was watching the dashboard."
2. **The solution** (3 min): "Fabric IQ gives us a shared data vocabulary, a Q&A agent, and an always-on monitoring agent — all running on infrastructure we already pay for."
3. **The demo** (5 min): Show the data agent answering a real question from your semantic model. Show the transparency steps. Show the ontology graph.
4. **The ask** (2 min): "We need 4 weeks and a team of 2 to run a pilot on [specific domain]. No new licensing. Minimal capacity impact."
5. **The upside** (3 min): Metrics you expect to hit. Strategic value of the ontology. Path to expansion.
