---
title: "Phase 2: Ontology"
layout: default
nav_order: 4
---

# Phase 2: Ontology — Define Your Business Language

The ontology is the **core item** for defining a common language. It digitally represents the enterprise vocabulary and semantic layer that unifies meaning across domains and OneLake sources.

Source: [Ontology overview](https://learn.microsoft.com/fabric/iq/ontology/overview)
{: .note }

---

## Core concepts

| Concept | Definition |
|:--------|:-----------|
| **Entity type** | Reusable logical model of a real-world concept (Shipment, Product, Sensor). Standardizes name, description, identifiers, properties, constraints. |
| **Entity instance** | Concrete occurrence of an entity type, populated from data bindings (a semantic row). |
| **Property** | Named fact about an entity with a declared data type. Can contain bindings to source data and semantic annotations. |
| **Relationship** | Typed, directional link between entity types or instances. Can have attributes (distance, confidence) and cardinality rules. |
| **Data binding** | Connection from ontology definitions to concrete data in OneLake (lakehouse tables, eventhouse streams, Power BI semantic models). |
| **Ontology graph** | Queryable instance graph built from data bindings and relationship definitions. Nodes = entity instances, edges = links. |

---

## Step 2a: Create entity types

Source: [Create entity types](https://learn.microsoft.com/fabric/iq/ontology/how-to-create-entity-types)
{: .note }

1. Open your ontology item. Select **Add entity type** from the top ribbon.
2. Enter a name (1–26 chars, alphanumeric/hyphens/underscores, must start and end with alphanumeric).
3. In the **Entity type configuration** pane → **Properties** tab → **Add properties**.
4. For each property, define: **name**, **data type**, **property type**. Select **Save**.
5. Define the entity type **Key** — select one or more properties as the unique identifier.
6. Optionally set an **Instance display name** property for friendly names in downstream experiences.

Property names must be unique across all entity types if they share the same name but differ in type. Two entity types *can* share a property name if the data type is the same.
{: .tip }

---

## Step 2b: Bind data to entity types

Source: [Data binding](https://learn.microsoft.com/fabric/iq/ontology/how-to-bind-data)
{: .note }

Since you're starting from a **Power BI semantic model**, you can import entity types directly from it. For manual binding:

### Static data binding

1. Select entity type → **Bindings** tab → **Add data to entity type**
2. Select a OneLake data source → **Connect** → choose a table → **Next**
3. Set **Binding type** to **Static**
4. Under **Bind your properties**, select source columns and name each property
5. **Save** → verify in Bindings and Properties tabs
6. Set the **Key** (string/integer columns that uniquely identify a record)
7. Optionally set **Instance display name**

### Time series data binding

Static data binding must be complete first. The entity type must have at least one static-bound property for the key.
{: .prerequisite }

1. Add data → select source (OneLake or Eventhouse)
2. Set **Binding type** to **Timeseries** → select **Source data timestamp column**
3. In the **Static** section, bind key columns; in the **Timeseries** section, define new properties
4. **Save** → verify

### Data binding constraints

- Each entity type supports **one static** binding (must be OneLake-backed)
- Multiple **time series** bindings are supported (from eventhouse and lakehouse)

The following are **not supported**:
- Lakehouses with OneLake security enabled
- External (non-managed) lakehouse tables
- Delta tables with column mapping enabled
- Upstream data changes auto-propagating — **manual refresh** of the graph model is required
{: .warning }

---

## Step 2c: Create relationships

Define typed, directional links between entity types:

1. Select **Add relationship type** from the ribbon
2. Choose source and target entity types
3. Define the relationship name, direction, and cardinality
4. Optionally add attributes (distance, confidence, effectiveAt)

---

## Step 2d: Preview & explore the ontology graph

Source: [Preview experience](https://learn.microsoft.com/fabric/iq/ontology/how-to-use-preview-experience)
{: .note }

1. In the **Entity Types** pane → select an entity type → **Entity type overview**
2. The preview experience shows:
   - **Data tiles**: Line charts (time series), bar charts (static), graph views
   - **Entity instances table**: Browse specific instances
   - **Graph view**: Expand graph tiles for full graph exploration
3. **Query builder**: Craft custom queries with filters and components
4. **Open in Fabric Graph**: For complex queries and graph algorithms

### Refreshing the graph model

- Schema changes auto-refresh the graph
- Upstream data changes (new rows, updates) require **manual refresh**:
  1. Workspace → find the graph model → **...** → **Schedule**
  2. Select **Refresh now** (or configure a recurring schedule)

The graph performs a **full refresh** each time, so batch your changes to manage cost.
{: .important }
