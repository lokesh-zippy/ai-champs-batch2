# Module 09 — Studio-Style Workflows for Business and Engineering Collaboration

## Overview

Module 09 is the **enterprise workflow-design layer** that sits alongside the Copilot and MCP engineering workflows built earlier. It covers how non-code and semi-technical users — program managers, product owners, TPOs, RTEs, Scrum roles — shape agent behaviour through **studio-style workflows in Atlassian Rovo Studio**: authoring agents, grounding them on Jira and Confluence, wiring triggers and actions, adding approval checkpoints, and handing finished workflows to technical owners without bypassing governance.

**Duration:** 2 hours (hands-on lab format)
**Delivered for:** Honeywell Engineering Teams — design/collaboration emphasis for Product Owners, Designers, TPOs, RTEs, and Scrum roles

> **Source note.** This guide is grounded in the **course outline** (`courseOutline/NIIT_Honeywell_AI_Champions_GitHub_AgentPrism (Software_Engineering).pdf`), section "9. Studio-Style Workflows for Business and Engineering Collaboration". A dedicated presentation deck was not available when this guide was written; the agenda and timings below are derived from the outline's sub-topic list and total duration.

## Quick Start

Module 09 moves from "what a studio workflow is" to a governed, handed-off workflow:

| # | Topic | Time | What You'll Do |
|---|-------|------|----------------|
| 01 | Studio Workflows vs Engineering Agents | 15 min | Where the boundary sits, and why business roles author here rather than in Copilot |
| 02 | Grounding Agents on Jira, Confluence &amp; Connected Sources | 20 min | Give an agent a permission-aware view of real project data |
| 03 | Triggers, Actions &amp; Connectors | 20 min | The trigger that starts a workflow and the steps it runs |
| 04 | Approval Flows &amp; Human-in-the-Loop Checkpoints | 20 min | Where a person confirms before the workflow proceeds |
| 05 | Permissions, Versioning &amp; Ownership | 20 min | Publishing a workflow the team can trust, and who owns it |
| 06 | Hands-On Lab: Author a Workflow, Hand It Over | 25 min | Build a program-reporting / triage / release workflow and hand it to a technical owner |

## Visual Summary

```mermaid
graph TB
    subgraph Foundation["FOUNDATION"]
        M1["01 · AI Champions Kick-off"]
        M2["02 · GitHub Copilot Enterprise"]
        M3["03 · Prompt &amp; Context Eng."]
    end
    subgraph BuildValidate["BUILD &amp; VALIDATE"]
        M4["04 · Spec-Driven Development"]
        M5["05 · SDD to Complete SDLC"]
        M6["06 · Test Strategy &amp; Testcontainers"]
        M7["07 · MCP-Enabled Agentic Eng."]
        M8["08 · PR Automation &amp; LLM-as-Judge"]
    end
    subgraph ScaleGovern["SCALE &amp; GOVERN"]
        M9["09 · Studio Workflows"]
        M10["10 · Agent Prism Observability"]
        M11["11 · ROI &amp; Token Economics"]
    end
    subgraph Cap["CAPSTONE"]
        M12["12 · Capstone &amp; Adoption Roadmap"]
    end

    M1 --> M2 --> M3 --> M4 --> M5 --> M6 --> M7 --> M8 --> M9 --> M10 --> M11 --> M12

    style Foundation fill:#1565c0,stroke:#0d47a1,color:#fff
    style BuildValidate fill:#2e7d32,stroke:#1b5e20,color:#fff
    style ScaleGovern fill:#e65100,stroke:#bf360c,color:#fff
    style Cap fill:#ad1457,stroke:#880e4f,color:#fff
    style M1 fill:#bbdefb,stroke:#1565c0,color:#0d47a1
    style M2 fill:#bbdefb,stroke:#1565c0,color:#0d47a1
    style M3 fill:#bbdefb,stroke:#1565c0,color:#0d47a1
    style M4 fill:#c8e6c9,stroke:#2e7d32,color:#1b5e20
    style M5 fill:#c8e6c9,stroke:#2e7d32,color:#1b5e20
    style M6 fill:#c8e6c9,stroke:#2e7d32,color:#1b5e20
    style M7 fill:#c8e6c9,stroke:#2e7d32,color:#1b5e20
    style M8 fill:#c8e6c9,stroke:#2e7d32,color:#1b5e20
    style M9 fill:#f9a825,stroke:#f57f17,color:#1a1a1a
    style M10 fill:#ffe0b2,stroke:#e65100,color:#bf360c
    style M11 fill:#ffe0b2,stroke:#e65100,color:#bf360c
    style M12 fill:#f8bbd0,stroke:#ad1457,color:#880e4f
```

