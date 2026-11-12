# Resource Study — PayPal AI Hackathon → CoAi Payment SKILL

**Research snapshot:** 2026-11-13. **Evidence levels:** D = provider primary documentation reviewed; O = PayPal's organizer blog/event resource list; I = engineering inference; U = unverified (page or video contents unavailable). Do **not** claim sponsor integration, trial credits or webinar attendance from a listing alone.

## Research method and limits
The PayPal event's resource directory and sponsor names were returned through Devpost's connected event overview. PayPal's own event blog describes sponsor roles. Several individual Devpost sponsor detail URLs and four YouTube videos were not retrievable from the research environment; their exact offers, technical demos and transcripts remain **UNVERIFIED**. We record webinar titles and conditional study questions, **not invented key takeaways**. Validate fresh terms on vendor websites before use.

Primary organizer context: https://developer.paypal.com/community/blog/PayPal_AI_Hackathon/
Event directory: https://paypalaihackathon.devpost.com/resources
Official PayPal APIs: https://developer.paypal.com/api/rest/

## Ten sponsor resources — applicability, value and kill criteria

| Sponsor (official event link) | Organiser-described role / confidence | CoAi candidate use (our inference) | Decision / falsification |
| --- | --- | --- | --- |
| [AG Grid](https://paypalaihackathon.devpost.com/details/aggrid) | Interactive dashboards, AG Studio, grid/chart use [O] | Merchant reconciliation grid, duplicate/hold filtering, discrepancy reviews | **P2** only if users cannot complete reconciliation using a basic table |
| [APIMatic](https://paypalaihackathon.devpost.com/details/apimatic) | API/context plugin for PayPal [O] | Typed API reference, developer agent assistance and API-client validation | **P1** research; reject if generated endpoint semantics cannot be corroborated against PayPal docs |
| [Astropods](https://paypalaihackathon.devpost.com/details/astropods) | Agent deployment/infrastructure [O] | Isolated AI anomaly agent runtime (read-only) | **P3** defer; no need if existing infrastructure meets audit/latency goals |
| [Bryntum](https://paypalaihackathon.devpost.com/details/bryntum) | Gantt/Scheduler/Calendar/Grid components [O] | 28-day settlement eligibility calendar and review queue | **P3** defer until a basic queue demonstrably fails |
| [Channel3](https://paypalaihackathon.devpost.com/details/channel3) | Product-data access API [O] | Possible SKU enrichment, receipt/item identity | **P3** not required for payment integrity; reject if it adds unlicensed product data or unnecessary PII |
| [Elastic](https://paypalaihackathon.devpost.com/details/elastic) | Hybrid/vector search [O] | Search audit evidence, disputes and receipts for AI explanations | **P2** only after query volume/search quality justifies added infrastructure |
| [KERNEL](https://paypalaihackathon.devpost.com/details/kernel) | Browser-acting agent cloud infrastructure [O] | Human-supervised merchant portal research, **never** unattended payment execution | **P3** defer on credential and action-risk grounds |
| [Postman](https://paypalaihackathon.devpost.com/details/postman) | API design/testing/docs [O] | Sandbox API collection, error scenarios, integration contract checks | **P1** best immediate fit, with secrets held in local secure environments |
| [Render](https://paypalaihackathon.devpost.com/details/render) | Hosting and workflows [O] | Isolated staging API, webhook receiver, async reconciliation | **P2** conditional; need durable store and verified HTTPS delivery before deployment |
| [Zapier](https://paypalaihackathon.devpost.com/details/zapier) | Integration automation [O/I] | Send **redacted** alerts after reconciled ledger events | **P3** no control over capture/refunds/payouts, no unrestricted finance actions |

**Priorities**: P1 = use first in research/build; P2 = validate when a real requirement exists; P3 = park. Vendor trial or licensing terms are not assumed; inspect current pages and contracts before integration.

PayPal's event blog discusses APIMatic, Render, Kernel, Channel3, Postman, Astropods, Elastic, AG Grid and Bryntum: https://developer.paypal.com/community/blog/PayPal_AI_Hackathon/ . Zapier is in the Devpost official resource list. These are descriptive categories, not integration test results.

## Four webinar entries — watch/read status

| Date & link | Organizer title | Research status | Study task (not claimed content) |
| --- | --- | --- | --- |
| [Oct 6 recording](https://www.youtube.com/watch?v=FFJfCW9z3Do) | Start building with PayPal | [U] video/transcript not accessible | Extract OAuth/Orders sandbox flow and compare current docs; record timestamped notes after watching |
| [Oct 7 recording](https://youtu.be/zRWixN68tDg) | Power Your PayPal Hackathon Project with APIMatic Context Plugins | [U] | Validate APIMatic discovery against provider docs, collect one exact API-method example and failure mode |
| [Oct 12 recording](https://www.youtube.com/watch?v=LGfG44i1Fj8) | Build a payments dashboard without building a dashboard | [U] | Determine demo method, possible data source, licensing and security implications |
| [Oct 13 recording](https://youtu.be/ck_zyB66p_Y) | Start building with PayPal | [U] | Extract any distinct checkout or troubleshooting flow; cite timestamps before inclusion |

**Completion protocol:** after manually watching, append for each video date accessed, timestamps, actual demonstrated calls, version, evidence screenshot/reference, misleading/outdated advice and a reproduction result. Without this, mark `NOT_STUDIED`.

## Core PayPal integration — study notes

1. [Developer docs](https://developer.paypal.com/) [D]: navigate platform docs; validate region, business eligibility and app setup.
2. [Payments](https://developer.paypal.com/payments/) [O/D navigation]: top-level entry; choose payment product only after checking country and fit.
3. [Checkout](https://developer.paypal.com/checkout/) [O/D navigation]: separate payer approval UI from merchant-side capture authority.
4. [JavaScript SDK v6](https://developer.paypal.com/sdk/js/) [D]: current version supports multiple checkout methods; never put client secret in browser. No SDK integration claimed.
5. [REST API](https://developer.paypal.com/api/rest/) [D]: OAuth client credentials, JSON response/error handling and sandbox endpoint.
6. [Developer Dashboard](https://developer.paypal.com/dashboard/) [D]: retrieve Sandbox Business app and Personal buyer; never publish credentials.
7. [Sandbox sample](https://github.com/paypaldev/getting-started-with-paypal-sandbox) [O]: reference repo linked by organizer; verify source/branch before adapting.

**CoAi evidence:** historical local reports show Sandbox OAuth HTTP 200, create order HTTP 201, and user-reported buyer approval; no independently reviewed capture-completed receipt or live payment.

## Eight payment capabilities — stage suitability

| Capability | Official reference | CoAi decision and risk |
| --- | --- | --- |
| Payment Links & Buttons | https://developer.paypal.com/payment-links-buttons/overview | Useful no-code benchmark, **not** a replacement for verifiable commission attribution |
| Pay Later | https://developer.paypal.com/pay-later/overview | Defer; region/product availability and finance obligations require checking |
| Subscriptions | https://developer.paypal.com/platforms/subscriptions | Later recurring TrustOS SaaS billing; cancel/failed-payment handling first |
| Invoicing | https://developer.paypal.com/invoicing/about | Optional SME invoicing; separate invoice status from capture status |
| Payouts | https://developer.paypal.com/docs/payouts/ | High-risk money movement: **human approval and merchant/country capability gate** |
| Disputes | https://developer.paypal.com/platforms/disputes/overview | High-priority negative lifecycle and commission-hold signal |
| Donations | https://developer.paypal.com/sdk/donate/ | Out of MVP; charitable collection has distinct compliance/eligibility |
| Webhooks | https://developer.paypal.com/api/webhooks/overview | Essential; verify authenticity, unique event IDs, retries and disorder |

PayPal's webhook docs distinguish event receipt from verification and describe retries; business rules must not trust an unsigned event. See https://developer.paypal.com/api/webhooks/overview .

## Five AI/agent sources

| Source | Focus | CoAi application |
| --- | --- | --- |
| https://github.com/paypal/AI-Toolkit | AI-assisted development plugin, skill/hooks/MCP [D] | Learning source, no upstream code copied |
| https://docs.paypal.ai/ | AI documentation index [D] | Discover versioned agent concepts |
| https://docs.paypal.ai/developer/tools/ai/agent-toolkit-quickstart | Python/TypeScript agents, framework options [D] | Build limited read-only anomaly explanation tool |
| https://docs.paypal.ai/developer/tools/ai/mcp-quickstart | Local/remote MCP modes [D] | Evaluate controlled tool permissions and auth |
| https://docs.paypal.ai/developer/tools/ai/agent-tools-ref | Tool categories (catalog, payments, disputes, reporting, invoices, subscriptions, tracking) [D] | Build explicit allowlist, disallow write-capable finance tools |

**Critical anti-pattern:** Reproducing the official AI Toolkit by copying upstream files is not the goal. This is CoAi's own policy SKILL with original instructions and links.

## Decision: integrate / monitor / reject
- **Integrate now:** PayPal Sandbox REST, Postman-style API testing, deterministic commission engine; AI Guardian **read-only proposals** with tests.
- **Evaluate after first customer evidence:** AG Grid and Render; Elastic only when audit search proves difficult; APIMatic if API integration maintenance saves real effort.
- **Park:** Astropods, Bryntum, Channel3, KERNEL, Zapier unless a specific measured requirement emerges.
- **Reject at MVP:** Pay Later, Payouts automation, production credentials, browser-agent money movement and unreviewed external datasets.

## Source handling and quality
- Provider documentation was reviewed at a high level; **do not infer that every subsection or endpoint was tested**.
- Sponsor detail pages and YouTube recordings were inaccessible, so fine-grained sponsor offers or webinar assertions are intentionally omitted.
- Links can change; revalidate version, business eligibility, pricing and regional coverage before feature commitment.
- Classification [O] means organizer described the role, **not** vendor security or product test.
