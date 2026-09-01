# Module 01 — AI Champions Kick-off and Enterprise Context

## Overview

Module 01 frames **why Honeywell software engineering teams need AI Champions**, maps today's delivery pain points, and introduces the AI-assisted engineering lifecycle the programme builds toward. It is the foundation module — every subsequent module assumes the shared vocabulary, role expectations, and success metrics established here.

**Duration:** 2 hours (workshop format)
**Delivered for:** Honeywell Engineering Teams

## Quick Start

The programme drives a shift from **isolated AI experimentation** to **disciplined enterprise adoption**:

| Today | Target |
|-------|--------|
| Individual engineers try Copilot ad hoc, no shared workflow | Common AI-assisted engineering lifecycle, adopted by role |
| Prompt-only development, inconsistent results | Specification-led development, validated by test strategy |
| No visibility into token usage, AI cost, or output quality | Agent Prism tracks traces, cost, and quality from day one |
| Brownfield changes risk violating architecture unnoticed | Change-impact analysis protects patterns and contracts |
| AI adoption measured by anecdote | Adoption measured against baselined cycle-time and quality metrics |

## Visual Summary

```mermaid
graph TB
    subgraph Foundation["FOUNDATION"]
        M1["Module 01<br/>AI Champions Kick-off"]
        M2["Module 02<br/>GitHub Copilot Enterprise"]
        M3["Module 03<br/>Prompt & Context Eng."]
    end

    subgraph BuildValidate["BUILD & VALIDATE"]
        M4["Module 04<br/>Spec-Driven Development"]
        M5["Module 05<br/>SDD to Complete SDLC"]
        M6["Module 06<br/>Test Strategy & Testcontainers"]
    end

    subgraph ScaleGovern["SCALE & GOVERN"]
        M7["Module 07<br/>MCP-Enabled Agentic Eng."]
        M8["Module 08<br/>PR Automation & LLM-as-Judge"]
        M9["Module 09<br/>Studio Workflows"]
        M10["Module 10<br/>Agent Prism Observability"]
    end

    subgraph Capstone["CAPSTONE"]
        M11["Module 11<br/>ROI & Token Economics"]
        M12["Module 12<br/>Capstone & Adoption Roadmap"]
    end

    M1 --> M2 --> M3 --> M4 --> M5 --> M6 --> M7 --> M8 --> M9 --> M10 --> M11 --> M12

    style Foundation fill:#1565c0,stroke:#0d47a1,color:#fff
    style BuildValidate fill:#2e7d32,stroke:#1b5e20,color:#fff
    style ScaleGovern fill:#e65100,stroke:#bf360c,color:#fff
    style Capstone fill:#ad1457,stroke:#880e4f,color:#fff
    style M1 fill:#bbdefb,stroke:#1565c0,color:#0d47a1
    style M2 fill:#bbdefb,stroke:#1565c0,color:#0d47a1
    style M3 fill:#bbdefb,stroke:#1565c0,color:#0d47a1
    style M4 fill:#c8e6c9,stroke:#2e7d32,color:#1b5e20
    style M5 fill:#c8e6c9,stroke:#2e7d32,color:#1b5e20
    style M6 fill:#c8e6c9,stroke:#2e7d32,color:#1b5e20
    style M7 fill:#ffe0b2,stroke:#e65100,color:#bf360c
    style M8 fill:#ffe0b2,stroke:#e65100,color:#bf360c
    style M9 fill:#ffe0b2,stroke:#e65100,color:#bf360c
    style M10 fill:#ffe0b2,stroke:#e65100,color:#bf360c
    style M11 fill:#f8bbd0,stroke:#ad1457,color:#880e4f
    style M12 fill:#f8bbd0,stroke:#ad1457,color:#880e4f
```

## Architecture

### Module 01 Agenda (120 minutes)

