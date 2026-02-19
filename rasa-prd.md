# Rasa: Enterprise AI Agent Orchestration Platform

## Product Requirements Document (PRD)

---

## 1. Persona

### Primary Persona: Maya Chen — Head of Conversational AI, Enterprise

| Attribute | Detail |
|---|---|
| **Role** | Head of Conversational AI / Director of AI Platform Engineering |
| **Company** | Mid-to-large enterprise (2,000–50,000+ employees) in financial services, healthcare, telecom, or insurance |
| **Reports to** | VP of Engineering or CTO |
| **Team size** | 8–25 engineers (ML engineers, dialogue designers, platform engineers, QA) |
| **Experience** | 10+ years in software engineering, 4+ years leading AI/ML teams |
| **Technical depth** | Understands ML pipelines, NLU, and LLM integration patterns; doesn't write daily code but reviews architecture decisions |

#### Goals

- Deploy conversational AI agents across **voice (IVR/contact center), chat (web/mobile), and internal tools (IT helpdesk, HR)** without being locked into a single LLM vendor.
- Maintain **full data sovereignty** — customer conversations never leave company-controlled infrastructure.
- Reduce time-to-production for new agents from months to weeks.
- Provide business stakeholders with visibility into agent performance without requiring engineering involvement.
- Scale from 5 agents today to 50+ within 18 months without re-architecting.

#### Frustrations

- **Vendor lock-in**: Current SaaS chatbot tools require sending all data to third-party clouds and only support one LLM provider.
- **Fragmentation**: Different teams use different tools for voice vs. chat vs. internal bots, causing duplicated effort and inconsistent experiences.
- **Observability gaps**: When an agent fails in production, it takes hours to diagnose whether the issue is in NLU, dialogue policy, LLM generation, or a downstream API.
- **Compliance pressure**: Regulated industry means every new vendor triggers a 3–6 month security review; self-hosted solutions bypass this.
- **Skill gap**: Dialogue designers and product managers can't contribute to agent logic because existing tools require Python expertise for every change.

#### Decision Criteria (Ranked)

1. **Infrastructure ownership** — Can we run it on our own cloud (AWS/Azure/GCP) or on-prem?
2. **LLM flexibility** — Can we swap between OpenAI, Anthropic, open-source models, or our own fine-tuned models without rewriting agents?
3. **Multi-channel, single-agent logic** — Can one agent definition serve voice, chat, and API channels?
4. **Enterprise governance** — RBAC, audit logs, PII redaction, approval workflows for production deployments.
5. **Total cost of ownership** — Licensing + infrastructure + team effort over 3 years vs. alternatives.

#### Quote

> "I don't want to rent AI — I want to own it. My board asks me every quarter who has access to our customer data, and 'some SaaS vendor's cloud' is not an acceptable answer."

---

### Secondary Persona: Jordan Reeves — Conversation Designer

| Attribute | Detail |
|---|---|
| **Role** | Senior Conversation Designer / Dialogue Architect |
| **Reports to** | Maya Chen |
| **Background** | Linguistics or UX design; comfortable with YAML/config but not a software engineer |
| **Primary tool** | Visual flow editors, spreadsheets of intents/utterances, testing simulators |

#### Goals

- Author and iterate on dialogue flows without waiting on engineering deployments.
- Test conversation paths end-to-end before they go to production.
- Reuse proven dialogue components (authentication, disambiguation, handoff-to-human) across agents.

#### Frustrations

- Every change requires a pull request and a CI/CD cycle.
- No way to A/B test two conversation strategies without engineering support.
- Voice and chat agents have completely separate authoring workflows.

---

### Tertiary Persona: Raj Patel — Platform / MLOps Engineer

| Attribute | Detail |
|---|---|
| **Role** | Senior Platform Engineer / MLOps |
| **Reports to** | Maya Chen |
| **Primary concern** | Infrastructure reliability, deployment pipelines, cost optimization |

#### Goals

- Deploy and update agents with zero downtime via GitOps or CI/CD.
- Monitor LLM token usage, latency percentiles, and error rates per agent.
- Enforce guardrails (topic boundaries, PII filtering, hallucination detection) at the platform level so individual agent teams can't accidentally bypass them.

