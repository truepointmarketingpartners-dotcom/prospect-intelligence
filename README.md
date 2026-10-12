# Prospect Intelligence & Claim Grounding

Paid HTTP APIs for agents on **Base**. Pay **USDC per call** via **x402**. No API key.

Product page: [AI Agent Tools](https://www.truepointmarketingpartners.com/ai-agent-tools)

## Prospect Intelligence

Base: https://prospect-intelligence.prospectintel.workers.dev

| Method | Path | Price (USDC) | Tool |
| --- | --- | --- | --- |
| `POST` | `/v1/basic` | **$0.10** | `research_company` |
| `POST` | `/v1/enriched` | **$0.75** | `enrich_prospect` |
| `POST` | `/v1/deep` | **$3.00** | deep company brief |
| `POST` | `/v1/tools` | **$0.75** | any listed tool by name |
| POST | /v1/email | **$0.08**, charged only when an address is returned | email_lookup |

**Email lookup (v1.8.0):** returns Hunter's real verification status (`valid`, `accept_all`, `unknown`, …), whether the address was found on a public source or inferred from the company's pattern, source URLs with first/last seen dates, the confidence score, and a plain recommendation (`send`, `send_with_caution`, `verify_first`, `do_not_send`). Addresses are never labeled "verified" unless Hunter says so. No result returns 404 and costs $0. Body: `domain`, `first_name`, `last_name` (optional `found_only: true`).

## Claim Grounding

Base: https://claim-grounding.prospectintel.workers.dev

| Method | Path | Price (USDC) |
| --- | --- | --- |
| `POST` | `/v1/extract_claims` | **$0.05** |
| `POST` | `/v1/verify_claim` | **$1.00** |
| `POST` | `/v1/check_answer` | **$1.00** |
| POST | /v1/deep_verify | **$0.75** (async; poll GET /v1/jobs/:id, free) |
| `POST` | `/v1/deep_research/lite` | **$5.00**, requires confirmation |
| `POST` | `/v1/deep_research` | **$8.00**, requires confirmation |
| `POST` | `/v1/deep_research/pro` | **$15.00**, requires confirmation |

## Pay by card (people)

Stripe checkout, $0.50 minimum: https://prospect-intelligence.prospectintel.workers.dev/buy and https://claim-grounding.prospectintel.workers.dev/buy

## Docs & discovery

- Prospect Intelligence docs: https://prospect-intelligence.prospectintel.workers.dev/docs
- Claim Grounding docs: https://claim-grounding.prospectintel.workers.dev/docs
- `/.well-known/x402` · `/openapi.json` · `/llms.txt` on each host

## Notes

Same wallet · Base USDC · x402 (Coinbase CDP facilitator). Worker **source** stays private; this public repo is the discoverable surface for agents and builders.

MCP stays local. This is not a public remote MCP claim. Not certified, hallucination-proof, or legal-grade.

Listed on [agentic.market](https://agentic.market) as Prospect Intelligence and Claim Grounding.