| # | Topic | Time |
|---|-------|------|
| 01 | Why AI Champions, Why Now | 15 min |
| 02 | The AI Champion Role, By Function | 15 min |
| 03 | Risks of Unmanaged AI & Agent Usage | 15 min |
| 04 | The AI-Assisted Software Engineering Lifecycle | 20 min |
| 05 | Agent Prism: The Monitoring Lens | 15 min |
| 06 | Workshop: Current-State vs Target-State Workflow | 40 min |

### The 12-Module Journey

This programme is an incremental engineering journey, not a collection of AI tool topics.

```
FOUNDATION              BUILD & VALIDATE          SCALE & GOVERN              CAPSTONE
─────────────           ─────────────────         ──────────────              ────────
1  AI Champions         4  SDD                    7  MCP-Enabled              11 ROI & Token
   Kick-off             5  SDD to SDLC               Agentic Eng.               Economics
2  GitHub Copilot       6  Test Strategy          8  PR Automation           12 Capstone &
   Enterprise              & Testcontainers          & LLM-as-Judge              Adoption
3  Prompt &                                                    9  Studio Workflows
   Context Eng.                                                 10 Agent Prism
```

Every later module assumes the shared vocabulary, role expectations, and success metrics this session establishes — from Copilot foundations through SDD, testing, MCP, PR quality, observability, and the final capstone.

### The AI-Assisted Software Engineering Lifecycle

The incremental journey the next eleven modules build, one capability at a time:

```mermaid
graph LR
    subgraph Stage1["Copilot Foundations"]
        A1[Chat & Inline Assist]
        A2[Agent Mode]
        A3[Repo-Aware Workflows]
    end

    subgraph Stage2["Context Engineering"]
        B1[Select Context]
        B2[Structure Context]
        B3[Compress Context]
    end

    subgraph Stage3["Spec-Driven Development"]
        C1[Specification]
        C2[Architecture & Plan]
        C3[Tasks & Acceptance Criteria]
    end

    subgraph Stage4["Test & PR Quality"]
        D1[Testcontainers Strategy]
        D2[AI-Assisted PR Review]
        D3[LLM-as-Judge Gates]
    end

    subgraph Stage5["MCP-Enabled Workflows"]
        E1[Approved Repositories]
        E2[APIs & Enterprise Workflows]
        E3[Governance & Permissions]
    end

    subgraph Stage6["Observe, Measure, Scale"]
        F1[Agent Prism Traces]
        F2[ROI & Token Economics]
        F3[Adoption Roadmap]
    end

    Stage1 --> Stage2 --> Stage3 --> Stage4 --> Stage5 --> Stage6

    style Stage1 fill:#1565c0,stroke:#0d47a1,color:#fff
    style Stage2 fill:#6a1b9a,stroke:#4a148c,color:#fff
    style Stage3 fill:#2e7d32,stroke:#1b5e20,color:#fff
    style Stage4 fill:#e65100,stroke:#bf360c,color:#fff
    style Stage5 fill:#c62828,stroke:#b71c1c,color:#fff
    style Stage6 fill:#f9a825,stroke:#f57f17,color:#1a1a1a
    style A1 fill:#bbdefb,stroke:#1565c0,color:#0d47a1
    style A2 fill:#bbdefb,stroke:#1565c0,color:#0d47a1
    style A3 fill:#bbdefb,stroke:#1565c0,color:#0d47a1
    style B1 fill:#e1bee7,stroke:#6a1b9a,color:#4a148c
    style B2 fill:#e1bee7,stroke:#6a1b9a,color:#4a148c
    style B3 fill:#e1bee7,stroke:#6a1b9a,color:#4a148c
    style C1 fill:#c8e6c9,stroke:#2e7d32,color:#1b5e20
    style C2 fill:#c8e6c9,stroke:#2e7d32,color:#1b5e20
    style C3 fill:#c8e6c9,stroke:#2e7d32,color:#1b5e20
    style D1 fill:#ffe0b2,stroke:#e65100,color:#bf360c
    style D2 fill:#ffe0b2,stroke:#e65100,color:#bf360c
    style D3 fill:#ffe0b2,stroke:#e65100,color:#bf360c
    style E1 fill:#ffcdd2,stroke:#c62828,color:#b71c1c
    style E2 fill:#ffcdd2,stroke:#c62828,color:#b71c1c
    style E3 fill:#ffcdd2,stroke:#c62828,color:#b71c1c
    style F1 fill:#fff9c4,stroke:#f9a825,color:#f57f17
    style F2 fill:#fff9c4,stroke:#f9a825,color:#f57f17
    style F3 fill:#fff9c4,stroke:#f9a825,color:#f57f17
```