#### Frustrations

- No centralized dashboard for all deployed agents.
- LLM cost attribution is impossible — can't tell which agent or feature is driving spend.
- Rolling back a broken agent version requires manual intervention.

---

## 2. Product Vision

**Rasa is the operating system for enterprise conversational AI.** It gives teams full ownership of their agents, their infrastructure, and their data — while making it as fast to build and iterate as any SaaS tool.

---

## 3. Problem Statement

Enterprises building conversational AI face a forced tradeoff:

| Option | Pros | Cons |
|---|---|---|
| **SaaS chatbot platforms** (e.g., vendor-hosted) | Fast to start, low ops burden | Vendor lock-in, data leaves your control, limited LLM choice, per-conversation pricing scales poorly |
| **DIY with LLM APIs + frameworks** | Full control | No orchestration, no observability, no governance; months of undifferentiated engineering |

Rasa eliminates this tradeoff by providing a **self-hosted, LLM-agnostic orchestration layer** purpose-built for enterprise conversational AI.

---

## 4. Product Scope

### 4.1 Agent Authoring

| Requirement | Priority | Description |
|---|---|---|
| **CALM (Conversational AI with Language Models) framework** | P0 | Declarative agent definition using flows, slots, and business logic — not rigid intent/entity trees. |
| **Multi-turn dialogue management** | P0 | State machine + LLM hybrid: deterministic paths for regulated workflows, LLM flexibility for open-ended conversation. |
| **Visual flow editor** | P1 | Browser-based UI for conversation designers to author, visualize, and simulate flows. No Python required for standard patterns. |
| **Component library** | P1 | Pre-built, reusable components: authentication, disambiguation, human handoff, slot filling, confirmation. |
| **Code-first extensibility** | P0 | Custom actions in Python for integrations (APIs, databases, CRMs). Hot-reloadable in dev. |
| **Multi-language support** | P1 | Agent logic defined once; NLU and response generation adapt per locale. |

### 4.2 LLM Integration Layer

| Requirement | Priority | Description |
|---|---|---|
| **LLM-agnostic connector** | P0 | Unified interface supporting OpenAI, Anthropic, Azure OpenAI, Google Vertex, AWS Bedrock, Ollama, vLLM, and any OpenAI-compatible API. |
| **Model routing** | P1 | Route different tasks (intent classification, response generation, summarization) to different models based on cost/latency/quality policy. |
| **Prompt management** | P0 | Version-controlled prompt templates with variable injection. A/B testing support. |
| **Guardrails engine** | P0 | Configurable input/output filters: PII redaction, topic boundaries, toxicity detection, hallucination checks, custom business rules. |
| **Fallback chains** | P1 | If primary LLM is unavailable or returns low-confidence, automatically fall back to secondary model or deterministic logic. |
| **Token budget controls** | P1 | Per-agent, per-conversation, and per-organization token limits with alerting. |

### 4.3 Multi-Channel Runtime

| Requirement | Priority | Description |
|---|---|---|
| **Channel abstraction** | P0 | Single agent definition serves all channels. Channel-specific rendering (voice SSML, chat rich cards, API JSON) handled at the edge. |
| **Voice** | P0 | Integration with Twilio, Genesys, Amazon Connect, Cisco, AudioCodes. Supports STT/TTS provider choice. |
| **Chat** | P0 | Web widget (embeddable), mobile SDK (iOS/Android), WhatsApp, Slack, Microsoft Teams. |
| **Internal/API** | P0 | REST and gRPC APIs for embedding agent capabilities in internal tools, email triage, document processing. |
| **Handoff to human** | P0 | Seamless escalation to live agents with full conversation context. Integrates with Zendesk, Salesforce, Genesys, ServiceNow. |

### 4.4 Deployment & Infrastructure

