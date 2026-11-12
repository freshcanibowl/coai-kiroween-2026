# CaniCommerce TrustOS — Reconstruct & Build Roadmap

**Goal**: transform the existing deterministic commission core into an evidence-first, payment-rail-agnostic, merchant-facing pilot without rebuilding CAIOS or pretending PayPal Sandbox proves production readiness.

## Build 00 — Baseline and freeze
- Inventory source/branches in `freshcanibowl/canicommerce-ai` and existing Windows PayPal Build01/Build02 packages.
- Record authoritative source SHA, tests, known risks and whether hackathon repo must remain frozen during judging.
- Do not migrate existing sources to CoAi merely to rename the project. CoAi is knowledge/skill governance; CaniCommerce owns the implementation.
- **Acceptance**: source manifest, reproducible deterministic commission tests and no leaked credentials.
- **Stop**: any missing provenance, secrets or production access.

## Build 01 — Payment Adapter contract (sandbox only)
- Introduce `PaymentAdapter` interface with `get_order`, `get_capture`, `get_refund`, `verify_event`. Explicit capability flags for mutation APIs; default mode is read-only.
- Validate expected merchant and order context, money in integer minor units, and constant allowlisted sandbox host.
- **Acceptance**: fixture-driven normal/negative contract tests and OAuth/create-order evidence segregated from capture evidence.
- **Stop**: unknown merchant identity or non-repeatable signature/auth validation.

## Build 02 — Capture verification + durable inbox
- Preserve user-reported approval as weak evidence until provider GET validates it.
- Persist `PayPal-Request-Id` with each attempted action, reconcile ambiguous network responses.
- Normalize webhook signatures, event IDs, event time and references in a single transaction.
- **Acceptance**: receipt verified as `COMPLETED` and exact amount/currency/merchant/order linkage; replay tests fail closed.
- **Stop**: duplicate capture/commission or unsigned webhook accepted.

## Build 03 — Commission ledger and 28-day hold
- Plug in existing 3% referrer/7% selling-partner policy with versioning and partner identity; do not pay anyone.
- Deduplicate capture-role-policy credits; post immutable refund/chargeback reversals and dispute holds.
- **Acceptance**: ledger equals independently reconciled provider net amounts across full/partial refunds and direct-sale cases.
- **Stop**: cross-tenant access, floating point drift, double-credit or orphan referrer.

## Build 04 — AI Guardian read-only explanations
- Add an adapter for a selected LLM with redacted, minimal context and tool access only to vetted read-only functions.
- Compare model against deterministic anomaly classifier for duplicate/mismatch/refund/dispute fixtures.
- Produce evidence-linked `ActionProposal` with no execution.
- **Acceptance**: negative prompt-injection tests, zero unauthorized tool calls, reproducible evaluation run.
- **Stop**: unsourced payment claims or model-initiated finance mutations.

## Build 05 — Merchant pilot UI
- One page: payments / holds / commissions / evidence / exception queue.
- Prefer basic React grid first; evaluate AG Grid after identifying a concrete usability bottleneck.
- Optional staging on Render only with durable database, webhook endpoint auth, secret manager and deployment rollback.
- **Acceptance**: merchants can explain and reconcile one sample disputed payout without developer assistance.
- **Stop**: untested permissions or unnecessary infrastructure expenses.

## Build 06 — Commercial validation
- 10 interviews; 3 genuine pilot commitments; at least 1 price-bearing pilot signal (**goals, not achievements**).
- Test subscription vs per-merchant monthly fee vs usage-based reconciliation; model provider fees, support cost, margin and churn.
- Separate marketing claims from verified metrics and signed customer evidence.
- **Acceptance**: identifiable buyer, current costly problem, buyer budget and repeat workflow.
- **Stop**: no paid interest or simple spreadsheet + rules solves the problem at lower total cost.

## Build 07 — Optional EvidenceKit/Web3 proof
- Add canonical statement digest and Merkle proof only after independent audit demand.
- No blockchain requirement for the payment path; optional anchoring must be removable without changing ledger correctness.
- **Acceptance**: external verifier can detect tampered evidence without requiring a wallet to purchase.
- **Stop**: no buyer or auditor asks for cross-party verification.

## Decisions and constraints
- Preferred stack is additive to existing CaniCommerce Python core and existing CAIOS architecture: do not rebuild.
- PayPal's developer APIs are documented, but live merchant support, payout eligibility, Malaysian rail availability and actual unit economics require separate verification.
- Sponsor stack: **Postman/APIMatic evaluate**, **AG Grid/Render conditional**, **others parked**.
- Keep hackathon-only claims separate from a commercial roadmap; no retroactive competition submissions or backdated commits.
- This plan does **not** claim any Build 01–07 implementation or acceptance test has passed.
