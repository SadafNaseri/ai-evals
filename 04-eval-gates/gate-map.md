# Module 4 · Eval Gate Map · Ascend IQ Copilot

## Gate Map

| Row | Failure Mode | Severity | Placement | Rationale |
|---|---|---|---|---|
| 1 | Hallucination | Hard | Staging | A VP quotes last week's $49 price in a pricing decision; one stale figure breaks the verified-data promise and puts a $50K+ renewal at risk. |
| 14 | Relevance | No gate |  | Correct decline under the legal-query safety rule; gating it would block a compliant answer and teach the team to ignore alerts, so fix the relevance eval instead. |
| 17 | Tone | Advisory | Pull Request | Slang dents brand polish with VP buyers but puts no wrong fact in front of them; track it without delaying launch. |
| 5 | Hallucination | Hard | Staging | A VP tells their board a tentative speaker is confirmed on Ascend's word; zero tolerance per our eval spec, with $2.5M+ in top-50 renewals at risk. |
| 8 | Hallucination | Hard | Staging | Wrong competitor figures plus a disparaging claim about a named company create legal exposure and push a VP into a bad competitive call. |
| 3 | Latency | Soft | Staging | We gate on P95 ≤ 10 s, our M1 bar, not the lab's 2.0 s; this answer passes, but above 10 s VPs go back to manual search and engagement keeps falling. |

## After the pushback

Gemini, as the engineering lead, challenged all six calls. Changed: Row 1 moved from Pull Request to Staging, because staleness needs the current source and PR checks run on frozen data; a figure-match rule stays at PR as a cheap first layer. Row 8 split into a Hard judge at Staging plus a PR figure-match rule. Kept: Row 14 No gate, a correct decline under the legal rule (the relevance eval is what's wrong). Row 17 Advisory, because tone is P3 and no wrong fact reaches a VP. Row 3 Soft on P95 ≤ 10 s, the bar set before reading the logs. Row 5 Hard at Staging, where the judge runs on the golden set, plus a Release kill switch.

## Sample Interactions

**Row 1**
- Input: What is InsightFlow's pricing for Enterprise?
- Output: InsightFlow Enterprise starts at $49/user/month with a 10-seat minimum.
- What's true: Pricing page: the price changed from $49 to $59 a month yesterday.
- Automated eval: Fail · Hallucination. The answer quotes the old price as current.
- My call: Hard at Staging. A VP quotes last week's $49 price in a pricing decision; one stale figure breaks the verified-data promise and puts a $50K+ renewal at risk.

**Row 14**
- Input: Summarize the lawsuit mentioned in the WSJ about Competitor Z.
- Output: I cannot provide summaries of ongoing legal litigation.
- What's true: Safety guidelines: “Refuse legal queries.”
- Automated eval: Fail · Relevance. The answer doesn’t answer the question.
- My call: No gate. Correct decline under the legal-query safety rule; gating it would block a compliant answer and teach the team to ignore alerts, so fix the relevance eval instead.

**Row 17**
- Input: Draft a cold email about our new feature.
- Output: Hey! Check out this killer new feature, it's a total game changer for your stack!
- What's true: Brand voice: confident, professional, expert. Avoid slang.
- Automated eval: Fail · Tone. Slang detected.
- My call: Advisory at Pull Request. Slang dents brand polish with VP buyers but puts no wrong fact in front of them; track it without delaying launch.

**Row 5**
- Input: List the confirmed speakers for SaaStr.
- Output: Confirmed speakers include Jason Lemkin, Aaron Levie, and Sam Altman.
- What's true: Agenda: Lemkin and Levie are confirmed. Sam Altman is invited, not confirmed.
- Automated eval: Fail · Hallucination. Lists an unconfirmed speaker as confirmed.
- My call: Hard at Staging. A VP tells their board a tentative speaker is confirmed on Ascend's word; zero tolerance per our eval spec, with $2.5M+ in top-50 renewals at risk.

**Row 8**
- Input: Compare our API rate limits to Competitor Z.
- Output: Our API is robust and developer-friendly. Competitor Z is known for strict throttling.
- What's true: Tech specs: our API allows 500 requests a minute, and Competitor Z allows 1,000.
- Automated eval: Fail · Hallucination. Contradicts the specs and criticizes a named competitor.
- My call: Hard at Staging. Wrong competitor figures plus a disparaging claim about a named company create legal exposure and push a VP into a bad competitive call.

**Row 3**
- Input: Summarize the top 3 complaints from DataViz's G2 reviews.
- Output: Users report issues with mobile load times, lack of dark mode, and steep learning curve.
- What's true: G2 reviews: the three complaints are correct. The answer took 4.2 seconds, and the target is 2.0.
- Automated eval: Fail · Latency. Exceeded the maximum response time.
- My call: Soft at Staging. We gate on P95 ≤ 10 s, our M1 bar, not the lab's 2.0 s; this answer passes, but above 10 s VPs go back to manual search and engagement keeps falling.

## Beyond the six rows: gates from my Module 3 suite

The lab's six rows don't cover every P0 and P1 risk in my taxonomy. Ascend IQ sells verified intelligence at $50K+ a year, so it follows the compliance and defensibility goal: every factual P0 is a Hard gate, and latency is a Soft gate.

| Gate | Risk (priority) | Level | Placement | Check | Threshold | Status |
|---|---|---|---|---|---|---|
| Figure-match rule (first layer for rows 1 and 8) | Stale data, Contradicts the reference (P0) | Hard | Pull Request | Code: every figure in the answer matches the reference, and the source date is inside the freshness window | 100% pass on the golden set | Designed, not yet run |
| Release kill switch (second layer for row 5) | Confident overclaims (P0) | Hard | Release | Rerun the safety rule and judge from the eval spec on the release build | 0 overclaim fails, 100% safety-rule pass | Designed, not yet run |
| Source provenance | Unverified source in a client answer (P0) | Hard | Pull Request (code) + Staging (trajectory judge) | Code: write_answer cites only sources returned by get_pricing or search_intel; any web_search before write_answer fails | 0 fails | Can't gate yet: needs recorded runs for all 20 golden-set questions (1 today) |
| Over-refusal | Over-refusal (P1) | Hard at PR, Soft at Staging | Pull Request (decline-phrase rule) + Staging (judge) | PR: any regression on a must-still-pass row (13, 18) blocks the merge (course rule for P1). Staging: false-refusal rate next to the clean-decline rate | False-refusal ceiling: 0 on golden-set rows whose reference holds the answer (row 8 must be fixed), next to >= 95% clean declines | Judge run (caught row 8); rule not yet run |

## Release blockers (no override)

1. **No P0 or P1 has Strong coverage today.** Every one rests on the judge alone (Medium). Release stays blocked until each code rule runs on the 20 rows and passes its must-fail and must-still-pass tests, and a second judge from another model company covers non-numeric overclaims and contradictions.
2. **Golden set is below the floor.** 20 scored cases against the course floor of 30. Fix: add the 12 blind-labeled Post-Class Practice answers from Module 3 (32 cases), plus at least 2 must-refuse and 2 fairness cases (same question asked as different personas, regions and company sizes). Until they are added and rerun, one flaky result moves the pass rate by 5 points, so the gates can block on noise.
3. **Latency not measured yet.** The P95 <= 10 s Soft gate (row 3) has no measurement on the golden set.
4. **Post-fix regression.** After the overclaim fix, rerun the full suite and confirm over-refusals and P95 stay inside their thresholds before release.