Module 09 is grounded on the same governed MCP connections built in Module 07. Module 10's Agent Prism monitors the workflows authored here the same way it monitors engineering agents.

## Architecture

### Module 09 Agenda (120 minutes, derived from the course outline)

| # | Topic | Time |
|---|-------|------|
| 01 | Studio Workflows vs Engineering Agents | 15 min |
| 02 | Grounding Agents on Jira, Confluence &amp; Connected Sources | 20 min |
| 03 | Triggers, Actions &amp; Connectors | 20 min |
| 04 | Approval Flows &amp; Human-in-the-Loop Checkpoints | 20 min |
| 05 | Permissions, Versioning &amp; Ownership | 20 min |
| 06 | Hands-On Lab: Author a Workflow, Hand It Over | 25 min |

### Where a Studio Workflow Sits

Business intent enters at the top; an engineering agent executes at the bottom. The studio workflow is the governed translation layer in between — and there is a deliberate boundary where a business-authored workflow hands off to a technical owner.

```mermaid
graph TB
    BI[Business intent<br/>program reporting · issue triage · release coordination]
    SW[Studio workflow<br/>Atlassian Rovo Studio]
    BND{{Governance boundary<br/>business author → technical owner}}
    EA[Engineering agent<br/>Copilot / MCP workflows from Modules 02–08]

    BI --> SW --> BND --> EA

    style BI fill:#6a1b9a,stroke:#4a148c,color:#fff
    style SW fill:#e65100,stroke:#bf360c,color:#fff
    style BND fill:#f9a825,stroke:#f57f17,color:#1a1a1a
    style EA fill:#2e7d32,stroke:#1b5e20,color:#fff
```

**Why business roles author here, not in Copilot:** a studio workflow is about *coordinating* engineering work — triage, reporting, release steps — not writing code. It reads project data, applies rules, and routes tasks; it does not open a PR itself.

### Anatomy of a Studio Workflow

```mermaid
graph LR
    T[Trigger<br/>Jira event · schedule · manual] --> G[Grounding<br/>Jira issues · Confluence docs]
    G --> A[Actions &amp; Connectors<br/>update issue · post summary · notify]
    A --> CP{Approval<br/>checkpoint}
    CP -->|approved| O[Output<br/>report · triaged backlog · release note]
    CP -->|rejected| A

    style T fill:#1565c0,stroke:#0d47a1,color:#fff
    style G fill:#2e7d32,stroke:#1b5e20,color:#fff
    style A fill:#6a1b9a,stroke:#4a148c,color:#fff
    style CP fill:#c62828,stroke:#b71c1c,color:#fff
    style O fill:#f9a825,stroke:#f57f17,color:#1a1a1a
```

| Element | What It Is |
|---------|-----------|
| **Trigger** | The event that starts the workflow — a Jira transition, a schedule, or a manual run |
| **Grounding** | The permission-aware view of project data the agent reasons over — Jira issues, Confluence design and release docs, other connected engineering sources |
| **Actions &amp; connectors** | The steps the workflow performs — updating an issue, posting a summary, calling a connected system |
| **Approval flow / human-in-the-loop checkpoint** | A point where a person must confirm before the workflow proceeds |
| **Output** | The deliverable — a program report, a triaged backlog, a release coordination summary |

### Permissions, Versioning &amp; Ownership

A published workflow is a shared asset, and it carries governance with it:

| Concern | How Module 09 handles it |
|---------|--------------------------|
| **Permission-aware access** | The workflow can only see and act on project data the running user (or service identity) is entitled to |
| **Business/technical collaboration** | Business users author and test; technical owners review and take implementation ownership — without the business user bypassing governance |
| **Versioning** | Published workflows are versioned, so a change is reviewable and reversible |
| **Ownership** | Every published workflow has a named owner accountable for it |

### Hands-On Lab: Author a Workflow, Hand It Over

Program and product users define an agent-assisted workflow in Atlassian Rovo Studio for **program reporting, issue triage, or release coordination**:

