# Module 5 · Coverage Matrix · AI Report Summaries

## Coverage Row

| Product | Hallucination Rate | Fairness | Latency | UX Trust | Robustness |
|---|---|---|---|---|---|
| AI Report Summaries | ✅ Covered | ❌ Gap | ⚠️ Partial | ❌ Gap | ❌ Gap |

## What Runs Today

- An LLM judge compares each summary with its source report before every release.
- Response time is logged, but nobody has set a target.

## Accepted Gap

**Robustness.** We accept this gap because AI Report Summaries only takes in Ascend's own research reports, a format our team controls, not open client questions. The release judge already scores every report type we ship today, so a separate robustness suite would cost more than the risk it removes right now.

We’ll look again: If a new report format or data source launches, or if one client flags a summary that missed the report's main point. Otherwise, at the next quarterly eval budget review.

## Critical Gap

We've flagged UX Trust as the most critical gap for AI Report Summaries because summaries go straight to client VPs with nobody reviewing them first. One casual or alarmist line reaches a $50K+ account under Ascend's name, the client's account lead hears about it first, and that renewal is at risk. To close it, we'll add a tone eval gate before every release. It reuses the Competitor Signal Tracker's existing style check (it flags casual or alarmist tone), adds a tone line to the release judge that already runs, and scores every summary in the release test set against the brand-voice guide. Any fail blocks the release.

## After the Pressure Test

Gemini (cross-company review) agreed with the marks and the UX Trust plan. Two changes. (1) Fairness: UX Trust stays the critical gap, but Fairness is no longer left without an owner. A quarterly periodic audit will compare a sample of summaries across regions and client industries. That answers the manager's question: we don't yet know whether summaries are worse for some regions or industries, and the audit is how we find out. (2) Robustness: still the accepted gap, but the 5 longest and most table-heavy reports join the release test set, so the existing judge sees edge cases at no new cost. Not changed: the tone gate blocks release here, while off-brand tone is only Advisory for Ascend IQ, because no person reads these summaries before the client does.

## Ascend IQ (not launched) · coverage today vs gates designed in Module 4

| Product | Hallucination Rate | Fairness | Latency | UX Trust | Robustness |
|---|---|---|---|---|---|
| Ascend IQ (External) | ⚠️ Partial: Hard gates designed (Staging judge, PR figure-match and safety rules); today the judge alone has run | ❌ Gap: no golden-set row tests persona, region or company size | ❌ Gap: Soft gate P95 <= 10 s designed, not measured yet | ⚠️ Partial: Advisory slang word list at PR (word list only) | ⚠️ Partial: over-refusal judge caught row 8; decline-phrase rule not yet run |

Same lesson as the row above: Ascend IQ's checks are designed but mostly not running. No P0 or P1 has Strong coverage today, which is a release blocker in `04-eval-gates/launch-strategy.md`.
