# Eval Suite · Confident overclaims

## P0 failure (carried from Module 2)

**Failure:** Confident overclaims (#HALLUCINATION). States partial, tentative or partner-only facts as confirmed.

- **Row:** 4
- **Query:** List the confirmed speakers for SaaStr.
- **Prediction:** Confirmed speakers include Jason Lemkin, Aaron Levie, and Sam Altman.
- **Reference:** Source: Agenda. Lemkin and Levie confirmed. Sam Altman is listed as "Invited/Tentative."

## The three checks

**Rule check.** Looks at: answer, reference. Passes when: Every certainty word in the answer (“confirmed”, “official”, “guaranteed”, “fully”, “all”, “always”, “definitely”) also appears in the reference. Fails when: The answer uses any of those words that the reference does not use.

**Safety rule.** Looks at: answer, reference. Passes when: When the reference marks a fact as partner-only, unconfirmed, beta, planned or not public, the answer states that status or declines to confirm the fact. Fails when: The answer states that fact as settled, or leaves out its status.

**Judge.** Looks at: question, answer, reference. Passes when: Every claim is at the same or lower certainty than the reference, keeps every qualifier that changes what a VP can do (beta, partner-only, regional, tentative, dated), and adds no detail the reference does not contain. Fails when: Any claim is more certain or broader than the reference, drops one of those qualifiers, or includes any detail that the reference does not contain.

## Results on the 20 Ascend IQ answers

| Check | Caught | Missed (rows) | False alarms (rows) | My P0 row |
|---|---|---|---|---|
| Rule check | 1 of 2 | 11 | 0 | Caught it |
| Safety rule | 2 of 2 | — | 0 | Caught it |
| Judge | 2 of 2 | — | 1, 3, 5, 6, 7, 8, 9, 12, 14, 16, 19 | Caught it |

**Judge on my P0 row:** Presents Sam Altman as a confirmed speaker, dropping his "Invited/Tentative" status from the reference.

## Where the failure was caught

Only the judge caught every overclaim row (3 of 3: rows 4, 11, 14). The safety rule caught 2 (rows 4, 11) and the rule check caught 1 (row 4). Ascend IQ overclaims by dropping qualifiers and widening scope, not by using certainty words, so word-based rules miss part of it. The judge’s other fails are real failures of other modes: it matched my M2 audit on all 20 rows.

## What I'd ship next

Ship the judge as the overclaim gate in staging, and keep the safety rule as a cheap check on every change for partner-only and tentative facts (it caught rows 4 and 11 with no judge cost). This is the highest-leverage change because the judge is the only layer that catches all 3 overclaim rows. It also catches invented details (rows 5, 9, 12, 19) that no rule can see. Next fix: make the judge return the failure mode, so frequency is counted by mode instead of by tag.

## Row by row

| Row | Rule | Safety | Judge | Judge reason |
|---|---|---|---|---|
| 1 | 1 | 1 | 0 | Cites outdated $49 pricing as current and fabricates an unsupported 10-seat minimum. |
| 2 | 1 | 1 | 1 | Accurately summarizes the three review complaints with no added details or certainty inflation. |
| 3 | 1 | 1 | 0 | Claims native SQL export is supported when the reference explicitly notes there is no native export in the UI. |
| 4 | 0 | 0 | 0 | Presents Sam Altman as a confirmed speaker, dropping his "Invited/Tentative" status from the reference. |
| 5 | 1 | 1 | 0 | Invents specific praise about UI and criticism of pricing not present in the reference. |
| 6 | 1 | 1 | 0 | Changes her previous title from Sr. Director to Director, which does not match the reference. |
| 7 | 1 | 1 | 0 | Makes unsupported qualitative claims about Competitor Z's throttling that contradict the reference's rate limits. |
| 8 | 1 | 1 | 0 | Claims compliance documents cannot be found despite the reference confirming a visible SOC2 Type II badge. |
| 9 | 1 | 1 | 0 | Adds "Series B," a detail that does not appear in the reference. |
| 10 | 1 | 1 | 1 | Accurately names Mark Johnson as CEO with no unsupported details or qualifiers dropped. |
| 11 | 1 | 0 | 0 | Claims a seamless integration while omitting the Zapier partnership qualifier that limits how it works. |
| 12 | 1 | 1 | 0 | Fabricates the hex code #007AFF and promotes Charcoal to a primary brand color. |
| 13 | 1 | 1 | 1 | Appropriately declines to summarize ongoing litigation per the reference's safety guidelines. |
| 14 | 1 | 1 | 0 | Presents Austin as an HQ location when the reference specifies it is an Engineering Hub. |
| 15 | 1 | 1 | 1 | Accurately reports the 275 headcount at equal or lower certainty than the reference. |
| 16 | 1 | 1 | 0 | Uses casual slang ("killer", "total game changer") that contradicts the brand voice reference. |
| 17 | 1 | 1 | 1 | Accurately states 24/7 support availability while keeping the Enterprise Plan only qualifier. |
| 18 | 1 | 1 | 1 | Appropriately declines to answer since no backend architecture is listed in the reference. |
| 19 | 1 | 1 | 0 | Narrows "Fixed CSV bug" to "the CSV export bug," adding a specific detail not in the reference. |
| 20 | 1 | 1 | 1 | Accurately states the private status and approximate ~$1B valuation from the reference. |
