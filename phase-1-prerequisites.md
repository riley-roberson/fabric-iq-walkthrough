---
title: "Phase 1: Prerequisites"
layout: default
nav_order: 3
---

# Phase 1: Tenant & Workspace Prerequisites

Before any Fabric IQ work, a Fabric administrator must enable these tenant settings in the **Admin Portal → Tenant Settings**.

---

## Required tenant settings

### 1. Ontology item (preview)

Enable **Ontology item (preview)** — required to create ontology items.
{: .prerequisite }

### 2. Graph (preview)

Enable **User can create Graph (preview)** — required for the graph associated with ontology.
{: .prerequisite }

---

## Required for Data Agent integration

### 3. Data agent item types (preview)

Enable **Users can create and share Data agent item types (preview)**.
{: .prerequisite }

### 4. Copilot and Azure OpenAI Service

Enable all three settings:
{: .prerequisite }

- **Users can use Copilot and other features powered by Azure OpenAI**
- **Data sent to Azure OpenAI can be processed outside your capacity's geographic region**
- **Data sent to Azure OpenAI can be stored outside your capacity's geographic region**

### 5. XMLA endpoints

Enable **Power BI semantic models via XMLA endpoints** — required for semantic model data sources.
{: .prerequisite }

---

## Required for Operations Agent

### 6. Operations agent preview

Tenant admin must enable the **Operations agent preview** setting.
{: .prerequisite }

### 7. Copilot and Azure OpenAI Service

Same three settings as listed in the Data Agent section above.
{: .prerequisite }

### 8. Cross-geo AI (conditional)

If capacity is **not** in US or EU regions, also enable **cross-geo processing and storage for AI**.
{: .important }

---

## Workspace requirements

- A workspace with a **Microsoft Fabric-enabled capacity** (paid F2 or higher, or P1+ with Fabric enabled)

Trial capacities are **not supported** for operations agents.
{: .warning }
