# Rasa Persona Document

## Enterprise AI Agent Orchestration Platform

---

## Persona Map Overview

```
                        BUYER                          USER                         INFLUENCER
                 ┌──────────────────┐         ┌──────────────────┐         ┌──────────────────┐
  EXECUTIVE      │  Elena Torres    │         │                  │         │  CFO / CISO      │
                 │  VP Engineering  │         │                  │         │  (Governance     │
                 │  (Budget holder) │         │                  │         │   veto power)    │
                 └────────┬─────────┘         └──────────────────┘         └──────────────────┘
                          │
  MANAGEMENT     ┌────────▼─────────┐         ┌──────────────────┐         ┌──────────────────┐
                 │  Maya Chen       │         │                  │         │  IT Security     │
                 │  Head of         │◄────────│                  │         │  Lead            │
                 │  Conversational  │         │                  │         │  (Vendor review) │
                 │  AI (Champion)   │         │                  │         │                  │
                 └────────┬─────────┘         └──────────────────┘         └──────────────────┘
                          │
  PRACTITIONER            │          ┌──────────────────┐  ┌──────────────────┐
                          ├─────────►│  Jordan Reeves   │  │  Raj Patel       │
                          │          │  Conversation     │  │  Platform /      │
                          │          │  Designer         │  │  MLOps Engineer  │
                          │          └──────────────────┘  └──────────────────┘
                          │
                          │          ┌──────────────────┐  ┌──────────────────┐
                          └─────────►│  Anika Sharma    │  │  Luis Moreno     │
                                     │  ML Engineer      │  │  CX / Contact    │
                                     │                   │  │  Center Manager  │
                                     └──────────────────┘  └──────────────────┘
```

---

## Persona 1: Maya Chen — Head of Conversational AI (Primary)

**Role:** Head of Conversational AI / Director of AI Platform Engineering
**Archetype:** The Platform Champion

### Demographics & Context

| Attribute | Detail |
|---|---|
| **Age** | 38 |
| **Location** | Chicago, IL |
| **Company** | Top-20 US bank, 45,000 employees |
| **Industry** | Financial services (also representative of healthcare, telecom, insurance) |
| **Reports to** | VP of Engineering (Elena Torres) |
| **Team size** | 18 — 4 ML engineers, 3 conversation designers, 3 platform engineers, 4 backend engineers, 2 QA, 1 product manager, 1 data analyst |
| **Experience** | 12 years in software engineering, 5 years leading AI/ML teams |
| **Education** | MS Computer Science, MBA |
| **Salary band** | $210K–$280K total comp |
| **Technical depth** | Reviews architecture decisions, reads code in PRs, doesn't write production code daily |

### Day in the Life

| Time | Activity | Pain Point |
|---|---|---|
| 8:00 AM | Checks Slack — overnight agent failures in the IVR system | No unified dashboard; has to check 3 tools |
| 9:00 AM | Standup with her team — conversation designer blocked waiting on engineer to deploy a flow change | Designer can't self-serve |
| 10:00 AM | Meeting with compliance — they want audit trail for every LLM-generated response | Current tooling doesn't log LLM I/O at the turn level |
| 11:00 AM | Vendor call — SaaS chatbot provider wants to renegotiate; per-conversation pricing is 3x what it was at contract signing | Volume growth = cost explosion |
| 1:00 PM | Architecture review — team proposes adding a second chatbot vendor for internal IT helpdesk because current tool doesn't support it | Tool fragmentation |
| 3:00 PM | Exec presentation prep — needs to show ROI of AI agents to the board | Can't easily extract containment rate, deflection savings, or cost-per-resolution |
| 5:00 PM | Sprint planning — 60% of backlog is "plumbing" (channel adapters, logging, auth) not agent intelligence | Too much undifferentiated work |

### Goals

