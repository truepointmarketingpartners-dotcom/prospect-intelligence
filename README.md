# TruePoint Prospect Intelligence & Claim Grounding

Pay-per-call APIs for AI agents. Pay **USDC on Base** per call with **x402**. No API key, no signup. People can pay by card instead.

- **Prospect Intelligence:** work-email lookup and company research for sales agents.
- **Claim Grounding:** checks factual claims against public sources and returns a verdict, confidence and citations.

**Accuracy results:** https://claim-grounding.prospectintel.workers.dev/accuracy (92 of 100 hard claims right on Deep verify, 0 confidently wrong; 38 of 38 found emails exactly right). Read the limitations there too.

## 30-second quickstart

**Option A: Coinbase agentic wallet (`awal`)**

```bash
npx awal@latest status          # sign in / check wallet
npx awal@latest balance         # needs USDC on Base
npx awal@latest x402 pay https://claim-grounding.prospectintel.workers.dev/v1/verify_claim \
  -X POST -d '{"claim":{"text":"The Eiffel Tower is in Paris"}}' --max-amount 1000000
```

`--max-amount` is in USDC atomic units (1000000 = $1.00), so you can never pay more than you set.

**Option B: any x402 client.** Call the endpoint with no payment. You get `HTTP 402` with the price in the body and a `payment-required` header. Sign the payment with your x402 client and retry. Settlement uses the Coinbase CDP facilitator on Base (`eip155:8453`).

**Option C: MCP.** Point an MCP client (streamable HTTP) at:

- `https://claim-grounding.prospectintel.workers.dev/mcp`
- `https://prospect-intelligence.prospectintel.workers.dev/mcp`

`tools/list` is free. Tool calls return x402 payment instructions; the paid HTTP route does the work.

**Option D: pay by card (people).** Stripe checkout, no wallet needed:

| Product | Price | Link |
| --- | --- | --- |
| Claim check | $0.50 | https://buy.stripe.com/00w5kDcBN8ri9Go6NP0co02 |
| Deep verify | $2.00 | https://buy.stripe.com/9B614n1X9bDuf0I2xz0co06 |
| Email lookup | $0.50 | https://buy.stripe.com/4gMcN56dp36Y05O4FH0co05 |

Card prices are higher than agent prices because Stripe has a $0.50 minimum and per-charge fees.

## Prices (USDC per call)

### Claim Grounding: `https://claim-grounding.prospectintel.workers.dev`

| Method | Path | Price | What it does |
| --- | --- | --- | --- |
| POST | `/v1/extract_claims` | $0.05 | Split text into checkable claims |
| POST | `/v1/verify_claim` | $1.00 | Check one claim (Wikipedia) |
| POST | `/v1/check_answer` | $1.00 | Split an AI answer into claims and check each (Wikipedia) |
| POST | `/v1/deep_verify` | $0.75 | Check against six sources: Wikipedia, Wikidata, Google News titles, Europe PMC, FRED, SEC EDGAR. Async: poll `GET /v1/jobs/:id` (free) |

Verdicts: `supported`, `contradicted`, `outdated`, `mixed`, `insufficient_evidence`.

### Prospect Intelligence: `https://prospect-intelligence.prospectintel.workers.dev`

| Method | Path | Price | What it does |
| --- | --- | --- | --- |
| POST | `/v1/email` | $0.08, **charged only when an address is found** | One work email for a name at a domain, with verification status, source and a `send` / `send_with_caution` / `verify_first` / `do_not_send` recommendation |
| POST | `/v1/basic` | $0.10 | `research_company` |
| POST | `/v1/enriched` | $0.75 | `enrich_prospect` |
| POST | `/v1/tools` | $0.75 | Any one named tool: research_company, research_person, find_decision_makers, analyze_marketing_opportunity, identify_buying_signals, enrich_prospect, score_prospect, generate_prospect_brief |
| POST | `/v1/deep` | $3.00 | Full prospect brief with a 0–90 score |

