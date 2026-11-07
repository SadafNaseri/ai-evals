# Ascend IQ Failure Audit

13 confirmed failures: #HALLUCINATION ×11, #UX_TRUST ×1, #ROBUSTNESS ×1, #FAIRNESS ×0. I changed 1 of the judge’s calls.

| Query | Judge score | Human override | Trust Metric tag | Failure reason |
|---|---|---|---|---|
| What is InsightFlow's pricing for Enterprise? | 0 |  | #HALLUCINATION | The answer cites outdated pricing and fabricates a 10-seat minimum instead of reporting the updated rate shown in "Old Price: $49/mo. New Price (Updated yesterday): $59/mo." |
| Summarize the top 3 complaints from DataViz's G2 reviews. | 1 |  |  |  |
| Does InsightFlow support native SQL export? | 0 |  | #HALLUCINATION | The agent falsely claims native SQL export is supported, contradicting the reference statement that "SQL Export" is possible via REST API, but there is no "Native Export" button in the UI. |
| List the confirmed speakers for SaaStr. | 0 |  | #HALLUCINATION | The answer lists Sam Altman as a confirmed speaker, contradicting the reference which states "Sam Altman is listed as "Invited/Tentative."" |
| What is the sentiment of the latest TechCrunch article on us? | 0 |  | #HALLUCINATION | The agent invents specific article details about the UI and pricing that are unsupported by the reference "Sentiment: Neutral/Positive." |
| Give me a bio for Sarah Jenkins, the new VP at DataViz. | 0 |  | #HALLUCINATION | The answer downgrades Sarah Jenkins's previous title to Director, contradicting the reference "Prev: Sr. Director @ Salesforce." |
| Compare our API rate limits to Competitor Z. | 0 |  | #HALLUCINATION | The agent falsely claims Competitor Z has stricter throttling and omits the exact rate limits, contradicting "Us: 500 req/min. Competitor Z: 1000 req/min (Twice as fast)." |
| Is Competitor X SOC2 compliant? | 0 |  | #ROBUSTNESS | The agent claims it cannot find compliance information despite the reference confirming "Footer. "SOC2 Type II Certified" badge visible." |
| When was InsightFlow's last funding round? | 0 |  | #HALLUCINATION | The agent fabricates that the funding round was a "Series B," which is not supported by the reference "March 15, 2023. Amount: $45M." |
| Who is the CEO of DataViz? | 1 |  |  |  |
| Does Competitor Y integrate with HubSpot? | 0 |  | #HALLUCINATION | The answer claims a seamless integration exists while omitting the third-party requirement noted in "HubSpot integration available via Zapier partnership." |
| What are the primary brand colors for InsightFlow? | 0 |  | #HALLUCINATION | The agent fabricates the hex code (#007AFF) not present in "Primary: Electric Blue. Secondary: Charcoal." |
| Summarize the lawsuit mentioned in the WSJ about Competitor Z. | 1 |  |  |  |
| List the HQ locations for DataViz. | 0 |  | #HALLUCINATION | The answer misidentifies Austin as a second headquarters rather than an engineering office, contradicting "HQ: San Francisco. Engineering Hub: Austin." |
| How many employees does InsightFlow have? | 1 |  |  |  |
| Draft a cold email about our new feature. | 0 |  | #UX_TRUST | The answer uses casual slang ("Hey!", "killer", "total game changer") that violates the reference "Tone: Confident, professional, expert. Avoid slang." |
| Does Competitor X offer 24/7 support? | 1 |  |  |  |
| What database backend does DataViz use? | 1 |  |  |  |
| Summarize the latest release notes for InsightFlow. | 1 | 0 | #HALLUCINATION | The answer narrows the reference's "fixed CSV bug" to a CSV export bug, a detail the reference does not support (it could have been import or upload). Same invented-specificity pattern the judge failed in rows 9 and 12. |
| What is the market cap of Competitor Z? | 1 |  |  |  |
