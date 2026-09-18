# Prospect Intelligence & Claim Grounding

Paid HTTP APIs for agents on **Base**. Pay **USDC per call** via **x402**. No API key.

Product page: [AI Agent Tools](https://www.truepointmarketingpartners.com/ai-agent-tools)

## The 5 primary endpoints

### Prospect Intelligence

Base: https://prospect-intelligence.prospectintel.workers.dev

| Method | Path | Price | Tool |
| --- | --- | --- | --- |
| `POST` | `/v1/basic` | **$0.10** USDC | `research_company` |
| `POST` | `/v1/enriched` | **$0.75** USDC | `enrich_prospect` |

Full URLs:
- https://prospect-intelligence.prospectintel.workers.dev/v1/basic
- https://prospect-intelligence.prospectintel.workers.dev/v1/enriched

### Claim Grounding

Base: https://claim-grounding.prospectintel.workers.dev

| Method | Path | Price |
| --- | --- | --- |
| `POST` | `/v1/extract_claims` | **$0.05** USDC |
| `POST` | `/v1/verify_claim` | **$0.10** USDC |
| `POST` | `/v1/check_answer` | **$0.10** USDC |

Full URLs:
- https://claim-grounding.prospectintel.workers.dev/v1/extract_claims
- https://claim-grounding.prospectintel.workers.dev/v1/verify_claim
- https://claim-grounding.prospectintel.workers.dev/v1/check_answer

Same wallet · Base USDC · x402.

## Docs & discovery

- Prospect Intelligence docs: https://prospect-intelligence.prospectintel.workers.dev/docs
- Claim Grounding docs: https://claim-grounding.prospectintel.workers.dev/docs
- `/.well-known/x402` · `/openapi.json` · `/llms.txt` on each host

## Notes

Worker **source** stays private. This public repo is the discoverable surface for agents and builders.

MCP stays local — this is not a public remote MCP claim. Not certified, hallucination-proof, or legal-grade.

Listed on [agentic.market](https://agentic.market) as Prospect Intelligence and Claim Grounding.
