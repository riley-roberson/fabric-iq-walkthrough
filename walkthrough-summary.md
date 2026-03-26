---
title: Walkthrough Summary
layout: default
nav_order: 8
---

# Walkthrough Summary: End-to-End Flow

This diagram shows the full Fabric IQ stack — from your Power BI semantic model through the ontology layer to both agent types.

---

```mermaid
flowchart TD
    PBSM["<b>Power BI Semantic Model</b><br/>Trusted KPIs & measures"]

    subgraph ONT["1. ONTOLOGY"]
        direction TB
        O1["Import entity types from<br/>semantic model"]
        O2["Define properties & keys"]
        O3["Bind static & time series data"]
        O4["Create relationships"]
        O5["Explore via graph preview"]
        O1 --> O2 --> O3 --> O4 --> O5
    end

    subgraph DA["2. DATA AGENT"]
        direction TB
        D1["Add ontology as source"]
        D2["Add semantic model as source"]
        D3["Write agent instructions"]
        D4["Add example queries"]
        D5["Test & publish"]
        D1 --> D2 --> D3 --> D4 --> D5
    end

    subgraph OA["3. OPERATIONS AGENT"]
        direction TB
        A1["Set business goals"]
        A2["Add KQL knowledge source"]
        A3["Define actions"]
        A4["Configure Activator +<br/>Power Automate"]
        A5["Generate playbook"]
        A6["Start agent & Teams msgs"]
        A1 --> A2 --> A3 --> A4 --> A5 --> A6
    end

    PBSM --> ONT
    ONT --> DA
    ONT --> OA

    style PBSM fill:#4472C4,color:#fff,stroke:#2F5597
    style ONT fill:#E2EFDA,stroke:#548235
    style DA fill:#D6E4F0,stroke:#2E75B6
    style OA fill:#FCE4D6,stroke:#ED7D31
```

---

## Phase-by-phase summary

| Phase | Key deliverable | Dependencies |
|:------|:----------------|:-------------|
| **Phase 1** | Tenant settings enabled, workspace provisioned | Fabric admin access |
| **Phase 2** | Ontology with entity types, bindings, relationships, graph | Phase 1 complete, Power BI semantic model |
| **Phase 3** | Published data agent with instructions and tested queries | Phase 1 + 2 complete |
| **Phase 4** | Running operations agent with playbook and Teams integration | Phase 1 + eventhouse + KQL database |