```mermaid
graph LR
    A[Ground on a sample<br/>Jira project + Confluence space] --> B[Configure the trigger,<br/>actions &amp; steps]
    B --> C[Test against<br/>representative project data]
    C --> D[Review permission &amp;<br/>governance implications]
    D --> E[Hand over to<br/>technical owners]

    style A fill:#1565c0,stroke:#0d47a1,color:#fff
    style B fill:#2e7d32,stroke:#1b5e20,color:#fff
    style C fill:#6a1b9a,stroke:#4a148c,color:#fff
    style D fill:#e65100,stroke:#bf360c,color:#fff
    style E fill:#f9a825,stroke:#f57f17,color:#1a1a1a
```

### Module 09 Outcomes

- The boundary between a studio workflow and an engineering agent is understood
- An agent has been grounded on a permission-aware view of Jira and Confluence data
- A trigger, actions, and steps have been configured in Atlassian Rovo Studio
- Approval flows and human-in-the-loop checkpoints have been placed appropriately
- Permission, versioning, and ownership implications of a published workflow are understood
- A workflow has been tested against representative data and handed to a technical owner

### Key Terminology

| Term | Definition |
|------|------------|
| **Studio-style workflow** | An agent-assisted workflow authored by a non-technical user in a visual studio, coordinating engineering or program work |
| **Atlassian Rovo Studio** | Atlassian's environment for authoring, testing, and publishing Rovo agents and workflows |
| **Grounding** | Giving an agent a permission-aware view of specific project data (Jira issues, Confluence pages) to reason over |
| **Trigger** | The event that starts a workflow — a Jira event, a schedule, or a manual invocation |
| **Connector** | An integration that lets a workflow read from or act on a connected system |
| **Human-in-the-loop checkpoint** | A required human confirmation before a workflow step proceeds |
| **Approval flow** | The routing and sign-off logic around sensitive workflow actions |
| **Permission-aware access** | The workflow's data visibility is bounded by the running identity's entitlements |
| **Workflow ownership** | The named person accountable for a published workflow's behaviour and maintenance |
| **Governance boundary** | The handoff point where a business-authored workflow is taken over by a technical owner for implementation |

## References

### Programme Materials

- Course Outline (primary source): `courseOutline/NIIT_Honeywell_AI_Champions_GitHub_AgentPrism (Software_Engineering).pdf`, section 9
- Phase context: [`build-validate-phase-modules-05-08.md`](./build-validate-phase-modules-05-08.md) — the engineering workflows this layer sits alongside
- Module 07 guide: [`module-07-mcp-agentic-workflows.md`](./module-07-mcp-agentic-workflows.md) — the governed connections Module 09 is grounded on

### Further Reading — External References

*Every link below was fetched and confirmed (HTTP 200) on 2026-09-02.*

**Atlassian Rovo and Rovo Studio**
- [Atlassian Rovo](https://www.atlassian.com/software/rovo) — the product overview: agents, search, and chat grounded on connected work data.
- [Rovo support documentation](https://support.atlassian.com/rovo/) — the canonical docs root for setting up and using Rovo.
- [What is Rovo Studio](https://support.atlassian.com/rovo/docs/what-is-rovo-studio/) — the environment for building and managing agents and workflows.
- [Rovo resources](https://support.atlassian.com/rovo/resources/) — templates, patterns, and getting-started material.

**Grounding data and automation**
- [Atlassian Confluence](https://www.atlassian.com/software/confluence) — the design and release documentation a workflow is grounded on.
- [Jira automation](https://www.atlassian.com/software/jira/features/automation) — triggers, conditions, and actions — the no-code automation model studio workflows extend.

**Agentic workflow patterns and governance**
- [Anthropic — Building Effective Agents](https://www.anthropic.com/engineering/building-effective-agents) — human-in-the-loop checkpoints, routing, and orchestration patterns.
- [Model Context Protocol](https://modelcontextprotocol.io/) — the governed connection layer (Module 07) that studio workflows are grounded on.

### Previous Module

Module 08 — PR Process Automation, Quality Gates and LLM-as-Judge (1.5 hours). AI-assisted PR review against spec compliance, standards, security, and test evidence, plus LLM-as-Judge scoring and human escalation.

### Next Module

Module 10 — Agent Prism for Monitoring, Observability and Enterprise Control (1.5 hours). Monitoring agent behaviour, traces, failures, drift, token usage, and cost — and connecting that telemetry to engineering outcomes.
