---
title: Code Examples
layout: default
nav_order: 8
---

# Code Examples

Ready-to-use templates for agent instructions, DAX patterns, KQL queries, and Power Automate configurations. Copy, adapt, and deploy.

---

## Data Agent instruction templates

### Full agent-level instruction (production-ready)

```markdown
## Objective
Help supply chain analysts understand shipment performance, carrier reliability,
and delivery SLA compliance across all regions. Prioritize accuracy over speed —
always confirm the data source before answering.

## Data sources
- **ShipmentLakehouse**: Use for raw shipment records, carrier data, and route history.
  This is the source of truth for transactional data.
- **SupplyChainModel**: Use for pre-calculated KPIs like on-time delivery rate,
  average transit time, and cost per mile. Prefer this source for any KPI questions.
- **LogisticsOntology**: Use when questions involve relationships between entities
  (e.g., "Which carriers serve the Northeast region?").

## Key terminology
- "OTD" = On-Time Delivery rate (deliveries within SLA / total deliveries)
- "OTIF" = On-Time In-Full (delivered on time AND complete quantity)
- "Dwell time" = Time a shipment spends at a facility before moving
- "Last mile" = Final delivery leg from distribution center to customer
- "Breach" = Any SLA violation (time, temperature, quantity)

## Response guidelines
- Always include the time period in your answer (e.g., "In Q1 2025...")
- When showing top/bottom N results, default to 10 unless the user specifies otherwise
- Format currency as USD with commas (e.g., $1,234,567)
- Format percentages to one decimal place (e.g., 94.3%)
- If a question is ambiguous, ask for clarification before querying

## Handling common topics
- When asked about "performance", default to OTD rate unless specified otherwise
- When asked about "cost", use cost per mile from SupplyChainModel
- When comparing carriers, always include volume (shipment count) alongside metrics
- When asked about trends, show month-over-month for the last 12 months
```

### Data source-level instruction (for a Power BI semantic model)

```markdown
## General knowledge
This semantic model contains retail sales data for 150 stores across 12 regions.
Data refreshes daily at 6 AM UTC. Historical data goes back to January 2020.

## Table descriptions
- **Sales**: Fact table with TransactionID, StoreID, ProductID, SaleDate, Quantity,
  Revenue, Discount. One row per line item.
- **Product**: ProductID, ProductName, Category, SubCategory, Brand, UnitCost, ListPrice.
  Contains ~15,000 active SKUs.
- **Store**: StoreID, StoreName, Region, State, City, OpenDate, SquareFootage, StoreType
  (Flagship, Standard, Outlet).
- **Calendar**: Date dimension with FiscalYear, FiscalQuarter, FiscalMonth, WeekOfYear,
  IsHoliday, IsWeekend.
- **Customer**: CustomerID, Segment (Premium, Standard, New), LTV, FirstPurchaseDate.

## When asked about
- Revenue questions → use [Total Revenue] measure (already handles currency conversion)
- Margin questions → use [Gross Margin %] measure (Revenue - COGS) / Revenue
- Year-over-year → use [YoY Growth %] measure (handles fiscal year alignment)
- "Best sellers" → rank by Revenue unless user specifies Quantity
- "Store performance" → use [Revenue per SqFt] for fair comparisons across store sizes
```

### Data source description (for routing)

```markdown
SupplyChainWarehouse contains 3 years of shipment transaction data for all carriers
and routes. Use this source for questions about shipment details, carrier performance,
route optimization, and historical trend analysis. It includes tables for Shipments,
Carriers, Routes, Facilities, and SLA definitions. Best for granular queries that need
joins across multiple dimensions.
```

---

## DAX patterns for semantic models

### YoY growth measure

```dax
YoY Growth % =
VAR CurrentPeriod = [Total Revenue]
VAR PriorPeriod =
    CALCULATE(
        [Total Revenue],
        SAMEPERIODLASTYEAR('Calendar'[Date])
    )
RETURN
    IF(
        NOT ISBLANK(PriorPeriod),
        DIVIDE(CurrentPeriod - PriorPeriod, PriorPeriod)
    )
```

### Running total with reset

```dax
Running Total =
CALCULATE(
    [Total Revenue],
    FILTER(
        ALL('Calendar'[Date]),
        'Calendar'[Date] <= MAX('Calendar'[Date])
            && 'Calendar'[FiscalYear] = MAX('Calendar'[FiscalYear])
    )
)
```

### Dynamic top N with "Other" bucket

