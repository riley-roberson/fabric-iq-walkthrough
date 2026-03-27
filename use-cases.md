---
title: Use Cases
layout: default
nav_order: 11
---

# Real Use Cases

Practical scenarios where the Fabric IQ stack delivers value — organized by industry and business function.

---

## Supply Chain & Logistics

### Shipment breach detection (Operations Agent)

A logistics company monitors shipment GPS and temperature data streaming into an eventhouse. The operations agent watches for:
- Deliveries exceeding SLA time windows
- Cold-chain temperature breaches on perishable goods
- Route deviations beyond a threshold distance

When a breach is detected, the agent sends a Teams alert to the dispatch manager with the shipment ID, breach type, and a recommended action (reroute, escalate to carrier, notify customer). The manager approves with one click.

**Stack:** Eventhouse → Ontology (Shipment, Route, Carrier entities) → Operations Agent → Power Automate → CRM ticket

### Inventory reorder (Data Agent + Operations Agent)

A retail chain uses the data agent for ad-hoc questions like "Which stores have less than 2 weeks of inventory for SKU X?" while the operations agent continuously monitors stock levels and auto-triggers purchase orders when thresholds are crossed.

**Stack:** Lakehouse + Eventhouse → Ontology (Product, Store, Supplier) → Data Agent (Q&A) + Operations Agent (auto-reorder)

---

## Financial Services

### Regulatory compliance monitoring (Operations Agent)

A bank streams transaction data into KQL databases. The ontology defines entities for Account, Transaction, Customer, and RegulatoryRule. The operations agent monitors for:
- Transactions exceeding reporting thresholds
- Unusual patterns across linked accounts
- Missing KYC documentation approaching deadlines

**Stack:** Eventhouse → Ontology (Account, Transaction, RegulatoryRule) → Operations Agent → Teams alerts + compliance workflow

### Portfolio performance Q&A (Data Agent)

Portfolio managers ask natural language questions against a Power BI semantic model containing fund performance, benchmark comparisons, and risk metrics. The ontology ensures terms like "alpha," "Sharpe ratio," and "drawdown" map consistently across all data sources.

**Stack:** Power BI semantic model → Ontology (Fund, Benchmark, RiskMetric) → Data Agent

---

## Manufacturing & IoT

### Predictive maintenance alerts (Operations Agent)

A factory streams sensor telemetry (vibration, temperature, pressure) from production equipment into an eventhouse. The ontology models Machine, Sensor, MaintenanceSchedule, and Part entities. The operations agent detects anomalies and recommends maintenance before failures occur.

**Stack:** Eventhouse (sensor streams) → Ontology (Machine, Sensor, Part) → Operations Agent → Power Automate → maintenance ticket in ERP

### Production line Q&A (Data Agent)

Plant managers ask questions like "What was the average cycle time on Line 3 last week?" or "Which shifts had the highest defect rate in March?" The data agent queries both the lakehouse (historical) and semantic model (KPI calculations).

**Stack:** Lakehouse + Power BI semantic model → Ontology (ProductionLine, Shift, QualityMetric) → Data Agent

---

## Healthcare

### Patient flow monitoring (Operations Agent)

A hospital monitors real-time bed occupancy, ER wait times, and discharge readiness. The operations agent alerts charge nurses when:
- ER wait time exceeds 45 minutes
- ICU occupancy crosses 90%
- Discharge paperwork is pending beyond 2 hours

**Stack:** Eventhouse (ADT feeds) → Ontology (Patient, Bed, Department, Staff) → Operations Agent → Teams alerts

### Clinical operations reporting (Data Agent)

Department heads ask questions like "What's our 30-day readmission rate for cardiac patients?" or "How many surgeries were postponed last quarter and why?" The data agent queries the warehouse and semantic model with ontology-grounded terminology.

**Stack:** Warehouse + Power BI semantic model → Ontology (Patient, Procedure, Department) → Data Agent

---

## Retail & E-Commerce

### Pricing and promotion impact (Data Agent)

Merchandising teams ask "What was the revenue lift from the Black Friday promotion on electronics?" or "Which categories have the highest markdown rate this quarter?" The semantic model holds the curated KPIs; the ontology ensures "markdown," "lift," and "category" mean the same thing everywhere.

**Stack:** Power BI semantic model → Ontology (Product, Promotion, Category) → Data Agent

### Fraud and chargeback detection (Operations Agent)

An e-commerce platform monitors transaction streams for chargeback patterns, velocity spikes, and mismatched shipping addresses. The operations agent flags suspicious orders and recommends holds, sending approval requests to the fraud team via Teams.

**Stack:** Eventhouse → Ontology (Order, Customer, PaymentMethod) → Operations Agent → Teams approval → order management system

---

## Cross-Industry Patterns

| Pattern | Agent type | Key signal |
|:--------|:-----------|:-----------|
| **Threshold breach** → alert + action | Operations | A metric crosses a defined boundary |
| **Anomaly detection** → investigate | Operations | Unexpected deviation from baseline |
| **Ad-hoc Q&A** → structured answer | Data | User asks a natural language question |
| **Multi-source exploration** → insight | Data | Question spans warehouse + semantic model + ontology |
| **Continuous monitor + human approval** | Operations | Agent recommends, human confirms |
| **Self-service analytics** → no SQL needed | Data | Business users query without technical skills |

The ontology is the common thread in every use case — it ensures that "customer," "shipment," or "defect" means the same thing whether a human asks a question or an agent triggers an action.
{: .important }