1. **Consolidate** all conversational AI (voice IVR, web chat, WhatsApp, internal helpdesk) onto a single platform.
2. **Own the infrastructure** — deploy on the bank's existing AWS environment so customer data never leaves their VPC.
3. **Unlock LLM flexibility** — use GPT-4 for generation today, swap to Claude or an open-source model tomorrow without rewriting agent logic.
4. **Reduce time-to-production** from 12 weeks to 3 weeks for a new agent.
5. **Empower non-engineers** — let conversation designers iterate on flows without a full CI/CD cycle.
6. **Scale** from 5 production agents to 50+ in 18 months.
7. **Demonstrate ROI** with clear metrics that map to business outcomes (calls deflected, handle time reduced, CSAT maintained).

### Frustrations

| Frustration | Impact | Current Workaround |
|---|---|---|
| **Vendor lock-in** — SaaS chatbot tool controls infra, data, and LLM choice | Can't meet compliance requirements; board pressure | Manual data export + redaction scripts |
| **Fragmentation** — voice team uses Tool A, chat team uses Tool B, internal uses a custom Python bot | Duplicated effort, inconsistent user experience, 3 separate on-call rotations | Weekly sync meetings to share learnings (doesn't scale) |
| **Observability gaps** — when an agent misbehaves, takes hours to trace NLU → policy → LLM → action | Increased MTTR, SLA breaches, customer complaints | Engineers manually grep through logs |
| **Compliance overhead** — each new SaaS vendor triggers 3–6 month security review | Slows adoption of better tools | Use existing (inferior) approved tools |
| **Designer bottleneck** — every conversation change requires a developer | Conversation designers are idle 40% of their time waiting on PRs | Designers write specs in Confluence, engineers translate to code |
| **Cost unpredictability** — LLM token costs spike unexpectedly with no attribution | Budget overruns, CFO scrutiny | Monthly manual token audit |

### Decision Criteria (Ranked)

| # | Criterion | Weight | Minimum Threshold |
|---|---|---|---|
| 1 | Infrastructure ownership (self-hosted, own cloud) | 30% | Must run in our AWS VPC |
| 2 | LLM flexibility (swap providers without rewrite) | 20% | Support 3+ LLM providers |
| 3 | Multi-channel from single agent definition | 15% | Voice + chat at minimum |
| 4 | Enterprise governance (RBAC, audit, PII) | 15% | SOC 2 Type II, RBAC, audit log |
| 5 | Total cost of ownership (3 year) | 10% | < 70% of current SaaS spend |
| 6 | Team productivity (designer self-service) | 10% | Visual editor or low-code option |

### Buying Behavior

- **Research phase:** Reads Gartner/Forrester reports, attends AI infrastructure meetups, talks to peers at other banks.
- **Evaluation phase:** Runs a 6-week proof of concept with 2–3 vendors; scores against a weighted rubric (above). Involves Raj (platform) and Jordan (design) in hands-on eval.
- **Purchase influencers:** CISO must approve data handling; CFO must approve budget; VP Engineering (Elena) is the formal budget holder.
- **Deal cycle:** 3–6 months from first contact to signed contract.
- **Contract preference:** Annual subscription with usage tiers, not per-conversation pricing.

### Messaging That Resonates

| Message | Why it Works |
|---|---|
| "Own your AI — run it on your infrastructure, with your LLM, under your rules." | Directly addresses #1 frustration (vendor lock-in) and #1 decision criterion (infrastructure ownership). |
| "One platform for every channel — voice, chat, API, internal." | Promises consolidation, her top goal. |
| "From 12 weeks to 3 weeks for a new agent." | Tangible time-to-value she can pitch to her VP. |
| "Your conversation designers ship changes without waiting on engineers." | Solves the team bottleneck she sees daily. |

### Anti-Messaging (What Doesn't Work)

| Message | Why it Fails |
|---|---|
| "We'll handle everything in our cloud." | Opposite of what she wants — she needs self-hosted. |
| "Powered by GPT-4." | Signals single-vendor dependency, not flexibility. |
| "No-code AI agent builder." | Sounds like a toy, not an enterprise platform. She needs code-first with low-code options, not low-code only. |

### Quote

> "I don't want to rent AI — I want to own it. My board asks me every quarter who has access to our customer data, and 'some SaaS vendor's cloud' is not an acceptable answer."

---

## Persona 2: Jordan Reeves — Conversation Designer (Secondary)

**Role:** Senior Conversation Designer / Dialogue Architect
**Archetype:** The Creative Builder

### Demographics & Context

| Attribute | Detail |
|---|---|
| **Age** | 31 |
| **Location** | Austin, TX (remote) |
| **Company** | Same as Maya (top-20 US bank) |
| **Reports to** | Maya Chen |
| **Experience** | 6 years — started in UX writing, transitioned to conversation design 4 years ago |
| **Education** | BA Linguistics, Google Conversation Design Certificate |
| **Salary band** | $110K–$140K |
| **Technical comfort** | YAML, JSON, basic SQL, regex; not comfortable with Python classes, async code, or CI/CD pipelines |
| **Tools used daily** | Figma (flow diagrams), Google Sheets (utterance libraries), Slack, Jira |

### Day in the Life

| Time | Activity | Pain Point |
|---|---|---|
| 9:00 AM | Reviews customer escalation transcripts from yesterday — spots a pattern where the bot misunderstands "transfer" (money transfer vs. call transfer) | No way to quickly test a disambiguation fix without a full deploy |
| 10:00 AM | Writes a Jira ticket describing the fix and the new dialogue branch | A 20-minute design task becomes a 3-day engineering ticket |
| 11:00 AM | Pair-designs a new "lost card" voice flow with a product manager using sticky notes on a Miro board | The Miro board can't be imported into the bot — manual translation required |
| 1:00 PM | Writes training utterances in a spreadsheet for a new intent | No way to test NLU confidence on these utterances before engineering picks it up |
| 3:00 PM | Attends a meeting about the WhatsApp channel launch — learns the WhatsApp bot is being built on a completely different platform than the web chat bot she designed | Her work can't be reused across channels |
| 4:00 PM | Reviews a PR from an engineer who implemented her design spec — discovers they interpreted "confirm before proceeding" differently than she intended | Design-to-implementation fidelity is low |

### Goals

1. **Author dialogue flows directly** in a visual or low-code tool that is connected to the production system — not in disconnected documents.
2. **Test changes instantly** — simulate a full conversation (multi-turn, with slot filling, API responses) from her browser.
3. **Design once, deploy everywhere** — write a "lost card" flow once and have it work on voice and chat.
4. **Reuse components** — authentication, disambiguation, escalation-to-human should be drag-and-drop building blocks.
5. **A/B test conversation strategies** without needing engineering to set up experiments.
6. **Understand performance** — see which flows have high drop-off, where users get stuck, which utterances cause NLU confusion.

### Frustrations

| Frustration | Severity | Frequency |
|---|---|---|
| Every change requires an engineer, a PR, a CI/CD cycle, and a staging deploy | Critical | Daily |
| Voice and chat are authored in completely separate systems | High | Weekly |
| No real-time conversation simulator — has to deploy to staging to test | High | Daily |
| Can't see NLU confidence scores or entity extraction results for her test utterances | Medium | Weekly |
| A/B testing requires Raj to set up feature flags and traffic splitting | Medium | Monthly |
| Design specs (in Miro/Confluence) frequently get misinterpreted by engineers | High | Weekly |

### Technical Requirements (From Jordan's Perspective)

| Need | Details |
|---|---|
| **Visual flow editor** | Drag-and-drop nodes for: user says, bot says, condition, slot fill, API call, handoff. Must support branching and loops. |
| **Conversation simulator** | Type (or speak) test messages and see the full bot response including slot values, NLU results, and which flow path was taken. |
| **Utterance testing** | Paste a batch of utterances and see intent classification + entity extraction results instantly. |
| **Component library** | Pre-built, configurable components for common patterns (auth, disambiguation, confirm, escalate). |
| **Version history** | See what changed in a flow, who changed it, and roll back to a previous version. |
| **Collaboration** | Comment on specific flow nodes, tag teammates, resolve comments (like Figma). |
| **Analytics per flow** | Drop-off rate, completion rate, avg turns to resolution, top misunderstood utterances — per flow, not just per agent. |

### Messaging That Resonates

| Message | Why |
|---|---|
| "Design conversations, don't code them." | Validates her role and removes the engineering bottleneck. |
| "Test it before you ship it — simulate any conversation in your browser." | Directly solves her #1 daily frustration. |
| "One flow. Every channel." | Saves her from duplicating work across voice and chat. |

### Quote

> "I can design a better conversation in 20 minutes than it takes engineering to deploy a one-line copy change. I just need a tool that lets me."

---

## Persona 3: Raj Patel — Platform / MLOps Engineer (Secondary)

**Role:** Senior Platform Engineer / MLOps
**Archetype:** The Reliability Guardian

### Demographics & Context

| Attribute | Detail |
|---|---|
| **Age** | 34 |
| **Location** | New York, NY |
| **Company** | Same as Maya (top-20 US bank) |
| **Reports to** | Maya Chen |
| **Experience** | 9 years — backend engineering → DevOps → MLOps |
| **Education** | BS Computer Science |
| **Salary band** | $170K–$220K |
| **Technical depth** | Deep — Kubernetes, Terraform, Helm, Prometheus/Grafana, ArgoCD, Python, Go |
| **Tools used daily** | kubectl, Terraform, ArgoCD, Grafana, PagerDuty, GitHub, VS Code |

### Day in the Life

| Time | Activity | Pain Point |
|---|---|---|
| 7:30 AM | PagerDuty alert — the voice IVR agent latency spiked to 8 seconds overnight | No trace to tell if it's the NLU model, the LLM API, or a downstream CRM call |
| 8:30 AM | Manually checks LLM provider status page, NLU model metrics, and CRM API logs in 3 different dashboards | No unified observability across the agent pipeline |
| 10:00 AM | Deploys a new version of the chat agent — requires manual steps: update Docker image tag, apply Helm values, run smoke tests | No standardized CI/CD for agent deployments |
| 11:30 AM | CFO's office asks for LLM cost breakdown by agent and business unit | Impossible — all LLM calls go through one API key with no attribution |
| 1:00 PM | Engineer on another team asks how to set up guardrails for PII — Raj has to write custom middleware because the platform doesn't have it built in | Guardrails are DIY per-agent instead of platform-wide |
| 3:00 PM | Conversation designer asks him to set up an A/B test — requires feature flags, traffic splitting, separate model endpoints | Simple experiment takes a full sprint |
| 4:30 PM | Rollback needed — a bad agent version went to production and there's no one-click rollback | Manually reverts Helm chart values and re-deploys |

### Goals

1. **Zero-downtime deployments** with one-click rollback for all agents.
2. **Unified observability** — single dashboard showing all agents, all channels, with per-turn tracing (NLU → policy → LLM → action → response).
3. **GitOps-native workflow** — agent config as code, ArgoCD-compatible, PR-based promotion through environments.
4. **Platform-level guardrails** — PII redaction, topic boundaries, and hallucination checks enforced centrally so individual teams can't bypass them.
5. **LLM cost attribution** — token usage and cost per agent, per flow, per LLM provider, exportable to finance's cost allocation system.
6. **Self-healing infrastructure** — auto-scaling based on conversation concurrency, LLM fallback chains when a provider degrades.
7. **Reduce toil** — stop being the bottleneck for A/B tests, environment setup, and deployment tasks.

### Frustrations

| Frustration | Severity | Impact |
|---|---|---|
| No end-to-end tracing across the agent pipeline | Critical | MTTR is hours instead of minutes |
| LLM costs are a black box | High | Budget overruns, CFO escalations |
| Each agent team builds their own deployment pipeline | High | Inconsistent reliability, duplicated effort |
| Guardrails are per-agent, not platform-wide | High | Compliance risk — one team forgets PII redaction |
| Rollback is manual | Critical | Extended outages when bad versions ship |
| He is the bottleneck for designer experiments | Medium | Team velocity suffers, he burns out |

### Technical Requirements (From Raj's Perspective)

| Need | Details |
|---|---|
| **Helm chart + Terraform modules** | Production-ready deployment on EKS/AKS/GKE with sensible defaults and full customization. |
| **OpenTelemetry integration** | Distributed traces spanning NLU, policy, LLM, custom action, and response — exportable to Jaeger, Datadog, or Grafana Tempo. |
| **Prometheus metrics** | Request rate, latency (p50/p95/p99), error rate, active conversations, token consumption — per agent, per channel. |
| **GitOps support** | Agent definitions as YAML in Git. ArgoCD or Flux-compatible. PR triggers training/test/deploy. |
| **Canary / blue-green deployments** | Route a percentage of traffic to a new agent version, monitor, then promote or rollback. |
| **Centralized guardrails config** | Define PII patterns, topic boundaries, and output filters at the platform level. Individual agents inherit or extend. |
| **LLM gateway** | A proxy layer that logs token usage, enforces rate limits, and routes to the right model per policy. |
| **RBAC for infrastructure** | Separate permissions: who can deploy to production, who can modify guardrails, who can view costs. |

### Messaging That Resonates

| Message | Why |
|---|---|
| "End-to-end tracing for every conversation turn — from NLU to LLM to API to response." | Directly solves his #1 pain (MTTR). |
| "GitOps-native. Helm chart included. ArgoCD-ready." | Speaks his language — he doesn't want to invent a deployment pipeline. |
| "Platform-level guardrails. Set once, enforce everywhere." | Removes the compliance risk that keeps him up at night. |
| "Know exactly which agent is spending how many tokens on which LLM." | Solves the CFO problem he can't answer today. |

### Quote

> "I don't need another AI tool — I need an AI platform that behaves like production infrastructure. Give me Helm charts, OTel traces, and GitOps. I'll handle the rest."

---

## Persona 4: Anika Sharma — ML Engineer (Tertiary)

**Role:** Senior ML Engineer
**Archetype:** The Model Specialist

### Demographics & Context

| Attribute | Detail |
|---|---|
| **Age** | 29 |
| **Location** | Seattle, WA |
| **Reports to** | Maya Chen |
| **Experience** | 5 years — NLP research → applied ML |
| **Education** | MS in NLP / Computational Linguistics |
| **Salary band** | $160K–$200K |
| **Technical depth** | Expert — PyTorch, transformers, fine-tuning, prompt engineering, evaluation frameworks |
| **Tools used daily** | Jupyter, VS Code, Weights & Biases, Hugging Face, Python |

### Goals

1. Fine-tune and evaluate NLU models specific to the company's domain (financial terminology, product names, compliance language).
2. Optimize LLM prompts for accuracy, cost, and latency — including prompt caching and few-shot strategies.
3. Build evaluation pipelines that catch regressions before they reach production.
4. Experiment with new models (open-source, fine-tuned) without disrupting production agents.

### Frustrations

| Frustration | Impact |
|---|---|
| No structured way to run offline evaluations against a test set before deploying | Regressions slip into production |
| Prompt changes are mixed in with infrastructure changes in the same PR | Hard to isolate prompt performance |
| Can't shadow-test a new model against production traffic | Model upgrades are all-or-nothing |
| No standard interface for swapping between LLM providers | Every provider switch requires code changes |

### Key Requirements

- **Evaluation framework:** Run a test suite of conversations against an agent and get a precision/recall/F1 report before merge.
- **Prompt versioning:** Version prompts independently from agent logic. Track which prompt version produced which results.
- **Shadow mode:** Route production traffic to a new model in read-only mode. Compare outputs without affecting users.
- **Model registry integration:** Pull models from Hugging Face, S3, or internal model registry. Register custom NLU models.

### Quote

> "I want to treat prompts like code — versioned, tested, reviewed, and deployed independently."

---

## Persona 5: Luis Moreno — CX / Contact Center Manager (Tertiary)

**Role:** Director of Customer Experience / Contact Center Operations
**Archetype:** The Business Stakeholder

### Demographics & Context

| Attribute | Detail |
|---|---|
| **Age** | 44 |
| **Location** | Dallas, TX |
| **Reports to** | SVP of Customer Experience |
| **Experience** | 18 years in contact center operations, 3 years working with AI/automation |
| **Education** | BBA, Six Sigma Black Belt |
| **Salary band** | $150K–$190K |
| **Technical depth** | Low — uses dashboards, spreadsheets, and Salesforce. Does not write code. |
| **Tools used daily** | Salesforce, Genesys WFM, Tableau, Excel, Slack |

### Goals

1. **Reduce average handle time (AHT)** by deflecting simple inquiries to AI agents.
2. **Maintain or improve CSAT** — AI agents must not frustrate customers.
3. **Get real-time visibility** into AI agent performance without asking engineering.
4. **Smooth human handoff** — when the AI agent escalates, the human agent must have full context.
5. **Forecast staffing** — understand how AI containment rate affects human agent headcount planning.

### Frustrations

| Frustration | Impact |
|---|---|
| Can't see AI agent metrics in the same dashboard as human agent metrics | Two separate worlds; can't measure blended performance |
| When escalation happens, the human agent gets no context — customer has to repeat everything | CSAT drops, AHT increases |
| No say in how the AI agent behaves — every change request goes through Maya's engineering team | Feels disconnected from a tool that directly impacts his KPIs |
| AI agent "containment rate" numbers from the vendor don't match his actual call deflection data | Trust gap |

### Key Requirements

- **Executive dashboard:** Containment rate, escalation rate, AHT (AI vs. human), CSAT proxy, cost per resolution — refreshed hourly.
- **Escalation context:** Full conversation transcript and extracted entities passed to the human agent's screen pop in Genesys/Salesforce.
- **Feedback loop:** Flag bad AI responses from the dashboard and route them to the conversation design team.
- **Reporting API:** Export AI agent metrics into Tableau alongside human agent metrics for blended reporting.

### Messaging That Resonates

| Message | Why |
|---|---|
| "See exactly how your AI agents perform — containment, CSAT, handle time — in real time." | He lives and dies by these metrics. |
| "When the bot hands off, the human agent sees everything." | Solves his biggest CX pain point. |
| "AI agents that make your team better, not replace them." | Politically important — he's not trying to eliminate his team. |

### Quote

> "I don't care what LLM you use. I care that my customers don't have to say their account number twice."

---

## Persona 6: Elena Torres — VP of Engineering (Buyer)

**Role:** VP of Engineering
**Archetype:** The Budget Holder

### Demographics & Context

| Attribute | Detail |
|---|---|
| **Age** | 47 |
| **Location** | Chicago, IL |
| **Reports to** | CTO |
| **Scope** | 200+ engineers across platform, data, and applied AI teams |
| **Involvement** | Signs off on vendor contracts > $200K; reviews Maya's quarterly plans |

### What She Cares About

1. **Total cost of ownership** — not just licensing, but infrastructure, headcount, and opportunity cost.
2. **Risk** — will this platform be around in 3 years? Is the company well-funded? Is there an open-source fallback?
3. **Consolidation** — fewer vendors, fewer tools, fewer on-call rotations.
4. **Compliance** — CISO must approve. If the tool introduces data handling risk, it's a non-starter.
5. **Team velocity** — does this platform make Maya's team faster or slower?

### Buying Involvement

- Reviews Maya's vendor evaluation rubric and POC results.
- Asks for a 3-year TCO comparison (Rasa vs. current SaaS vs. DIY).
- Requires CISO sign-off before contract.
- Final signature on contracts > $200K ARR.

### Messaging That Resonates

| Message | Why |
|---|---|
| "Replace 3 tools with 1 platform. Fewer vendors, lower risk, lower cost." | She wants consolidation. |
| "Self-hosted — no new vendor data processing agreement needed." | Accelerates CISO approval. |
| "Open-source core with enterprise support — no lock-in." | De-risks the decision. |

### Quote

> "Bring me a 3-year TCO model that beats what we're paying today, and make sure the CISO doesn't flag it. Then we can talk."

---

## Persona Interaction Map

```
                  Elena (VP Eng)
                  Approves budget
                       │
                       ▼
     ┌─────────── Maya (Head of AI) ───────────┐
     │            Champions platform            │
     │            Runs POC & evaluation          │
     │                                          │
     ▼                  ▼                       ▼
  Jordan             Raj                    Anika
  (Designer)         (Platform)             (ML Eng)
  Evaluates:         Evaluates:             Evaluates:
  - Visual editor    - Deployment           - LLM connectors
  - Simulator        - Observability        - Model eval
  - Components       - GitOps              - Prompt mgmt
                     - Guardrails
     │                  │                       │
     └──────────────────┼───────────────────────┘
                        ▼
                    Luis (CX Mgr)
                    Evaluates:
                    - Dashboards
                    - Handoff quality
                    - Containment metrics
                    (Influences from the business side)
```

### Influence Dynamics

| Relationship | Dynamic |
|---|---|
| **Maya → Elena** | Maya builds the business case; Elena approves if TCO and risk are acceptable. |
| **Maya → Jordan/Raj/Anika** | Maya assigns POC tasks; their hands-on experience determines Maya's recommendation. |
| **Jordan → Maya** | If Jordan says "I can't use this tool without an engineer," Maya vetoes. Designer self-service is a hard requirement. |
| **Raj → Maya** | If Raj says "this doesn't fit our infra" or "observability is insufficient," Maya vetoes. Production-readiness is non-negotiable. |
| **Luis → Elena** | Luis's CX metrics influence Elena's view of ROI. If AI agents hurt CSAT, Elena pulls funding. |
| **CISO → Elena** | CISO has veto power over any tool that touches customer data. Self-hosted deployment significantly accelerates CISO approval. |

---

## Jobs to Be Done (JTBD) Summary

| Persona | Job to Be Done | Success Metric |
|---|---|---|
| **Maya** | When I'm consolidating AI tools across channels, I want a single platform I can own, so that I reduce vendor risk and ship agents faster. | Time to production < 3 weeks; 1 platform replaces 3 tools. |
| **Jordan** | When I'm designing a conversation flow, I want to author and test it visually, so that I can iterate without waiting on engineering. | > 60% of flow changes ship without engineering involvement. |
| **Raj** | When I'm operating AI agents in production, I want unified observability and GitOps deployments, so that I can diagnose issues in minutes and deploy with confidence. | MTTR < 15 min; zero-downtime deployments; one-click rollback. |
| **Anika** | When I'm evaluating a new LLM or prompt strategy, I want to test it against production traffic in shadow mode, so that I can validate improvements before rollout. | Regression test suite runs on every PR; shadow mode available for model comparison. |
| **Luis** | When I'm managing customer experience, I want to see AI and human agent performance in one view, so that I can optimize the blended experience. | Single dashboard with containment rate, AHT, CSAT; seamless handoff with context. |
| **Elena** | When I'm approving AI platform investments, I want a clear TCO model and compliance story, so that I can make a confident decision. | 3-year TCO < current spend; CISO approval in < 4 weeks. |

---

## Persona Validation Checklist

Use this checklist to validate these personas against real customer interviews:

- [ ] Confirmed that infrastructure ownership is the #1 decision criterion (not price, not features)
- [ ] Confirmed that conversation designers exist as a distinct role (not engineers wearing a design hat)
- [ ] Confirmed that multi-channel consolidation is a real goal (not just nice-to-have)
- [ ] Confirmed that LLM vendor flexibility matters today (not just "someday")
- [ ] Confirmed that CX/contact center managers are involved in the buying process
- [ ] Confirmed the 3–6 month deal cycle for enterprise procurement
- [ ] Confirmed that CISO/security review is a gate in every deal
- [ ] Confirmed that GitOps / Helm / Kubernetes is the expected deployment model
- [ ] Confirmed that open-source core matters for risk mitigation (not just cost)
- [ ] Confirmed that LLM cost attribution is a real and urgent pain (not theoretical)
