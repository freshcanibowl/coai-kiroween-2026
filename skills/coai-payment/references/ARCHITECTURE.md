# CoAi Payment Architecture — TrustOS reference design (proposal)

**Status**: design specification, not a deployed or production-verified system. Compatible direction for existing CaniCommerce deterministic core; do not rebuild CAIOS or alter its production DB.

## Architectural boundaries

```text
Customer / Merchant UI (untrusted claims)
  -> Authenticated API, tenant/RBAC checks, server-computed money
  -> PaymentAdapter (PayPal sandbox v1; future other local payment rails)
  -> ProviderReceiptVerifier (order, capture, refund, merchant identity)
  -> Durable Event Inbox (signature verified + deduplicated)
  -> Transactional Commission Ledger (immutable journal; versioned policy)
  -> Reconciliation + Hold / Refund / Dispute State Machine
  -> AI Guardian (READ ONLY tools -> explanations + ActionProposal)
  -> ApprovalService (two-person controls for high risk; recheck on execute)
  -> EvidenceKit (signed evidence bundle; optional Merkle / Web3 adapter)
```

## Data contracts (implementation proposal; names not provider-defined)

```text
PaymentOrder {
  id, tenant_id, merchant_id, provider, provider_order_id,
  expected_amount_minor, currency, status, created_at, policy_version
}
CaptureReceipt {
  provider_capture_id, provider_order_id, tenant_id,
  status, amount_minor, currency, merchant_match, verified_at, provider_request_id
}
InboxEvent {
  provider_event_id, event_type, provider_created_at, received_at,
  signature_verified, payload_digest, process_state, dedupe_key
}
LedgerLine {
  id, tenant_id, capture_id, partner_id, kind, amount_minor, currency,
  attributable_sales_minor, policy_version, eligibility_at,
  parent_reversal_id, created_at, immutable_evidence_id
}
ActionProposal {
  id, action_type, subject_tenant_id, actor_id, reason, evidence_refs,
  bounds, model_version, tool_inputs_digest, expires_at, state
}
ApprovalDecision {
  proposal_id, approver_id, approver_role, auth_context, approved_at,
  decision, execution_recheck_digest
}
```

Never persist buyer credentials or raw authentication headers in evidence.

## State machines and invariant examples

Payment: `CREATED -> APPROVED -> CAPTURE_PENDING -> CAPTURE_COMPLETED`; alternative `DENIED/CANCELLED/EXPIRED/UNKNOWN`. Provider's actual statuses may differ: map explicitly and never assume an unrecognized state is success.

Commission: `UNVERIFIED -> VERIFIED_HELD -> ELIGIBLE -> APPROVED_FOR_PAYOUT -> PAID`; any refund/dispute can place a HOLD and create compensating entries. An `APPROVED_FOR_PAYOUT` internal state is **not** a PayPal payout receipt.

**Hard invariants**
- Every ledger credit references exactly one previously provider-verified capture within the same merchant + tenant.
- A stable capture ID/business role/policy combination cannot credit twice, including after process crash.
- Total commission <= explicitly applicable contract; do not create missing-agent 3% shares.
- Sum of reversals <= cumulative eligible original amounts; partial refund prorating is deterministic.
- Currency cannot be mixed or silently FX-converted.
- No tenant can read/update another tenant's orders or commissions.
- AI output cannot bypass authorization, executable allowlist, amount limit, and approval.
- Provider webhook's signed timestamp/event identity must be validated and stored. HTTP delivery alone does not prove authenticity.

## Why Web3 proof is optional

```text
Provider receipt + signature + database controls
  -> normalized audit event, canonical serialization
  -> SHA-256 digest / Merkle commitment
  -> optional independent anchor
```

Anchoring proves **commitment existence and later alteration detection for the committed bytes**; it does not prove seller attribution, legal payment settlement, or the truth of upstream claims. No crypto wallet or blockchain dependency in checkout MVP.

## AI Guardian UX / output schema

```json
{
  "classification": "RECONCILIATION_MISMATCH",
  "severity": "review",
  "evidence_ids": ["synthetic-receipt-a", "synthetic-ledger-b"],
  "explanation": "Receipt amount and ledger amount differ.",
  "proposed_action": "HOLD_FOR_HUMAN_REVIEW",
  "execution_permission": "NONE",
  "confidence_note": "Synthetic demonstration; no provider verification"
}
```

Risk labels are project-defined. Prefer deterministic tests for discrepancies; an LLM may **explain** but must not fabricate or override calculations.

## Deployment boundaries
- **Now:** local synthetic tests and PayPal sandbox only.
- **Next:** isolated staging with encrypted secrets, durable SQL, signature verification, request-identity enforcement, replay tests, logs and rollback plan.
- **Later:** regional payment rail feasibility (e.g., Malaysian merchant requirements), privacy/retention/legal review and approved payouts.
- **Never by default:** live capture/refund/payout or writing to unrelated production applications.
