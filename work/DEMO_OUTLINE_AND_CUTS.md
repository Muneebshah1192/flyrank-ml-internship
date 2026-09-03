# 5-Minute Demo Outline & Shareable Cuts
**Author:** Syed Muneeb Haider Shah  
**Track:** Machine Learning (Assignment ML-12)  
**Project:** Refresh Opportunity Scoring for Search Content

---

## 1. 5-Minute Demo Showcase Outline

* **Minute 0:00–1:00 | The Question:** How can FlyRank content and SEO teams reliably triage which decaying search content to refresh first when editorial audit capacity is limited to 50 items a week?
* **Minute 1:00–2:00 | The Method:** Analyzed 104,301 verified client-content pairs from 33.8M warehouse rows (April–June 2026), evaluated with a strict client-grouped holdout split (35 training clients, 9 test clients, zero domain overlap) to eliminate data leakage.
* **Minute 2:00–3:00 | One Chart:** Top-50 prioritization benchmark showing the Heuristic Rule Baseline achieving Precision@50 of 0.80 (40/50 true declining items), outperforming Random Forest at 0.68 (34/50) and Logistic Regression at 0.56 (28/50).
* **Minute 3:00–4:00 | One Honest Result:** Complex machine learning models overfit client-specific metadata and secondary content signals, failing to beat the domain-aligned 3-signal rule baseline on unseen client domains.
* **Minute 4:00–5:00 | One Recommendation:** Deploy the explainable 3-signal composite rule (40% impression volume, 35% CTR opportunity, 25% freshness) directly into weekly triage queues, reserving ML models for non-blocking secondary diagnostics.

---

## 2. Shareable Cuts

### Cut 1: Short Social Post (Methodology Cut)
Does complex machine learning always beat an engineered heuristic rule? Not in our latest search intelligence evaluation across 33.8 million Google Search Console records.

While building a content decay refresh triage pipeline during my FlyRank ML internship, I evaluated Logistic Regression, Random Forest, and a multi-signal heuristic baseline using a client-grouped holdout split (zero domain cross-contamination).

The result: the transparent rule baseline won with a Precision@50 of 0.80 (40/50 decaying items identified), outperforming Random Forest (0.68). In production decision-support systems, domain alignment and top-of-queue precision matter far more than algorithmic complexity.

### Cut 2: 3-Sentence Employer-Facing Summary
* **What I Built:** A leakage-aware content decay prioritization pipeline evaluating statistical learning models against transparent heuristic scoring for SEO refresh triage queues.
* **On What Data:** Evaluated on 104,301 verified client-content pairs spanning 33.8 million search performance rows from the FlyRank Search Intelligence warehouse.
* **What It Showed:** An interpretable composite rule baseline outperformed Random Forest by 17.6% on Precision@50 (0.80 vs. 0.68) on unseen client partitions, establishing why production decision systems prioritize domain-aligned heuristics over uncalibrated complexity.
