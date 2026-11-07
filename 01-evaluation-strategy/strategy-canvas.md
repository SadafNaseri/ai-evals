# AI Evaluation Strategy Canvas

> Repo file `ai-evals/01-evaluation-strategy/strategy-canvas.md` (the repo is your submission).

## 1. Product Strategy, The Context

**Who it's for:** VP-level strategists and product leaders at Fortune 500 companies who pay upward of $50,000 a year for verified market intelligence.

**What they use it for:** Asking plain-language questions, like “Compare the pricing models of Stripe and Adyen,” and getting answers in seconds instead of digging through the platform.

**What's at stake:** Engagement is dropping, Q4 retention is at risk, and the CPO wants Ascend IQ live for the top 50 enterprise accounts next month. One poor answer to the wrong client can do lasting damage to the relationship.

## 2. Measurements, The Execution

**User promise.** For VP-level strategists at our top enterprise accounts, Ascend IQ promises to answer plain-language market questions in under 10 seconds (P95), with every figure traceable to Ascend’s verified data, or to say clearly when it doesn’t know, so strategists can build board-ready comparisons in minutes instead of hours, and keep making decisions on Ascend data and renew.

**Top 3 trust metrics:**
- **Latency**, Response speed (P95 / P99). Slow kills engagement.
- **Hallucination Rate**, % of outputs that are confidently false or fabricated.
- **Robustness**, Coherence on messy, adversarial, out-of-scope inputs.

**Why these three:** Latency (P95 ≤ 10s, question to complete answer with sources): protects “under 10 seconds.” If an answer is slower than searching the platform by hand, clients go back to doing it by hand.
Hallucination Rate (0 invented figures in the test set): protects “verified.” One invented figure in a board deck breaks the reason clients pay.
Robustness (≥ 95% clean declines): protects “plain-language.” Executives ask hurried, vague or multi-part questions. The agent should answer the part Ascend covers and decline the part it doesn’t, not guess.


## 3. Strategic Trade-Offs, The Cost

### Trade-off 1 · Hallucination Rate ↔ Latency
We prioritize hallucination rate over latency because a slower verified answer costs a client a few seconds, while a fast wrong answer costs us the account.

### Trade-off 2 · Latency ↔ Robustness
We prioritize robustness over latency because checking whether a question is in scope, and breaking multi-part questions into steps, adds seconds. A strategist will wait a moment for “Ascend doesn’t cover that market” but won’t forgive a confident guess.

---
_Generated from the AI Evaluation Strategy Canvas, M1 lab tool, AI Evals Certification._