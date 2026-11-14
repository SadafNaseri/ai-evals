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

Scored against the 3 overclaim rows (4, 11, 14). The Lab Guide's own export scored against 2 rows (4, 11); this table is corrected by hand.

| Check | Caught | Missed (rows) | False alarms (rows) | Other real failures caught (rows) | My P0 row |
|---|---|---|---|---|---|
| Rule check | 1 of 3 | 11, 14 | none | none | Caught it |
| Safety rule | 2 of 3 | 14 | none | none | Caught it |
| Judge | 3 of 3 | none | none | 1, 3, 5, 6, 7, 8, 9, 12, 16, 19 (10) | Caught it |

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

## Change from Module 2

In Module 2 I listed rows 3, 4, 7, 11 and 14 as overclaims. Rows 3 and 7 state something the reference contradicts; they do not drop a qualifier or widen scope. They are now counted as "contradicts the reference", and my P0 row moves from 7 to 4 (a tentative speaker presented as confirmed).

Priority stays P0. Counted by mode, overclaims are 3 of 20 (15%), not 11 of 20. Against a promise of zero unverified claims, any rate above zero in answers to the top 50 accounts counts as high frequency.

## Judge fails by failure mode (not by tag)

Classified by me from the judge's reasons. The judge was asked to start each reason with a mode label but did not, so this sorting is manual.

| Failure mode | Rows | Count | Root cause |
|---|---|---|---|
| Confident overclaims (dropped qualifier or widened scope) | 4, 11, 14 | 3 | Generation |
| Contradicts the reference | 3, 6, 7 | 3 | Generation |
| Invented specificity | 5, 9, 12, 19 | 4 | Generation |
| Stale data | 1 | 1 | Retrieval |
| Over-refusal | 8 | 1 | Prompt (decline rule too strict) |
| Off-brand tone | 16 | 1 | Prompt |
| **Total** | | **13** | |

## Evaluators for the other P0 and P1 risks

Status: the judge has been run on all 20 rows. The code rules below are designed but not yet run; their "must fail" and "must still pass" rows are the test they have to pass before they become gates.

| Risk | Priority | Evaluator | Type | Must fail | Must still pass | Lab 1 evidence | Status |
|---|---|---|---|---|---|---|---|
| Contradicts the reference | P0 | Rule: every figure in the answer matches the figure in the reference (same rule as stale data). Plus the judge for non-numeric contradictions (rows 3, 6). | Code (partial) + judge + human | 3, 6, 7 | 10, 15, 20 | Judge failed 3, 6, 7 | Judge run; rule not yet run |
| Stale data | P0 | Rule: every figure in the answer matches the current value in the reference, and the source date is inside the freshness window; any mismatch or expired source fails. Plus the judge. | Code + judge | 1 | 15, 20 | Judge failed row 1 | Judge run; rule not yet run |
| Over-refusal | P1 | Rule: when the reference contains an answer and is not marked legal-restricted or "nothing public", the answer must not decline ("cannot find", "unable to", "not available"). Plus the judge. | Code + judge | 8 | 13, 18 (correct declines) | Judge failed row 8 and passed 13 and 18 | Judge run; rule not yet run |

Contradicts the reference is rated P0 for the same reason as overclaims: it puts a wrong fact in front of a VP, at 3 of 20.

Over-refusal is tracked as a false-refusal rate next to the clean-decline rate. Clean declines alone reward refusing everything.

## Coverage matrix

Strong means a code gate or two probabilistic checks. Strong is required for every P0 and P1.

| Failure | Priority | Code | LLM judge | Human | Rating |
|---|---|---|---|---|---|
| Confident overclaims | P0 | Safety rule (partial) | Yes | PdM reviews every judge fail on P0 rows before release | Strong (judge + human) |
| Contradicts the reference | P0 | Figure-match rule (numeric only) | Yes | PdM reviews every judge fail on P0 rows before release | Strong (judge + human) |
| Stale data | P0 | Figure and freshness rule | Yes | | Strong |
| Over-refusal | P1 | Decline-phrase rule | Yes | | Strong |
| Invented specificity | P2 | | Yes | | Medium |
| Off-brand tone | P3 | | Yes | | Medium |
| Fairness (persona, region, company size) | Not rated | | | | Weak: no golden-set row tests it |

Gap: the golden set has no #FAIRNESS rows. Next step: add the same questions asked as different personas, regions and company sizes, and compare pass rates.

## Judge calibration

| Measure | Result |
|---|---|
| Judge (Gemini) vs my Module 2 labels, pass/fail | 20 of 20 agree (Module 2 judge: 19 of 20) |
| Row 19 (unsupported detail) | Now fails, as it should. The "any detail not in the reference fails" rule fixed it. |
| Format compliance (mode label at start of reason) | 0 of 20. The judge ignored the instruction. |

Limits: my labels were made after I saw the Module 2 judge's calls, so they are not fully blind. 20 rows is a small set. Next: label 12 new answers blind before the judge runs, rerun the judge in a new chat to check that its calls are stable, and require structured output (pass/fail, failure mode, reason in fixed fields) so failure modes are counted automatically, not sorted by hand.

## How to run this suite

- Rule check and safety rule: script-runnable. Each compares fixed word lists in the answer against the reference and returns 1 or 0 per row.
- Judge: paste the judge prompt and the 20-row golden set (`02-failure-discovery/audit-log.md`) into an LLM from a different company than the model that wrote the answers. It returns 1 or 0 and one line of reasoning per row.
- Rerun the whole suite on every prompt, retrieval or model change, and compare against this baseline: 13 fails, 7 passes.

## Trajectory scorecard

Pending. The golden set holds question, answer and reference only, with no tool calls. To score: compare each recorded run with the reference path on the six dimensions (tool selection, argument correctness, redundant or looping steps, recovery, plan coherence, task completion).
