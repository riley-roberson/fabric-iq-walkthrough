---
title: "Phase 4: Operations Agent"
layout: default
nav_order: 6
---

# Phase 4: Operations Agent — Real-Time Autonomous Actions

The operations agent automates the **observe → analyze → decide → act** cycle. It continuously tracks key metrics, surfaces insights, and recommends targeted actions. Each agent is a dedicated Fabric item for a specific business process.

Source: [Operations agent](https://learn.microsoft.com/fabric/real-time-intelligence/operations-agent)
{: .note }

---

## Prerequisites

Before creating an operations agent, ensure the following:
{: .prerequisite }

- Workspace with Fabric-enabled capacity (trial **not** supported)
- An **eventhouse** in the workspace
- A **KQL database** in the eventhouse
- A **Teams account**
- Tenant admin: operations agent preview enabled + Copilot/Azure OpenAI enabled
- Non-US/EU capacity: also enable cross-geo processing and storage for AI

---

## Key terminology

| Term | Definition |
|:-----|:-----------|
| **Knowledge source** | Database connection the agent uses to find and monitor data |
| **Tool** | Built-in functionality (NL-to-KQL, anomaly detection, Teams/email messaging) |
| **Thread** | Conversation session between agent and user |
| **Playbook** | Agent's internal representation of entities, data, rules, and possible actions |
| **Entity** | Business objects the agent monitors (bikes, docking stations, flights) |
| **Instance** | Specific occurrences of an entity (Bike #0451, Flight MS1234) |
| **Rules** | Conditions or patterns in data the agent monitors before making recommendations |
| **Autonomous rules** | Rules with actions the agent can take without human confirmation |

---

## Step 4a: Create the operations agent

1. Fabric home → **...** → **Create**
2. In the **Real-Time Intelligence** section → select **operations agent**
3. Enter a name, select a workspace → **Create**

---

## Step 4b: Configure the agent (Agent Setup page)

### 1. Business goals

Define what the agent should focus on — helps the agent understand context and objectives.

### 2. Agent instructions

Provide specific guidance for the agent's behavior and decision-making.

### 3. Knowledge source

Select a relevant data source (KQL database) for the agent to analyze.

### 4. Actions

Define actions the agent can take:

1. Name the action and provide a description
2. List parameters the action requires
3. Configure each action:
   1. Select the action → **Configure custom action** pane
   2. Select workspace and **activator item** → create a connection
   3. Copy the **Connection string** → **Open flow builder**
   4. In Flow builder, paste the connection string → **Save**
   5. Use dynamic content from parameters in the flow

---

## Step 4c: Generate and review the playbook

After configuration, **Save** the agent to generate the playbook. The playbook outlines:

- Goals, instructions, data, and actions
- Properties mapped to underlying data fields
- Entities the agent monitors
- Rules/conditions it evaluates

Property names in rules may refer to the ontology property name rather than the underlying column. Confirm the model and rules match your requirements.
{: .important }

To adjust behavior: update goals or instructions → save again.

---

## Step 4d: Start the agent

- Select **Start** in the toolbar to activate the agent
- Select **Stop** to deactivate

---

## Step 4e: Receive messages in Teams

1. Install the **Fabric Operations Agent** Teams app (search in Teams app store if not auto-installed)
2. The agent sends Teams messages when data matches defined rules
3. Messages include: summary of insights + recommended actions
4. Update recipients in agent settings → **Agent behavior** (recipients must belong to your org and have write permissions on the agent item)

The agent operates using the **delegated identity and permissions of its creator**. When a recipient approves a recommendation, the agent executes on behalf of the creator.
{: .important }

---

## Step 4f: Respond to recommendations

When the agent recommends an action:

- You receive a Teams message with context about what triggered the condition
- Select **Yes** to approve or **No** to reject
- You can adjust parameter values before final approval
