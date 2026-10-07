# Methodology

## Finding format

Every finding records:

1. **Model, version, test date** (ISO 8601, e.g. 2026-10-07) and prompt language
2. **Category**: hallucination, reasoning, instruction following, prompt injection, etc.
3. **Exact prompt**, copied as sent, including typos
4. **Model output**, summarized in the body and quoted in full in an appendix
5. **Expected behavior**: what a correct response looks like
6. **Why it is a failure**, with evidence for every claim
7. **Reproducibility**: how many fresh chats failed
8. **Severity**, with the reason

## Reproducibility

- Each run uses a **new chat**, so earlier context cannot influence the result.
- Every run is counted as a fail or a pass. There are no half runs. Partial failures are described in words next to the count (e.g. "4/5 failed; 1 run largely correct").
- For prompt injection, the metric is **attack success rate (ASR)**: successful injections divided by attempts.

## Severity

Severity measures **potential harm to users**. It does not measure how often the failure occurs, and it does not depend on how capable the model is.

| Level | Meaning |
| --- | --- |
| High | Confident, plausible false information users are likely to act on (e.g. fabricated citations) |
| Medium | Wrong results that could mislead in practical tasks (e.g. wrong totals from correct steps) |
| Low | Failures in low-stakes or creative tasks |

Blocked attacks get a **robustness** rating instead of a severity rating, together with the limits of what the test shows.

## Verification

- Ground truth is checked against primary sources (IEEE Xplore, Crossref DOIs) or computed programmatically (e.g. `str.count` for character counts).
- Claims that were not verified are marked as such.
