---
title: Best Practices
layout: default
nav_order: 9
---

# Best Practices

Lessons learned, common pitfalls, and configuration guidance that Microsoft Learn doesn't cover.

---

## Tenant settings

### Do it once, do it right

Enable **all** Fabric IQ tenant settings at the same time. Enabling them piecemeal leads to confusing errors where one item works but its downstream dependency doesn't.

Recommended order:
1. Ontology + Graph
2. Copilot and Azure OpenAI (all three sub-settings)
3. XMLA endpoints
4. Data agent item types
5. Operations agent preview
{: .tip }

### Scope settings carefully

- Use **security groups** to limit tenant settings rather than enabling for the entire organization
- Create a dedicated security group (e.g., `FabricIQ-Preview-Users`) and enable settings only for that group
- This prevents accidental usage by teams who aren't ready and limits your blast radius during preview

Don't enable cross-geo AI processing unless your capacity is outside US/EU. Enabling it unnecessarily can raise data residency concerns with compliance teams.
{: .warning }

---

## Capacity and cost

### Right-size your capacity

| Workload | Minimum | Recommended |
|:---------|:--------|:------------|
| Ontology + Graph (dev/test) | F2 | F4 |
| Data agent (small team) | F2 | F4 |
| Operations agent | F2 (trial **not** supported) | F8+ |
| Full stack (ontology + both agents) | F4 | F16+ |

### Cost management

- **Pause capacity** when not actively developing — agents consume CUs even when idle if running
- **Stop operations agents** when not needed — they poll continuously
- Graph refreshes are **full refreshes** — batch your data changes and schedule refreshes during off-peak hours
- Monitor CU consumption in the Fabric Capacity Metrics app weekly during preview

Ontology graph refreshes can spike CU usage. Never schedule them during business hours when reports are in heavy use.
{: .warning }

---

## Ontology design

### Entity type guidelines

- **Start small**: 3-5 entity types max for your first ontology. You can always add more.
- **One ontology per domain**: Don't try to model the entire enterprise in one ontology. Start with one business domain (supply chain, finance, etc.)
- **Keys matter**: Choose stable, immutable keys. Natural keys (`orderId`, `sku`) are better than surrogate keys when possible.
- **Name for humans**: Entity types and properties should read like plain English. The data agent and operations agent both use these names to interpret questions.

### Data binding rules of thumb

- Always complete **static binding before time series** — this is a hard requirement
- Use your **Power BI semantic model** as the static binding source when possible — it's already curated
- Don't bind every column — only bind properties that are relevant to questions users will ask or rules agents will evaluate
- Keep time series bindings focused: 3-5 metrics per entity type, not 50

If your semantic model has calculated columns or complex DAX measures, they won't be available as binding targets. Only base columns from the underlying tables are bindable.
{: .important }

### Relationship design

- Relationships should answer a real business question ("Which carrier shipped this order?")
- Don't create relationships just because a foreign key exists — only model relationships the agents or graph queries will actually use
- Favor **one-to-many** over **many-to-many** where possible — graph traversals are simpler and faster

---

## Data agent

### Instruction quality = answer quality

The single biggest factor in data agent accuracy is **instruction quality**. Invest more time here than anywhere else.

- Write instructions as if you're onboarding a new analyst who has never seen your data
- Include **every acronym and business term** your users might use
- Tell the agent what **not** to do — "Never join Sales to Inventory directly; always go through Product"
- Include edge cases — "When asked about revenue for the current month, note that data is only complete through yesterday"

### Data source selection

| Source type | Best for | Limitations |
|:------------|:---------|:------------|
| Power BI semantic model | KPI questions, dimensional analysis | No example queries, limited to DAX |
| Lakehouse | Granular transactional data, complex joins | Needs good table/column naming |
| Warehouse | Same as lakehouse, better for large scale | Same naming requirements |
| KQL database | Real-time or near-real-time questions | KQL syntax can be complex for agent |
| Ontology | Relationship-aware questions | Newer capability, less mature |

### Testing strategy

Before publishing, test with these question categories:

1. **Happy path**: Questions your instructions explicitly cover
2. **Ambiguous**: Questions with multiple valid interpretations ("What's our performance?")
3. **Out of scope**: Questions the agent shouldn't try to answer
4. **Edge cases**: Empty results, null values, future dates
5. **Cross-source**: Questions that span multiple data sources (if you have several)

If the agent generates incorrect DAX/SQL for a question pattern, add that exact pattern to your instructions with the correct table/column references.
{: .tip }

---

## Operations agent

### Goal and instruction writing

- Goals should be **specific and measurable**: "Monitor bike dock availability and alert when any station drops below 20% capacity" not "Watch the bikes"
- Instructions should include **thresholds**: "A critical alert means occupancy above 95%. A warning means above 80%."
- Include **who should be notified** for different severity levels
- Specify **what actions are appropriate** and **what actions should never be taken** without human approval

### Playbook review checklist

After the agent generates its playbook, verify:

- [ ] Entity names map to the right underlying tables
- [ ] Property names match your ontology (not raw column names)
- [ ] Rules have the correct thresholds (the agent may hallucinate values)
- [ ] Actions reference the correct Power Automate flows
- [ ] Autonomous rules (if any) are genuinely safe to run without approval

Never enable autonomous rules in production without thorough testing in a dev workspace first. Once the agent can act without approval, there's no undo.
{: .warning }

### Teams integration

- Recipients must have **write permissions** on the agent item — read-only won't receive messages
- Test the Teams app in a small group before rolling out to the full team
- The agent uses the **creator's identity** for all actions — make sure the creator account has appropriate permissions to the downstream systems (ERP, CRM, etc.)
- Set up a **dedicated service account** as the agent creator if the original creator might leave the organization

---

## Security and governance

### Permission model

| Layer | Who controls it | What it governs |
|:------|:----------------|:----------------|
| Tenant settings | Fabric admin | Which features are available |
| Workspace roles | Workspace admin | Who can create/edit items |
| Data source permissions | Data owner | Who can read the underlying data |
| Agent sharing | Agent creator | Who can use the published agent |
| Operations agent identity | Agent creator | Whose permissions the agent uses for actions |

### Audit and compliance

- Data agents generate **transparency steps** showing the SQL/DAX/KQL they executed — review these regularly
- Operations agent actions are logged — set up alerts for rejected recommendations (high rejection rates mean the rules need tuning)
- All data access flows through **Microsoft Entra ID** — existing conditional access policies apply
- The ontology doesn't duplicate data — it's a semantic layer on top of OneLake. Data residency is governed by the underlying storage location.
