# AI Evaluation Strategy Canvas

> Repo file `ai-evals/01-evaluation-strategy/strategy-canvas.md` (the repo is your submission).

## 1. Product Strategy, The Context

**Who it's for:** VP-level strategists and product leaders at Fortune 500 companies who pay upward of $50,000 a year for verified market intelligence.

**What they use it for:** Asking plain-language questions, like “Compare the pricing models of Stripe and Adyen,” and getting answers in seconds instead of digging through the platform.

**What's at stake:** Engagement is dropping, Q4 retention is at risk, and the CPO wants Ascend IQ live for the top 50 enterprise accounts next month. One poor answer to the wrong client can do lasting damage to the relationship.

## 2. Measurements, The Execution

**User promise.** For VP-level strategists at our top enterprise accounts, Ascend IQ promises to answer plain-language market questions in under 10 seconds (P95), with every figure traceable to Ascend’s verified data, or to say clearly when it doesn’t know, so that clients keep making decisions on Ascend data and renew.

**Top 3 trust metrics:**
- **Latency**, Response speed (P95 / P99). Slow kills engagement.
- **Hallucination Rate**, % of outputs that are confidently false or fabricated.
- **Robustness**, Coherence on messy, adversarial, out-of-scope inputs.

**Why these three:** Latency (P95): protects “under 10 seconds.” If an answer is slower than searching the platform by hand, clients go back to doing it by hand.
Hallucination rate: protects “verified.” One invented figure in a board deck breaks the reason clients pay.
Robustness: protects “plain-language.” Executives ask vague, multi-part or out-of-scope questions (like a market Ascend doesn’t cover), and the agent should decline, not guess.


## 3. Strategic Trade-Offs, The Cost

### Trade-off 1 · Hallucination Rate ↔ Latency
We prioritize hallucination rate over latency because a slower verified answer costs a client a few seconds, while a fast wrong answer costs us the account.

### Trade-off 2 · Latency ↔ Robustness
We prioritize robustness over latency because checking whether a question is in scope, and breaking multi-part questions into steps, adds seconds. A strategist will wait a moment for “Ascend doesn’t cover that market” but won’t forgive a confident guess.

---
_Generated from the AI Evaluation Strategy Canvas, M1 lab tool, AI Evals Certification._