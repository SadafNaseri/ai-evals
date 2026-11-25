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

Strong means a code gate or two probabilistic checks. Strong is required for every P0 and P1. Two ratings per row: "Designed" is the coverage once every listed check runs; "Today" counts only checks that have actually run on the golden set (the judge on all 20 rows, the rule check and safety rule in Lab 1). Human review is counted as a layer, not as a second probabilistic check.

| Failure | Priority | Code | LLM judge | Human | Rating |
|---|---|---|---|---|---|
| Confident overclaims | P0 | Safety rule (run; caught 2 of 3) | Yes (run; 3 of 3) | PdM reviews every judge fail on P0 rows before release | Designed: Strong. Today: Medium (one judge plus a partial code rule) |
| Contradicts the reference | P0 | Figure-match rule (numeric only; not yet run) | Yes (run; 3 of 3) | PdM reviews every judge fail on P0 rows before release | Designed: Strong only for numeric facts. Today: Medium |
| Stale data | P0 | Figure and freshness rule (not yet run) | Yes (run; caught row 1) | | Designed: Strong. Today: Medium |
| Over-refusal | P1 | Decline-phrase rule (not yet run) | Yes (run; caught row 8) | | Designed: Strong. Today: Medium |
| Invented specificity | P2 | | Yes | | Medium |
| Off-brand tone | P3 | | Yes | | Medium |
| Unverified source in a client answer (trajectory) | P0 | Source-provenance rule (not yet run) | Trajectory judge (run on 1 recorded run) | PdM reviews trajectory fails | Designed: Strong. Today: Medium |
| Fairness (persona, region, company size) | Not rated | | | | Weak: no golden-set row tests it |

Today no P0 or P1 meets Strong: every one rests on the judge alone, plus a partial safety rule for overclaims. This is a finding, not an omission: Ascend IQ cannot pass a release gate until every P0 and P1 is Strong, so this gap is carried into the Module 4 gate map as a release blocker. Before release, the code rules must run on the 20 rows and pass their "must fail" and "must still pass" tests. For non-numeric overclaims and contradictions, where no code rule fits, a second judge from another model company is the second probabilistic check.

Gap: the golden set has no #FAIRNESS rows. Next step: add the same questions asked as different personas, regions and company sizes, and compare pass rates.

## Judge calibration

Two checks, both against my own calls.

**1. My Module 3 judge (my rubric) vs my Module 2 labels (20 golden-set rows)**

| Measure | Result |
|---|---|
| Pass/fail agreement | 20 of 20 (Module 2 judge: 19 of 20) |
| Row 19 (unsupported detail) | Now fails, as it should. The "any detail not in the reference fails" rule fixed it. |
| Format compliance (mode label at start of reason) | 0 of 20. The judge ignored the instruction. |

Limit: my Module 2 labels were made after I saw the Module 2 judge's calls, so they are not fully blind.

**2. Blind check: the Post-Class Practice judge (course rubric, not my Module 3 judge) vs my labels on 12 new Ascend IQ answers**

Practice judge rubric: "the answer is grounded in the source, and it directly answers the question." I labeled all 12 before seeing the judge's calls.

| Measure | Result |
|---|---|
| Blind agreement | 10 of 12 (disagreed on answers 03 and 10) |
| After my review | 9 of 12. I changed answer 05 to fail (it adds Pipedrive, not in the source). I kept answer 08 as fail: its source is an open news search, not Ascend's verified data. |
| Pattern in all 3 disagreements | The judge passes answers that are partly grounded but add details the source does not contain: 03 (every plan, AI forecasting, $20 price, analyst claim), 05 (Pipedrive), 10 (invented quote and reviewer, wrong top complaint). |

Rewritten rubric: Pass only if every claim in the answer appears in the source, nothing contradicts it, and it directly answers the question. Fail if the answer adds any detail, quote, name, number or scope the source does not contain, or changes a fact. Fail if the answer relies on a source that is not Ascend's verified data, such as an open news or web search. If the verified source contains the answer, a refusal fails.