**The throughline:** Every stage is measured against the same success metrics — implementation cycle time, quality/regression risk, PR review time, testing effort, token usage, and AI cost.

### Greenfield vs Brownfield Paths

```mermaid
graph TB
    Start([Engineering Request]) --> Decision{Greenfield or Brownfield?}

    Decision -->|Greenfield| GF[New Feature / App]
    Decision -->|Brownfield| BF[Enhance Existing System]

    GF --> GF1[Problem Framing & Requirements]
    GF1 --> GF2[Specification via SDD]
    GF2 --> GF3[Architecture & Plan]
    GF3 --> GF4[Implementation with Copilot]
    GF4 --> GF5[Testcontainers Strategy]
    GF5 --> GF6[PR Review & LLM-as-Judge]
    GF6 --> GF7[CI/CD & Release]

    BF --> BF1[Repository & Architecture Analysis]
    BF1 --> BF2[Dependency & Change-Impact Analysis]
    BF2 --> BF3[Preserve Design Principles & Patterns]
    BF3 --> BF4[Controlled Implementation]
    BF4 --> BF5[Regression Protection]
    BF5 --> BF6[Safe Evolution & Release]

    style Start fill:#1565c0,stroke:#0d47a1,color:#fff
    style Decision fill:#f9a825,stroke:#f57f17,color:#1a1a1a
    style GF fill:#2e7d32,stroke:#1b5e20,color:#fff
    style BF fill:#c62828,stroke:#b71c1c,color:#fff
    style GF1 fill:#c8e6c9,stroke:#2e7d32,color:#1b5e20
    style GF2 fill:#c8e6c9,stroke:#2e7d32,color:#1b5e20
    style GF3 fill:#c8e6c9,stroke:#2e7d32,color:#1b5e20
    style GF4 fill:#c8e6c9,stroke:#2e7d32,color:#1b5e20
    style GF5 fill:#c8e6c9,stroke:#2e7d32,color:#1b5e20
    style GF6 fill:#c8e6c9,stroke:#2e7d32,color:#1b5e20
    style GF7 fill:#c8e6c9,stroke:#2e7d32,color:#1b5e20
    style BF1 fill:#ffcdd2,stroke:#c62828,color:#b71c1c
    style BF2 fill:#ffcdd2,stroke:#c62828,color:#b71c1c
    style BF3 fill:#ffcdd2,stroke:#c62828,color:#b71c1c
    style BF4 fill:#ffcdd2,stroke:#c62828,color:#b71c1c
    style BF5 fill:#ffcdd2,stroke:#c62828,color:#b71c1c
    style BF6 fill:#ffcdd2,stroke:#c62828,color:#b71c1c
```

### Module-to-Capability Mapping

| Module | Capability | Lifecycle Stage |
|--------|-----------|-----------------|
| 01 | AI Champions Kick-off & Enterprise Context | Foundation |
| 02 | GitHub Copilot Enterprise | Copilot Foundations |
| 03 | Prompt & Context Engineering | Context Engineering |
| 04 | Spec-Driven Development | SDD |
| 05 | SDD to Complete SDLC | Greenfield Full Cycle |
| 06 | Test Strategy & Testcontainers | Test & PR Quality |
| 07 | MCP-Enabled Agentic Engineering | MCP Workflows |
| 08 | PR Automation & LLM-as-Judge | Test & PR Quality |
| 09 | Studio Workflows (Rovo Studio) | MCP Workflows |
| 10 | Agent Prism Observability | Observe, Measure, Scale |
| 11 | ROI & Token Economics | Observe, Measure, Scale |
| 12 | Capstone & Adoption Roadmap | Capstone |

