---
title: "Phase 3: Data Agent"
layout: default
nav_order: 5
---

# Phase 3: Data Agent — Conversational Q&A

The data agent allows conversational Q&A over enterprise data using generative AI. Users ask questions in plain English and receive structured, secure, read-only answers.

Source: [Create a data agent](https://learn.microsoft.com/fabric/data-science/how-to-create-data-agent) · [Configure a data agent](https://learn.microsoft.com/fabric/data-science/data-agent-configurations)
{: .note }

---

## Prerequisites

Before creating a data agent, ensure the following:
{: .prerequisite }

- Paid F2+ capacity (or P1+ with Fabric enabled)
- Fabric data agent tenant settings enabled
- Cross-geo processing/storage for AI enabled
- Power BI semantic models via XMLA endpoints enabled
- At least one data source with data (warehouse, lakehouse, semantic model, KQL database, or ontology)

### Authentication

No Azure OpenAI key or token is needed — Fabric uses a Microsoft-managed Azure OpenAI Assistant. Data access runs under your **Microsoft Entra ID** identity and workspace/data permissions. Only **Read** permission is needed on Power BI semantic models (Write not required).

---

## Step 3a: Create the data agent

1. Navigate to workspace → **+ New Item** → search for **Fabric data agent**
2. Provide a name → proceed to configuration

---

## Step 3b: Add data sources

- Add up to **5 data sources** in any combination: lakehouses, warehouses, Power BI semantic models, KQL databases, ontologies
- OneLake catalog auto-appears; add each source individually
- In the **Explorer** pane, use checkboxes to include/exclude specific tables

Use descriptive names for tables and columns. `SalesData` is better than `TableA`. `ActiveCustomer` is clearer than `C1`. Descriptive names help the AI generate accurate queries.
{: .tip }

---

## Step 3c: Ask questions

The agent translates natural language to SQL, DAX, or KQL depending on the data source.

### Good questions (structured data retrieval)

- "What were our total sales in California in 2023?"
- "What are the top 5 products with the highest list prices?"
- "What are the most expensive items that have never been sold?"

### Out of scope (requires complex reasoning/causal inference)

- "Why is our factory productivity lower in Q2 2024?"
- "What is the root cause of our sales spike?"

---

## Step 3d: Configure the data agent

### Agent-level instructions (up to 15,000 characters)

Use this recommended format:

```markdown
## Objective
// Overall goal of the agent
// Example: "Help users analyze retail sales performance and customer behavior across regions."

## Data sources
// Which data sources to prioritize and when
// Example: "Use 'SalesLakehouse' for product/transaction data. Use 'CRMModel' for customer demographics."

## Key terminology
// Business terms and acronyms
// Example: "'GMV' refers to Gross Merchandise Value."

## Response guidelines
// Formatting and presentation expectations

## Handling common topics
// Special handling rules for frequent queries
```

### Data source-level instructions

```markdown
## General knowledge
// Background info for this specific data source

## Table descriptions
// Key tables and important columns

## When asked about
// Query-specific logic or table preferences
// Example: "When asked about shoe sales, always use the SalesProduct table."
```

### Data source descriptions

Summarize what the data source contains, what questions it answers, and business-specific nuances. The agent uses this to route questions to the right source.

### Example queries (few-shot learning)

- Supported for lakehouse, warehouse, and KQL databases
- Provide diverse question/SQL or question/KQL pairs
- Queries must contain valid syntax matching the selected table schema

Example queries are **not supported** for Power BI semantic models or ontologies.
{: .warning }

---

## Step 3e: Publish and share

1. Test across various questions
2. Confirm accurate SQL/DAX/KQL generation
3. Select **Publish** → provide a description
4. Two versions created: **draft** (continue refining) and **published** (shared with colleagues)
