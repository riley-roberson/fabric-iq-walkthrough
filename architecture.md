---
title: Architecture
layout: default
nav_order: 7
---

# Architecture: Data Agent vs Operations Agent

Understanding the architectural differences between the two agent types helps you choose the right one for your scenario — or decide to use both.

---

## Comparison

| Dimension | Data Agent | Operations Agent |
|:----------|:-----------|:-----------------|
| **Role** | Conversational analytics — answering questions | Autonomous decision-making — monitoring and acting |
| **Data access** | Lakehouses, warehouses, Power BI models, KQL databases, ontologies | Real-time data streams via eventhouse/KQL databases |
| **Grounding** | Ontology for semantic grounding + NL2SQL/DAX/KQL | Ontology-based rules + real-time event interpretation |
| **Integration** | Teams, Copilot Studio, custom apps, Microsoft 365 Copilot, Foundry | Activator, Power Automate, Teams alerts/approvals, ERP/CRM |
| **Interaction** | User asks a question → agent retrieves answer | Agent monitors continuously → recommends/triggers actions |
| **Output** | Structured query results + transparency steps | Notifications + recommended actions with approval workflow |

---

## Decision guide

### Use a Data Agent when

- Users need to ask ad-hoc questions about historical or current data
- You want natural language access to warehouses, lakehouses, or semantic models
- The goal is **self-service analytics** without writing SQL/DAX/KQL
- Answers are read-only and don't trigger downstream processes

### Use an Operations Agent when

- You need **continuous monitoring** of real-time metrics or event streams
- Business rules should trigger notifications or actions automatically
- The workflow involves **human-in-the-loop approvals** via Teams
- Integration with Power Automate, Activator, or ERP/CRM systems is required

### Use both when

- The data agent handles ad-hoc questions and exploration
- The operations agent monitors the same data for threshold breaches and triggers actions
- The ontology provides a shared semantic layer grounding both agents