## Core Concepts

### The AI Champion Role

One shared foundation, then role-specific depth as the programme progresses. Each function area owns a distinct part of the AI-assisted engineering lifecycle.

```mermaid
graph TB
    subgraph Architects["Architects & Engineering Leads"]
        AR1[Architecture / Dependency Analysis]
        AR2[Design-Principle Compliance]
        AR3[Change-Impact Review]
    end

    subgraph DevTest["Developers & Testers"]
        DT1[Deepest Copilot Hands-On]
        DT2[SDD Implementation]
        DT3[Testcontainers Test Strategy]
    end

    subgraph DevOps["DevOps & SRE"]
        DO1[CI/PR Pipelines]
        DO2[MCP Tool Governance]
        DO3[Agent Prism Telemetry]
    end

    subgraph Product["Product, Program & Design"]
        PP1[Studio-Style Workflows]
        PP2[Specification Quality]
        PP3[Business Intent → Implementation]
    end

    style Architects fill:#1565c0,stroke:#0d47a1,color:#fff
    style DevTest fill:#2e7d32,stroke:#1b5e20,color:#fff
    style DevOps fill:#e65100,stroke:#bf360c,color:#fff
    style Product fill:#6a1b9a,stroke:#4a148c,color:#fff
    style AR1 fill:#bbdefb,stroke:#1565c0,color:#0d47a1
    style AR2 fill:#bbdefb,stroke:#1565c0,color:#0d47a1
    style AR3 fill:#bbdefb,stroke:#1565c0,color:#0d47a1
    style DT1 fill:#c8e6c9,stroke:#2e7d32,color:#1b5e20
    style DT2 fill:#c8e6c9,stroke:#2e7d32,color:#1b5e20
    style DT3 fill:#c8e6c9,stroke:#2e7d32,color:#1b5e20
    style DO1 fill:#ffe0b2,stroke:#e65100,color:#bf360c
    style DO2 fill:#ffe0b2,stroke:#e65100,color:#bf360c
    style DO3 fill:#ffe0b2,stroke:#e65100,color:#bf360c
    style PP1 fill:#e1bee7,stroke:#6a1b9a,color:#4a148c
    style PP2 fill:#e1bee7,stroke:#6a1b9a,color:#4a148c
    style PP3 fill:#e1bee7,stroke:#6a1b9a,color:#4a148c
```

| Role | Primary Ownership |
|------|-------------------|
| **Architects & Engineering Leads** | Architecture/dependency analysis, design-principle compliance, change-impact review across greenfield and brownfield work |
| **Developers & Testers** | Deepest hands-on Copilot, SDD, and Testcontainers-based test-strategy skill in the programme |
| **DevOps & SRE** | CI/PR pipelines, MCP tool governance, connecting Agent Prism telemetry to operational review |
| **Product, Program & Design** | Studio-style workflows, specification quality, translating business intent into implementation-ready work |

### Business & Engineering Risks of Unmanaged AI

The problems this programme exists to solve, grouped by where they show up.

| Business Risk | Impact |
|---------------|--------|
| Slow feature and application implementation cycles | Delays in delivery, missed market windows |
| Limited visibility into AI usage, quality, and measurable ROI | Cannot justify investment or optimise spend |
| Inconsistent results when teams rely on prompts alone | Unpredictable quality, rework, wasted effort |

| Engineering Risk | Impact |
|------------------|--------|
| AI-generated changes disrupting brownfield architecture or patterns | Broken contracts, architectural drift |
| Manual, inconsistent PR review creating bottlenecks | Slower throughput, missed defects |
| Excessive token consumption from weak context discipline | Unnecessary AI cost, lower quality outputs |

### Current State vs Target State

The workshop maps today's engineering pain points against the target workflow this programme builds toward.

**Pain points identified:**
- Feature implementation cycles — slow, repetitive analysis and coding
- Brownfield enhancement — risk of violating existing architecture
- PR cycles — manual review bottlenecks
- Review latency — inconsistent validation
- Testing effort — manual, incomplete regression coverage
- AI usage blind spots — no visibility into what's working