```dax
Top N Category Revenue =
VAR TopN = 5
VAR RankedCategories =
    ADDCOLUMNS(
        VALUES('Product'[Category]),
        "@Rev", [Total Revenue]
    )
VAR TopCategories =
    TOPN(TopN, RankedCategories, [@Rev], DESC)
VAR CurrentCategory = SELECTEDVALUE('Product'[Category])
RETURN
    IF(
        CurrentCategory IN TopCategories,
        [Total Revenue],
        CALCULATE([Total Revenue], REMOVEFILTERS('Product'[Category]))
            - SUMX(TopCategories, [@Rev])
    )
```

These DAX patterns work well as semantic model measures that the data agent can query via NL2DAX. Name measures descriptively — the agent uses measure names to decide which one to call.
{: .tip }

---

## KQL queries for operations agent knowledge sources

### Anomaly detection on time series

```kql
SensorReadings
| where Timestamp > ago(7d)
| summarize AvgTemp = avg(Temperature), StdTemp = stdev(Temperature) by bin(Timestamp, 1h), MachineId
| extend UpperBound = AvgTemp + (3 * StdTemp),
         LowerBound = AvgTemp - (3 * StdTemp)
| join kind=inner (
    SensorReadings
    | where Timestamp > ago(1h)
) on MachineId
| where Temperature > UpperBound or Temperature < LowerBound
| project MachineId, Timestamp, Temperature, UpperBound, LowerBound
```

### SLA breach detection

```kql
Shipments
| where Status == "Delivered"
| extend TransitHours = datetime_diff('hour', DeliveryTime, PickupTime)
| extend SLAHours = case(
    ServiceLevel == "Express", 24,
    ServiceLevel == "Standard", 72,
    ServiceLevel == "Economy", 168,
    120)
| extend Breached = TransitHours > SLAHours
| where Breached == true
| summarize BreachCount = count(), AvgOverage = avg(TransitHours - SLAHours) by CarrierId, ServiceLevel
| order by BreachCount desc
```

### Rolling window aggregation

```kql
Transactions
| where EventTime > ago(30d)
| summarize
    DailyRevenue = sum(Amount),
    DailyOrders = count(),
    DailyAvgOrder = avg(Amount)
    by bin(EventTime, 1d), Region
| extend
    Rolling7dRevenue = series_fir(DailyRevenue, repeat(1, 7)),
    Rolling7dOrders = series_fir(DailyOrders, repeat(1, 7))
```

---

## Power Automate flow skeleton for operations agent actions

### Teams approval + ERP update

```json
{
  "trigger": "When an Operations Agent action is invoked",
  "connection_string": "<paste from agent action config>",
  "steps": [
    {
      "action": "Post adaptive card and wait for response",
      "channel": "Operations Alerts",
      "card_body": "Agent recommends: {{action_description}}",
      "parameters_shown": ["entity_id", "recommended_value", "current_value"]
    },
    {
      "condition": "If approved",
      "true_branch": [
        {
          "action": "HTTP request to ERP API",
          "method": "POST",
          "url": "https://erp.contoso.com/api/v2/{{entity_type}}/{{entity_id}}",
          "body": {
            "field": "{{parameter_name}}",
            "value": "{{approved_value}}"
          }
        },
        {
          "action": "Post message to Teams",
          "message": "Action completed: {{action_description}} for {{entity_id}}"
        }
      ],
      "false_branch": [
        {
          "action": "Post message to Teams",
          "message": "Action rejected by {{responder_name}}: {{action_description}}"
        }
      ]
    }
  ]
}
```

This is a logical skeleton — adapt it in the Power Automate visual designer. The actual connector actions will vary by your ERP/CRM system.
{: .note }

---

## Ontology entity type patterns

### Naming conventions

```
Entity types:     PascalCase, singular    → Shipment, ProductCategory, SensorReading
Properties:       camelCase               → deliveryDate, unitPrice, isActive
Relationship:     verb phrase             → "ships to", "belongs to", "monitors"
Keys:             end with Id or Code     → shipmentId, carrierCode, skuId
```

### Common entity type templates

```yaml
# Supply chain
Shipment:
  key: shipmentId (string)
  properties:
    - origin (string)
    - destination (string)
    - carrier (string)
    - pickupDate (datetime)
    - deliveryDate (datetime)
    - status (string)
    - weight (decimal)
  relationships:
    - "shipped by" → Carrier
    - "contains" → Product

# IoT / Manufacturing
Machine:
  key: machineId (string)
  properties:
    - machineName (string)
    - location (string)
    - installDate (datetime)
    - status (string)
    - lastMaintenanceDate (datetime)
  timeseries:
    - temperature (decimal)
    - vibration (decimal)
    - pressure (decimal)
  relationships:
    - "located in" → Facility
    - "maintained by" → Technician
```
