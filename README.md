Accuracy results
TruePoint Marketing Partners · Tests run October 12, 2026 · Claim Grounding 1.4.5 · Prospect Intelligence 1.8.6

We publish summary numbers, how we tested, and what our tools do not do. The test items themselves stay private so nobody (including us) can tune the tools to the published set. Results are from small, self-built tests. Read the limitations below before relying on them.

Claim Grounding: Deep verify
92 / 100
hard claims correct (live service)
0
confidently wrong answers (wrong at confidence ≥ 0.7)
15 / 17
fresh held-out claims, never used in development
Test set	Version	Correct	Confidently wrong
Hard set (100 claims)	1.4.5 (live)	92 / 100	0
Hard set (100 claims)	1.4.4 (previous)	45 / 100	20
Fresh held-out set (17 claims)	1.4.5	15 / 17	0
Original set (30 claims)	1.4.5	25 / 30	—
All 8 misses on the hard set came back as insufficient_evidence or mixed, never as a confident wrong verdict.

Hard set by category (live 1.4.5)
Category	What it exercises	Correct
FRED economic data	Rates, prices and other U.S. economic series	13 / 13
SEC EDGAR filings	Reported company financials	15 / 15
Wikidata facts	Structured facts: dates, places, officeholders	16 / 16
Wikipedia	General encyclopedic facts	9 / 9
Outdated claims	Facts that were true once but changed	7 / 10
Negation	"X is not Y" claims	4 / 5
Compound	Two-part claims that must both hold	5 / 5
Europe PMC	Medical and scientific research findings	10 / 12
Recent news	Recent events from news headlines	5 / 7
Made-up	Fabricated people, companies or events	8 / 8
Total		92 / 100
How we tested
Deep verify checks each claim against six public sources: Wikipedia, Wikidata, Google News headlines, Europe PMC, FRED (Federal Reserve Economic Data) and SEC EDGAR.
TruePoint wrote the claims and decided the correct answer for each one. Numbers and dates for FRED and SEC claims were checked against the primary data. Claims were deliberately hard: wrong dates, swapped names, outdated facts, negations, two-part claims and invented entities.
A claim counts as correct when the verdict matches the expected answer. "Confidently wrong" means a wrong verdict with confidence of 0.7 or higher.
The hard set was run against the live service after deployment. The 17-claim held-out set was written separately and never used while building fixes, so it is the fairest single number here.
Prospect Intelligence: Email lookup
38 / 40
real contacts found
38 / 38
found addresses exactly right
0
made-up names labeled "send"
Result	Count
Real contacts found (40 tested)	38 / 40
Found addresses that were exactly right	38 / 38 (0 wrong)
"send" label correct	26 / 26
"send_with_caution" label correct (re-scored under the 1.8.6 rule)	10 / 10
Made-up names (10 tested) labeled "send"	0
How we tested
40 real people whose work emails are published as press or media contacts, plus 10 invented names at real company domains.
An address counts as right only if it exactly matches the published address.
Version 1.8.6 changed one rule after this test: an address that was only guessed from a company's pattern, on a domain whose mail server accepts any address (a "catch-all" domain), is now labeled verify_first instead of send_with_caution. The label counts above are scored under that rule.
Published press contacts are easier to find than the average person, so expect a lower find rate on ordinary lists.
What these tools don't do
Claim Grounding
It is not legal, medical, financial or compliance advice, and not a certification that something is true.
It works in English only and checks public sources only. It cannot see paywalled, private or internal documents.
It can and will return insufficient_evidence when public sources don't settle a claim. That is a feature, not a failure, but it means not every claim gets a yes or no.
Very recent news may lag: it relies on news headlines, and sources take time to update.
Verify claim and Check answer use Wikipedia only. Only Deep verify uses all six sources.
It checks facts, not opinions, predictions or tone.
Prospect Intelligence
It does not guarantee an email will be delivered or read. "valid" means the company's mail server accepted the address, not that a person reads it.
It finds work emails at a company domain. It does not find personal or private emails, and it does not return phone numbers.
Decision-makers are reported as unknown unless a public source names them. Nothing is invented.
If a person has asked our email provider to stop processing their data, we return nothing for them.
Company research uses public sources (such as Wikipedia and the company's homepage) and may be incomplete or out of date.
About these numbers
These are small samples (100, 30 and 17 claims; 50 contacts) written and graded by TruePoint, not an independent audit. They show how the tools behaved on one day, on one set of items. Results on your own data may differ. We will add larger and independent tests over time and update this page with dates and versions.
Contact
Questions, bug reports or a claim we got wrong: email truepointmarketingpartners@gmail.com or call (951) 234-7620.
Product page: truepointmarketingpartners.com/ai-agent-tools

Last updated October 13, 2026 · Claim Grounding 1.4.5 · Prospect Intelligence 1.8.6