**Target workflow:**

```mermaid
graph LR
    A[Business Intent] --> B[Specification via SDD]
    B --> C[Architecture Review]
    C --> D[Task Breakdown]
    D --> E[Copilot-Assisted Implementation]
    E --> F[Testcontainers Validation]
    F --> G[LLM-as-Judge PR Review]
    G --> H[CI/CD Pipeline]
    H --> I[Agent Prism Observability]

    style A fill:#1565c0,stroke:#0d47a1,color:#fff
    style B fill:#6a1b9a,stroke:#4a148c,color:#fff
    style C fill:#2e7d32,stroke:#1b5e20,color:#fff
    style D fill:#e65100,stroke:#bf360c,color:#fff
    style E fill:#1565c0,stroke:#0d47a1,color:#fff
    style F fill:#c62828,stroke:#b71c1c,color:#fff
    style G fill:#f9a825,stroke:#f57f17,color:#1a1a1a
    style H fill:#2e7d32,stroke:#1b5e20,color:#fff
    style I fill:#6a1b9a,stroke:#4a148c,color:#fff
```

### Agent Prism — The Monitoring Lens

Observability is connected from Module 01 onward, not added later. Agent Prism provides five monitoring capabilities that map directly to programme success metrics.

```mermaid
graph TB
    AP[Agent Prism] --> T[Traces & Replay]
    AP --> FP[Failure Patterns]
    AP --> TU[Token Usage & Cost Leakage]
    AP --> QC[Quality & Control Signals]
    AP --> AT[Adoption Tracking]

    T -->|maps to| M1[Implementation Cycle Time]
    FP -->|maps to| M2[Quality / Regression Risk]
    TU -->|maps to| M3[Token Usage & AI Cost]
    QC -->|maps to| M4[PR Review Time]
    AT -->|maps to| M5[Testing Effort]

    style AP fill:#6a1b9a,stroke:#4a148c,color:#fff
    style T fill:#1565c0,stroke:#0d47a1,color:#fff
    style FP fill:#c62828,stroke:#b71c1c,color:#fff
    style TU fill:#f9a825,stroke:#f57f17,color:#1a1a1a
    style QC fill:#2e7d32,stroke:#1b5e20,color:#fff
    style AT fill:#e65100,stroke:#bf360c,color:#fff
    style M1 fill:#bbdefb,stroke:#1565c0,color:#0d47a1
    style M2 fill:#ffcdd2,stroke:#c62828,color:#b71c1c
    style M3 fill:#fff9c4,stroke:#f9a825,color:#f57f17
    style M4 fill:#c8e6c9,stroke:#2e7d32,color:#1b5e20
    style M5 fill:#ffe0b2,stroke:#e65100,color:#bf360c
```

| Capability | What It Tracks | Programme Metric |
|------------|----------------|------------------|
| **Traces & Replay** | Full execution traces of AI interactions | Implementation cycle time |
| **Failure Patterns** | Repeated error modes and regressions | Quality / regression risk |
| **Token Usage & Cost Leakage** | Token consumption per task, cost anomalies | Token usage & AI cost |
| **Quality & Control Signals** | Specification compliance, design adherence | PR review time |
| **Adoption Tracking** | Who is using AI, how often, which workflows | Testing effort & ROI |

### Workshop Output

The 40-minute hands-on activity closes this module. Participants walk away with:

**You'll map:**
- Pain points across feature implementation, brownfield enhancement, PR cycles, review latency, and testing effort
- Today's engineering workflow, stage by stage
- Current AI usage blind spots

**You'll walk away with:**
- A mapped target-state engineering workflow
- Greenfield and brownfield use cases identified for this cohort
- Baseline measures set for cycle time, quality/regression risk, PR review time, testing effort, token usage, and AI cost

### Module 01 Outcomes

- The shift from isolated AI experimentation to disciplined adoption is understood
- Role-based AI Champion expectations are mapped for your function
- Business and engineering risks of unmanaged AI usage are identified
- The end-to-end AI-assisted software engineering lifecycle is introduced
- Agent Prism's role as the monitoring lens is understood
- A target-state workflow and baseline success metrics are drafted

