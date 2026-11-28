# Module 5 · Eval Budget · Ascend IQ

**Quarterly budget cap:** $200,000
**Online eval spend:** $150,000 (2 of at most 3 online evals)

## Budget

| Failure | Trust metric | Risk | Level | Cost |
| --- | --- | :---: | :---: | ---: |
| Data Fabrication | Hallucination Rate | P0 | Online eval | $85,000 |
| Context Specificity | Robustness | P1 | Periodic audit | $7,000 |
| Overconfident Tone | UX Trust | P1 | Online eval | $65,000 |
| Data Bias | Fairness | P2 | Periodic audit | $5,500 |
| Slow Answers | Latency | P3 | Code-based check | $1,250 |

## Downgrades

### Periodic audit · Context Specificity (Robustness · P1)
- **Cheaper check:** Every answer restates the filters and constraints it applied (for example, "Negative reviews only, last 90 days"), so a dropped filter is visible to the VP. Every week, a reviewer scores 50 live answers drawn only from filtered, excluded, date-bound or multi-part questions, and checks each applied filter against the question. The same audit reruns on every prompt or retrieval change.
- **Why it’s enough:** With the filters restated in every answer, a dropped constraint is visible before the number reaches a deck, and the audit catches the pattern and turns repeat misses into golden-set cases. It misses individual misses between reviews, and it relies on the VP reading the restated filters.

### Periodic audit · Data Bias (Fairness · P2)
- **Cheaper check:** Every two weeks, a reviewer compares matched US and APAC answers to the same question types for depth and number of sources.
- **Why it’s enough:** Bias only shows when you compare many answers, which a per-answer judge can't do. It misses drift between audits, so we move it online as soon as any audit finds a depth or source gap, not when APAC volume grows, because low quality can be why APAC adoption stays low.

### Code-based check · Slow Answers (Latency · P3)
- **Cheaper check:** A timer on every request, with alerts if rolling P95 goes over 10 s (the M1 bar and the M4 Soft gate), on P99, and on P95 per top-50 account.
- **Why it’s enough:** It catches every slow answer at 5% of the online cost, including one account that is slow while the global P95 looks fine. It misses why an answer was slow, so we use traces for that.

## How this maps to my Module 2 to 4 work

- **Overconfident Tone is my Confident overclaims, a P0, not a P1.** The lab files it under UX Trust at P1. In my audit it states tentative or partner-only facts as confirmed (rows 4, 11, 14 of 20), so it puts a false fact in front of a VP. `03-eval-suites/eval-spec.md` sets the threshold at 0 overclaim fails, and `04-eval-gates/gate-map.md` makes it a Hard gate. That is why it gets one of the two online evals.
- **Data Fabrication covers my other factual P0s:** Contradicts the reference (3 of 20) and Stale data (1 of 20), plus Invented specificity (4 of 20, P2).
- **The only P0/P1 downgrade is Context Specificity.** Both funded failures put a false fact in front of a VP with no visible sign. A context miss becomes visible once every answer restates its filters, so a cheaper check is enough.

## Why not a third online eval

- Context Specificity online would bring online spend to $220,000, and Data Bias to $205,000. Both break the cap.
- Slow Answers would fit ($175,000), but a timer already catches every slow answer for $1,250. Paying $25,000 for a judge to do a timer's job buys nothing.

## Total and reserve

- Total at all levels: $163,750 (online $150,000, audits $12,500, code $1,250).
- $36,250 stays under the cap as reserve, to raise the overclaim judge's sampling rate during the staged rollout to 5 of the top 50 accounts (`04-eval-gates/launch-strategy.md`).

## What the CPO review changed

Pressure-tested by Gemini as the CPO (cross-company review; Claude helped draft this budget).

- **Changed, Context Specificity:** my first rationale ("the VP sees the miss right away") was wrong for silently dropped filters. A VP who asks for churn excluding one cohort gets a clean-looking number and puts it in a deck. Fix: every answer restates its applied filters, the audit samples only filtered or multi-part questions and checks each filter, and it reruns on every prompt or retrieval change.
- **Changed, Data Bias:** the trigger to go online was backwards. It now moves online on the first depth or source gap, not when APAC volume passes 20%.
- **Changed, Slow Answers:** added P99 and per-account P95 alerts, still code, so one slow top-50 account can't hide behind a healthy global P95.
- **Rejected:** a "$50,000 conditional online eval" for Context Specificity on "60 to 70% of traffic". Both figures are invented, the lab's levels have fixed costs, and it would leave no P0/P1 downgrade. Also rejected "your math is wrong": online spend is $150,000 of $200,000, and the reserve has a named use.