What this means: a "grounded" rubric without an every-claim rule passes invented specificity, the same failure the Module 2 judge missed on row 19. My Module 3 judge already has the every-claim rule, but it has not yet been tested blind on these 12 answers. Next: run my rewritten rubric blind on the same 12 answers to confirm it fixes the 3 disagreements; require structured output (pass/fail, failure mode, reason in fixed fields) so failure modes are counted automatically; and rerun the trajectory judge twice with the reference path frozen to test stability.

## How to run this suite

- Rule check and safety rule: script-runnable. Each compares fixed word lists in the answer against the reference and returns 1 or 0 per row.
- Judge: paste the judge prompt and the 20-row golden set (`02-failure-discovery/audit-log.md`) into an LLM from a different company than the model that wrote the answers. It returns 1 or 0 and one line of reasoning per row.
- Rerun the whole suite on every prompt, retrieval or model change, and compare against this baseline: 13 fails, 7 passes.

## Trajectory scorecard

Task: a client asked Ascend IQ, "Compare the pricing models of Stripe and Adyen." The final answer reads well. Source: M3 Post-Class Practice recorded run.

**Reference path** (the course draft was missing a step; I added `compare`):
1. get_pricing(company="Stripe")
2. get_pricing(company="Adyen")
3. compare(a, b) (added by me: the task is a comparison, and the tool exists)
4. write_answer(text, sources)

**Recorded run:**
1. get_pricing(company="Stripe"): verified Stripe pricing.
2. get_pricing(company="Ayden"): error, no company named "Ayden".
3. web_search(query="Adyen pricing"): an unverified blog post. Ascend IQ is not allowed to use web results in client answers.
4. write_answer(sources=["Stripe pricing page", "paymentsblog.example/adyen-fees"]): sent to the client.

**Scores** (judge: Gemini; Ascend IQ is not a Gemini model; reviewed by me)

| Dimension | Judge | My call | Step that decided it |
|---|---|---|---|
| Tool selection | Fail | Agree | Step 3: used the forbidden web_search and skipped compare |
| Argument correctness | Fail | Agree | Step 2: "Ayden" typo, the root of every later step |
| Redundant or looping | Pass | Agree | Four distinct calls, no repeats |
| Recovery | Fail | Agree | Step 3: switched to a banned tool instead of retrying get_pricing("Adyen") |
| Plan coherence | Partial | Agree | Retrieve-then-answer shape is right, but it skipped compare and broke the sourcing rule |
| Task completion | Partial | **Fail** | Step 4: the client received an answer built on a banned, unverified source. The Adyen half can't be called accurate, and delivering it breaks Ascend's verified-data promise. |

Result: 1 Pass, 1 Partial, 4 Fail (my calls). Human-judge agreement: 5 of 6.

**Worst dimension for a client: Recovery.** The typo alone would have caused a retry or a decline. The recovery is what put an unverified blog into an answer presented as verified intelligence. An output-only eval would pass this answer: it reads well and its Stripe figures are right.

**Check added to the suite (source provenance):** code check on every run. Fail if write_answer cites any source that was not returned by get_pricing or search_intel, such as a web_search result or an outside URL. Paired rule: after a tool error, the agent must retry with a corrected argument or tell the client that part is not covered; any web_search before write_answer fails the run. This run fails both.

| Failure | Priority | Code | LLM judge | Human | Rating |
|---|---|---|---|---|---|
| Unverified source in a client answer (trajectory) | P0 | Source-provenance rule | Trajectory judge vs reference path | PdM reviews trajectory fails | Designed: Strong. Today: Medium |

Priority P0: it breaks the core promise (every figure traceable to Ascend's verified data) while the answer looks correct, so users cannot catch it.

Judge stability note: Gemini scored the run twice, before and after I added compare to the reference path. Two scores changed (Argument correctness Partial to Fail, Task completion Fail to Partial). The reference path changed between runs, so this is not a clean stability test, but trajectory judge scores need human review before they gate a release.

Limit: one recorded run. The golden set needs recorded runs for each of its 20 questions before trajectory can be a release gate.