### Key Terminology

| Term | Definition |
|------|------------|
| **AI Champion** | An engineer who drives disciplined AI adoption within their team, owning both capability building and governance |
| **SDD** | Spec-Driven Development — using structured specifications to drive precise, traceable implementation |
| **MCP** | Model Context Protocol — connecting AI agents to approved tools, repositories, and enterprise workflows |
| **LLM-as-Judge** | Using a language model to evaluate output quality against defined criteria |
| **Agent Prism** | The observability platform for monitoring AI agent behaviour, traces, cost, and quality |
| **Context Engineering** | Selecting, structuring, and compressing the right context for AI instead of relying on raw prompts |
| **Brownfield** | Enhancing existing systems while preserving architecture, patterns, and contracts |
| **Greenfield** | Building new features or applications from scratch |

## References

### Programme Materials

- Module 01 Presentation: `presentations/Module01_AI_Champions_Kickoff_Enterprise_Context.pdf`
- Course Outline: `courseOutline/NIIT_Honeywell_AI_Champions_GitHub_AgentPrism (Software_Engineering).pdf`

### Further Reading — External References

**Industry evidence for disciplined AI adoption**
- [DORA — 2024 Accelerate State of DevOps Report](https://dora.dev/research/2024/dora-report/) — the data behind "AI is not a panacea": AI raises individual throughput but can reduce delivery stability when batch size and testing discipline slip.
- [DORA — Impact of Generative AI on Software Development](https://dora.dev/ai/) — DORA's consolidated guidance on measuring and capturing the ROI of AI-assisted development.
- [GitHub — Research: Quantifying GitHub Copilot's impact on developer productivity](https://github.blog/news-insights/research/research-quantifying-github-copilots-impact-on-developer-productivity-and-happiness/) — baseline productivity study referenced across the programme's metrics.
- [Anthropic — Building Effective Agents](https://www.anthropic.com/engineering/building-effective-agents) — vocabulary for agentic workflows the later modules depend on.

**The AI-assisted engineering lifecycle**
- [Anthropic — Effective Context Engineering for AI Agents](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents) — context engineering as the successor to prompt engineering (Module 03).
- [GitHub — Spec-driven development with AI: get started with a new open-source toolkit](https://github.blog/ai-and-ml/generative-ai/spec-driven-development-with-ai-get-started-with-a-new-open-source-toolkit/) — the SDD approach introduced in Module 04.
- [Model Context Protocol — official site and specification](https://modelcontextprotocol.io/) and [Anthropic — Introducing the Model Context Protocol](https://www.anthropic.com/news/model-context-protocol) — the open standard behind Module 07.
- [Evidently AI — LLM-as-a-judge: a complete guide](https://www.evidentlyai.com/llm-guide/llm-as-a-judge) — evaluation patterns used in Module 08.

**Governance and risk**
- [GitHub Copilot Trust Center](https://copilot.github.trust.page/) — security, privacy, IP, and compliance posture for enterprise Copilot adoption.
- [Responsible use of GitHub Copilot features](https://docs.github.com/en/copilot/responsible-use) — GitHub's per-feature limitations and responsible-use guidance.
- [NIST AI Risk Management Framework](https://www.nist.gov/itl/ai-risk-management-framework) — a reference frame for the business and engineering risks catalogued in this module.

**Agent Prism — the monitoring lens**
- [OpenTelemetry — GenAI semantic conventions](https://opentelemetry.io/docs/specs/semconv/gen-ai/) — the trace schema Agent Prism and similar tools consume.
- [Arize Phoenix](https://github.com/Arize-ai/phoenix) — a complementary open-source LLM/agent tracing and evaluation platform.

### Next Module

Module 02 — GitHub Copilot Enterprise Features and Engineering Workflows (2 hours). Hands-on with chat, inline assist, agent mode, and repository-aware Copilot capabilities across real engineering tasks.
