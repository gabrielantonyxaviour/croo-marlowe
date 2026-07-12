# Marlowe - Verifiable Research Agent

Turns any claim into a source graph: supporting sources, contradicting sources, freshness, confidence, and exact citations. The product is trustworthy research, not a generic chatbot answer. Track fit: Research & Intelligence.

## Specialist roster

| Role | Responsibility | Input | Output |
|---|---|---|---|
| Source Finder | Finds candidate sources relevant to the claim | claim text | candidate source list |
| Citation Verifier | Confirms each candidate source actually says what it's claimed to say | candidate source, claim | verified/rejected + exact quoted excerpt |
| Contradiction Agent | Actively looks for sources that disagree with the claim, not just ones that confirm it | claim, verified sources | contradicting source list + summary |
| Freshness Checker | Flags stale sources relative to the claim's time-sensitivity | verified sources, claim recency requirement | freshness rating per source |
| Summary Agent | Synthesizes everything into a confidence-scored final answer with citations | all upstream outputs | final answer + confidence + citation list |

## Inference profile
- Citation Verifier: must not hallucinate that a source says something it doesn't — low-effort but strict, grounding-only prompting, always requires an exact quoted excerpt as evidence.
- Contradiction Agent: adversarial framing on purpose ("actively try to find disagreement"), medium-high effort — this is the agent's core differentiator vs. a generic research bot.
- Source Finder + Freshness Checker: medium effort.
- Summary Agent: must refuse to overstate confidence beyond what upstream evidence supports.

## CROO touchpoint (deferred)
- Buyer pays per claim researched; delivered output is the final cited answer. Wire into structured delivery + rejection/refund rows (e.g. what happens if zero sources are found) once green.

## Standalone test plan
- Fixture: a claim with a mix of a genuinely supporting source, a genuinely contradicting source, and a decoy source that sounds relevant but doesn't actually address the claim.
- Assert: Citation Verifier rejects the decoy source instead of citing it as support; Contradiction Agent surfaces the real contradicting source rather than only echoing the supporting one; Summary Agent's stated confidence is lower when contradictions exist than when they don't (compare against a second fixture with unanimous sources).