| Requirement | Priority | Description |
|---|---|---|
| **Self-hosted** | P0 | Runs on customer's own Kubernetes cluster (EKS, AKS, GKE, OpenShift, bare metal). Helm chart and Terraform modules provided. |
| **Air-gapped support** | P1 | Fully functional without internet access for defense, government, and high-security environments. |
| **GitOps-native** | P0 | Agent definitions stored as code (YAML + Python). CI/CD pipelines trigger training, testing, and deployment. |
| **Horizontal scaling** | P0 | Stateless runtime scales independently of model serving. Auto-scaling policies based on conversation concurrency. |
| **Multi-environment promotion** | P0 | Dev → Staging → Production pipeline with approval gates and diff previews. |
| **Zero-downtime deployments** | P0 | Blue-green or canary rollout for agent updates. Instant rollback. |

### 4.5 Observability & Analytics

| Requirement | Priority | Description |
|---|---|---|
| **Conversation explorer** | P0 | Search and replay any conversation. Filter by outcome, sentiment, escalation, error. |
| **Real-time dashboard** | P0 | Active conversations, success rate, avg. handle time, containment rate, CSAT proxy — per agent, per channel. |
| **LLM cost attribution** | P1 | Token usage and cost broken down by agent, flow, and LLM provider. |
| **Tracing** | P0 | End-to-end trace for every turn: NLU → policy → LLM call → action → response. OpenTelemetry-compatible. |
| **Alerting** | P1 | Configurable alerts on error rate spikes, latency degradation, guardrail trigger frequency. Integrates with PagerDuty, OpsGenie, Slack. |
| **Reporting API** | P1 | Export metrics and conversation data to existing BI tools (Looker, Tableau, PowerBI). |

### 4.6 Governance & Enterprise Controls

| Requirement | Priority | Description |
|---|---|---|
| **RBAC** | P0 | Role-based access: Admin, Agent Developer, Conversation Designer (visual editor only), Viewer (dashboards only), Auditor (read-only logs). |
| **SSO / SAML / OIDC** | P0 | Enterprise identity provider integration. |
| **Audit log** | P0 | Immutable log of every configuration change, deployment, and access event. |
| **PII handling** | P0 | Configurable PII detection and redaction in logs, analytics, and LLM prompts. Supports custom PII patterns. |
| **Data retention policies** | P1 | Configurable per-agent conversation data retention with automated purge. |
| **Approval workflows** | P1 | Require sign-off from designated reviewers before agent changes reach production. |

---

## 5. Architecture Overview (Logical)

```
┌─────────────────────────────────────────────────────────────┐
│                     RASA PLATFORM                           │
│                                                             │
│  ┌──────────┐  ┌──────────────┐  ┌────────────────────┐    │
│  │  Studio   │  │   CLI / Git  │  │    Admin API       │    │
│  │ (Visual)  │  │  (Code-first)│  │   (Programmatic)   │    │
│  └─────┬─────┘  └──────┬───────┘  └─────────┬──────────┘    │
│        └───────────┬────┴────────────────────┘              │
│                    ▼                                        │
│  ┌─────────────────────────────────────────────────────┐    │
│  │              Agent Registry & Config Store           │    │
│  └─────────────────────┬───────────────────────────────┘    │
│                        ▼                                    │
│  ┌─────────────────────────────────────────────────────┐    │
│  │              Dialogue Orchestration Engine            │    │
│  │   ┌──────────┐  ┌──────────┐  ┌──────────────────┐  │    │
│  │   │ NLU      │  │ Policy / │  │  LLM Connector   │  │    │
│  │   │ Pipeline │  │ Flow Eng │  │  + Guardrails    │  │    │
│  │   └──────────┘  └──────────┘  └──────────────────┘  │    │
│  └─────────────────────┬───────────────────────────────┘    │
│                        ▼                                    │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌──────────┐    │
│  │  Voice   │  │   Chat   │  │   API    │  │  Email   │    │
│  │ Gateway  │  │ Gateway  │  │ Gateway  │  │ Gateway  │    │
│  └──────────┘  └──────────┘  └──────────┘  └──────────┘    │
│                                                             │
│  ┌─────────────────────────────────────────────────────┐    │
│  │          Observability & Analytics Layer              │    │
│  │   Tracing · Metrics · Logs · Conversation Explorer   │    │
│  └─────────────────────────────────────────────────────┘    │
└─────────────────────────────────────────────────────────────┘
         │                              │
         ▼                              ▼
┌──────────────┐              ┌──────────────────┐
│ Customer's   │              │ Customer's LLM   │
│ Infra (K8s)  │              │ (Any provider)   │
└──────────────┘              └──────────────────┘
```

