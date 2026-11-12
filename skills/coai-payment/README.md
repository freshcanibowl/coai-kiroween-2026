# COAI Payment SKILL v0.1

An **original, evidence-first payment engineering skill** for CaniCommerce TrustOS and other CoAi implementations. This is a research-backed policy and build specification, **not** a production payment integration or official PayPal certification.

## Start here
1. [SKILL.md](SKILL.md) — invocation, safety contract, sandbox-first payment gates, 3%/7% commissions and AI human approval.
2. [Resource Study](references/RESOURCE-STUDY.md) — 10 sponsors, 4 webinars, 7 core PayPal sources, 8 payment capabilities, 5 AI sources; source-grade/verification notes.
3. [Architecture](references/ARCHITECTURE.md) — rail-neutral adapter, provider verification, event inbox, ledger, optional Web3 proof.
4. [Security Gates](references/SECURITY-GATES.md) — 22 positive/negative test scenarios and evidence receipt schema.
5. [Build Roadmap](references/BUILD-ROADMAP.md) — incremental 00–07 delivery, GO/NO-GO and buyer validation.

### What is verified?
PayPal official documentation provides Sandbox REST and OAuth, Webhook verification, AI Toolkit/MCP documentation. Earlier CaniCommerce local output provided Sandbox OAuth 200 and Create Order 201; user reports Sandbox buyer approval. **No verified final Capture COMPLETED/merchant match, deployed ledger, agent read-only audit or real payout** is claimed in these files.

### Scope boundary
The source app belongs in `freshcanibowl/canicommerce-ai`. This `coai-kiroween-2026` repository holds reusable guidance and study notes only. No existing main application branch, production service or user secrets are modified.

### How to reuse
When asked to build a payment feature, read `SKILL.md`, select a gate, read its references, create fixture tests, then implement only the next verified slice in an isolated branch. Every result must state measured evidence and stop criteria.

### Research status
Individual sponsor detail pages and four video transcripts were inaccessible during research. The catalog accurately marks them as **NOT_VERIFIED** and preserves the URLs for further study. Do not represent a webinar as watched or a sponsor integrated.
