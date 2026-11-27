# Module 4 · Launch Strategy · Ascend IQ

## Launch call today

Delay launch. The current beta fails every Hard gate it has been tested on: Confident overclaims 3 of 20, Contradicts the reference 3 of 20, Stale data 1 of 20, and Unverified source in a client answer on the 1 recorded run. A failed Hard gate has no override, so the staged rollout below applies only once every Hard gate passes and the Soft gate is the only one failing.

## Release criteria

Every row is tested on the Ascend IQ golden set (20 cases today; 32 once the Module 3 Post-Class Practice answers are added).

| Severity | Failure | Metric | Threshold | Method | Runs at |
|---|---|---|---|---|---|
| Hard | Confident overclaims (P0, from my eval spec) | Share of golden-set answers with a claim more certain or broader than the reference, a dropped qualifier, or a detail not in the reference | 0 overclaim fails, and the safety rule passes 100% | LLM judge from a different model company than Ascend IQ's (the eval-spec evaluator); safety rule as code at Pull Request; PdM reviews every P0 judge fail | Staging (judge, blocks release); safety rule blocks the merge at Pull Request; rerun on the release build as a kill switch |
| Soft | Latency (gate map row 3) | P95 time from question to complete answer with sources | P95 <= 10 s (my Module 1 bar, set before reading the logs) | Code-based check (timer) | Staging |
| Advisory | Off-brand tone (gate map row 17, P3) | Share of answers that use a word from the brand-voice slang list ("killer", "game changer" and similar) | 0 slang hits; a hit posts a warning only | Code-based check (word list) | Pull Request |

## Request for engineering

- **Hard gate · Confident overclaims.** Check: the LLM judge, run on the Ascend IQ golden set at Staging, plus the safety rule at Pull Request. Measures: answers with a claim more certain or broader than the reference, a dropped qualifier, or a detail not in the reference. Passes when: 0 overclaim fails and a 100% safety-rule pass. On a fail: block the merge (safety rule) or the release (judge). No override.
- **Soft gate · Latency.** Check: a timer, run on the Ascend IQ golden set at Staging. Measures: P95 from question to complete answer with sources. Passes when: P95 <= 10 s. On a fail: pause the release until the PdM signs off on an override.
- **Advisory gate · Off-brand tone.** Check: a word-list rule, run on the Ascend IQ golden set at Pull Request. Measures: slang from the brand-voice list. Passes when: 0 hits. On a fail: post a warning, and don't block.

## If our Soft gate fails

If our Soft gate fails (P95 above 10 s on the golden set), I recommend a staged rollout because it limits slow answers to 5 of the top 50 accounts, with their account teams briefed, while we measure P95 on live traffic. If live P95 stays above 10 s or those accounts use Ascend IQ less than the platform search, we roll back.

Why not the other levers: a beta label sets the wrong expectation for clients paying $50K+ for verified intelligence, and a feature flag needs a live latency eval we have not built yet.

## Other gates and conditions that block release

These come from my Module 3 suite and the gate map's "Beyond the six rows" section.

| Gate or condition | Threshold | Level | Status |
|---|---|---|---|
| Contradicts the reference (P0) | 0 judge fails; figure-match rule passes 100% at Pull Request | Hard | Judge run (3 of 20 fail); rule not yet run |
| Stale data (P0) | Every figure matches the current reference, source inside the freshness window | Hard | Judge run (1 of 20 fail); rule not yet run |
| Unverified source in a client answer (P0) | 0 source-provenance fails | Hard | Can't gate yet: needs recorded runs for all golden-set questions (1 today) |
| Over-refusal, false-refusal ceiling (P1) | 0 false refusals on golden-set rows whose reference holds the answer, next to >= 95% clean declines; any regression on a must-still-pass row blocks the merge | Hard at Pull Request, Soft at Staging | Judge run (1 of 20 fail, row 8); rule not yet run |
| Coverage | Every P0 and P1 at Strong (a code gate or two probabilistic checks) | Release blocker | Not met: every P0 and P1 is Medium today |
| Golden set size | At least 30 cases across the four case types | Release blocker | 20 today |
| Latency | P95 measured on the golden set | Release blocker | Not measured yet |
| Post-fix regression | After the overclaim fix, rerun the full suite; over-refusals and P95 stay inside their thresholds | Release blocker | Not run yet |

## What the team checks before launch

1. Every Hard gate passes on a golden set of at least 30 cases.
2. Every P0 and P1 is at Strong coverage.
3. P95 is measured. If it is over 10 s, the staged rollout above applies.
4. The post-fix regression rerun is clean.