---

## 6. Success Metrics

| Metric | Target | Measurement |
|---|---|---|
| **Time to first production agent** | < 4 weeks from platform deployment | Customer onboarding tracking |
| **Agent containment rate** | > 80% of conversations resolved without human escalation | Platform analytics |
| **Uptime** | 99.95% platform availability | Internal SLA monitoring |
| **LLM provider switch time** | < 1 day to swap primary LLM for an agent | Customer-reported |
| **Conversation designer autonomy** | > 60% of agent changes made without engineering | Commit/change attribution |
| **Mean time to diagnose** | < 15 min from issue detection to root cause identification | Tracing + incident log |

---

## 7. Competitive Positioning

| Dimension | Rasa | SaaS Chatbot Vendors | DIY (LangChain + custom) |
|---|---|---|---|
| Infrastructure ownership | Full (self-hosted) | None (vendor cloud) | Full but unmanaged |
| LLM flexibility | Any provider, any model | Usually 1–2 providers | Any, but manual wiring |
| Dialogue management | Hybrid (deterministic + LLM) | Varies, often LLM-only | None built-in |
| Multi-channel | Unified | Partial | Build your own |
| Governance / RBAC | Built-in | Varies | Build your own |
| Observability | Integrated tracing + analytics | Dashboard only | Build your own |
| Time to production | Weeks | Days (but limited) | Months |

---

## 8. Release Strategy

### Phase 1 — Foundation

- CALM dialogue engine with LLM connectors (OpenAI, Anthropic, Azure OpenAI)
- CLI-first authoring, YAML flows, Python custom actions
- Chat channel (web widget + REST API)
- Helm-based Kubernetes deployment
- Conversation explorer + basic dashboard
- RBAC + SSO

### Phase 2 — Scale

- Visual flow editor (Rasa Studio)
- Voice channel integrations (Twilio, Genesys)
- Model routing and fallback chains
- Guardrails engine v1 (PII redaction, topic boundaries)
- LLM cost attribution dashboard
- GitOps CI/CD templates

### Phase 3 — Enterprise Hardening

- Air-gapped deployment support
- Approval workflows and promotion pipelines
- Advanced guardrails (hallucination detection, custom business rules)
- Reporting API + BI tool connectors
- Multi-language agent support
- Component marketplace

---

## 9. Open Questions

| # | Question | Owner | Status |
|---|---|---|---|
| 1 | Should the visual editor support custom action authoring, or only flow-level design? | Product | Open |
| 2 | What is the licensing model — per-agent, per-node, per-conversation, or flat enterprise? | Business | Open |
| 3 | How deep should built-in STT/TTS support go vs. deferring to partner integrations? | Engineering | Open |
| 4 | Should Rasa provide a managed cloud option alongside self-hosted, or stay self-hosted only? | Strategy | Open |
| 5 | What level of backward compatibility is required with Rasa Open Source 3.x agent definitions? | Engineering | Open |

---

## 10. Appendix: Glossary

| Term | Definition |
|---|---|
| **CALM** | Conversational AI with Language Models — Rasa's framework for building LLM-native dialogue agents with deterministic control where needed. |
| **Flow** | A declarative, YAML-defined conversation path that specifies steps, conditions, slot collection, and branching logic. |
| **Slot** | A named variable that stores information collected during a conversation (e.g., `account_number`, `intent_confirmed`). |
| **Custom Action** | A Python function that executes business logic (API calls, database queries) during a conversation turn. |
| **Guardrail** | A filter applied to LLM inputs or outputs to enforce safety, compliance, or quality rules. |
| **Containment Rate** | Percentage of conversations resolved by the AI agent without escalation to a human. |
| **Channel Gateway** | An adapter layer that translates between the platform's internal message format and a specific channel's protocol (e.g., Twilio, WhatsApp). |