## Sample request and response

**Request** (real, paid, Oct 12, 2026):

```bash
npx awal@latest x402 pay https://claim-grounding.prospectintel.workers.dev/v1/verify_claim \
  -X POST -d '{"claim":{"text":"The Eiffel Tower is in Paris"}}'
```

**Response** (HTTP 200, shortened with `…`):

```json
{
  "mode": "fast",
  "job_id": null,
  "claim": {
    "claim_id": "clm_14b3ffa736fd4fd1",
    "text": "The Eiffel Tower is in Paris",
    "span": { "start": 0, "end": 29 },
    "grounding_required": true,
    "verdict": "supported",
    "status_detail": "supported",
    "confidence": 0.95,
    "citations": [
      {
        "url": "https://en.wikipedia.org/wiki/Eiffel_Tower",
        "title": "Eiffel Tower",
        "quote": "The Eiffel Tower is a lattice tower on the …",
        "retrieved_at": "…",
        "provider": "wikipedia"
      }
    ],
    "rationale_short": "…"
  },
  "why": "…",
  "source": "https://en.wikipedia.org/wiki/Eiffel_Tower",
  "receipt": { "claim_hashes": ["sha256:…"], "evidence_urls": ["https://en.wikipedia.org/wiki/Eiffel_Tower"], "verdicts": ["supported"], "priced_usd": 1, "created_at": "…" }
}
```

Paid on Base: [tx 0x839f…a901](https://basescan.org/tx/0x839f3413d7f73434e982cbcd918f06c04f6c772f74faa3c7b9622943eab5a901).

**The unpaid first call** (real, live) returns `HTTP 402`:

```json
{
  "x402Version": 2,
  "error": "Payment required",
  "accepts": [{
    "scheme": "exact",
    "network": "eip155:8453",
    "amount": "80000",
    "asset": "0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913",
    "payTo": "0x485FfC037f02dDDc154A454a9cA89C4B1b29e25b",
    "maxTimeoutSeconds": 300,
    "extra": { "name": "USD Coin", "version": "2" }
  }]
}
```

(`80000` = $0.08 for `/v1/email`. The asset is USDC on Base.)

## Errors

| Status | When | Charged? |
| --- | --- | --- |
| 200 | Success | Yes |
| 402 | No or invalid payment. Body and `payment-required` header list what to pay | No |
| 400 | Bad input, e.g. missing `claim.text`, or `mode: "deep"` sent to verify_claim (use `/v1/deep_verify`). On email: missing domain or names | No |
| 404 | Email lookup found no address (`no_result`), unknown path, or unknown job id | No |
| 405 | Non-POST request to `/mcp` | No |
| 409 | Card payment already used for a lookup (one payment = one lookup) | No |
| 422 | Email provider rejected the input (e.g. domain has no mail records) | No |
| 451 | Email lookup: the person opted out of data processing. Do not contact or store | No |
| 502 / 503 | Upstream source or email provider unavailable. Safe to retry later | No |

x402 only settles payment on a 2xx response, so failed calls cost nothing.

## What these tools don't do

- Not legal, medical, financial or compliance advice. Not a certification.
- English only. Public sources only; no paywalled or private documents.
- Can return `insufficient_evidence`. Very recent news may lag.
- No guarantee an email is delivered or read. No personal/private emails, no phone numbers.
- Decision-makers are `unknown` unless a public source names them. Nothing is invented.

## Discovery

`/docs` · `/.well-known/x402` · `/openapi.json` · `/llms.txt` · `/.well-known/mcp/server-card.json` on each host. Listed on [agentic.market](https://agentic.market).

Worker source stays private; this repo is the public surface for agents and builders.

## Support

- Email: truepointmarketingpartners@gmail.com
- Phone: (951) 234-7620
- Site: https://www.truepointmarketingpartners.com/ai-agent-tools
