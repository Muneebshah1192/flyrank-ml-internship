# Refresh Opportunity Scoring for Search Content

## FlyRank Machine Learning Capstone

This repository contains the final Machine Learning capstone for the FlyRank internship.

The project studies a practical search-intelligence question:

> Which published content items should be prioritized for refresh review based on observed search-performance signals and content characteristics?

The goal is **decision support**, not automatic content rewriting and not prediction of Google's internal ranking algorithm.

---

## 1. Project outcome

The project compares three approaches for ranking content for review:

1. A transparent rule-based baseline
2. Logistic Regression
3. Random Forest

All approaches are evaluated using the same client-grouped holdout and **Precision@50**, which measures how many of the first 50 recommendations are observed as declining.

### Main result

| Approach | Precision@50 |
|---|---:|
| Rule baseline | **0.80** |
| Random Forest | 0.68 |
| Logistic Regression | 0.56 |

The rule-based baseline produced the strongest measured top-50 prioritization in this experiment.

This is reported as an observed result for the selected data window, target definition, features, model configurations, and validation split. It is not presented as evidence that simple rules will always outperform machine learning.

---

## 2. Research question

**Which published content items should be prioritized for refresh review based on observed search-performance signals and content characteristics?**

### Decision supported

The output helps an editorial or SEO team decide which pages to investigate first when review capacity is limited.

A high-ranked item means:

> **Review this item earlier.**

It does not mean:

> **Rewrite this item automatically.**

Human review remains part of the workflow.

---

## 3. Data

The analysis uses the **FlyRank internship warehouse release v20260703**.

### Main warehouse sources

- `fact_content_daily_performance`
- `dim_content`

### Analysis window

**April 1, 2026 → June 30, 2026**

April is used as the historical reference period. June provides the later observed outcome.

### Observed population

- 33,806,178 daily rows were observed across the April–June window.
- 66 clients were represented.
- 409,326 content items were represented.
- 158,700 client-content pairs had observations in both April and June.
- 104,301 records formed the final modeling population.

### Modeling population rules

The modeling population was restricted to:

- published content
- non-deleted content
- at least 100 April impressions
- observations available in both comparison periods

The minimum April-impression threshold reduces unstable percentage changes caused by very small denominators.

### Data grain

The daily performance table was checked at:

`report_date × client_hash_id × content_hash_id`

The June development sample contained 11,694,072 rows and 11,687,682 distinct daily records. The duplicate rows identified during the grain check were exact duplicates and were not treated as independent performance observations.

---

## 4. Methodology

### Outcome definition

A content item is considered **observably declining** when June impressions are at least 20% lower than April impressions.

Among the 104,301 modeling records:

- 77,431 were labeled declining
- 26,870 were not declining

The target is an **observed proxy outcome**. It is not Google's internal ranking score.

### Historical features

The model feature set uses information available before the later outcome period:

- April impressions
- April clicks
- April CTR
- April average position
- search volume
- competition
- backlinks
- character count
- word count
- content age
- days since update
- selected missingness indicators

### Leakage controls

The following were excluded from model inputs:

- June impressions
- June clicks
- June CTR
- June average position
- April-to-June impression change
- decline label
- client/content identifiers as predictive features

The purpose is to keep later outcome information out of the historical feature set.

### Validation design

A client-grouped holdout was used.

- Training: 99,796 records across 35 clients
- Holdout: 4,505 records across 9 clients
- Client overlap: 0

This avoids placing content from the same client into both training and holdout.

### Baseline

The baseline is a transparent scoring rule combining:

- 40% historical impression opportunity
- 35% low-CTR opportunity
- 25% freshness opportunity

### ML models

Two supervised models were tested:

- Logistic Regression
- Random Forest

---

## 5. Results

### Precision@50

| Approach | Precision@50 |
|---|---:|
| Rule baseline | **0.80** |
| Random Forest | 0.68 |
| Logistic Regression | 0.56 |

On the holdout:

- the baseline had 40 declining items in its top 50
- Random Forest had 34
- Logistic Regression had 28

### Interpretation

The transparent baseline performed best in the tested experiment.

The finding supports using a simple, explainable prioritization rule for this specific capstone setup rather than adding ML complexity without measured benefit.

It does **not** establish that:

- rules always beat ML
- ML is unsuitable for search intelligence
- the same result will hold on future data

---

## 6. Ranked recommendations

The final recommendation queue is based on the best-performing baseline.

Typical reason codes include:

- `STRONG_DECLINE`
- `LOW_CTR_OPPORTUNITY`
- `OLDER_CONTENT`
- `MIXED_SIGNAL`

### Recommended human workflow

1. Review high-priority items first.
2. Confirm that the observed decline is meaningful.
3. Check search demand and strategic relevance.
4. Inspect SERP/intent context before changing metadata or content.
5. Decide between refresh, monitoring, consolidation, or no action.

The ranking is a triage aid, not an automatic rewrite engine.

---

## 7. Repository artifacts

### Notebook

`work/notebooks/capstone.ipynb`

This contains the capstone analysis and supporting code.

### Model comparison

`work/outputs/capstone_model_comparison.csv`

Contains the measured model-versus-baseline comparison.

### Ranked recommendations

`work/outputs/capstone_ranked_recommendations.csv`

Contains the public-safe ranked recommendation output.

### Research paper

`docs/index.html`

This is the deployed research paper used for the GitHub Pages site.

### Submission URL

`submission/paper_url.txt`

This file must contain exactly **one line**: the direct URL of the deployed research paper.

---

## 8. Public-safe and ethical framing

This project intentionally avoids publishing:

- client names
- raw domains
- private URLs
- private queries
- credentials
- raw private exports

Identifiers are used only for joins, grouping, and validation.

The research does not claim to prove Google's ranking algorithm, explain causal ranking changes, or guarantee that refreshing a page will restore traffic.

The appropriate language for the findings is:

**observed · measured · directional · decision-support**

---

## 9. Limitations

The main limitations are:

- The decline label is a proxy based on observed impression movement.
- The study covers a selected April–June 2026 window.
- Evaluation uses one client-grouped holdout split.
- Metadata coverage is uneven for some variables.
- The study is observational.
- Results should not be treated as causal evidence.
- Results should not be generalized to Google's internal ranking mechanisms.

---

## 10. Reproducibility

The repository is organized so a reviewer can inspect:

```text
work/
├── notebooks/
│   └── capstone.ipynb
└── outputs/
    ├── capstone_model_comparison.csv
    └── capstone_ranked_recommendations.csv

docs/
└── index.html

submission/
└── paper_url.txt
```

The capstone notebook should be run top-to-bottom before final submission.

---

## 11. Data credit

Built on the **FlyRank ML Internship dataset**.

FlyRank:
https://flyrank.ai

---

## 12. Final submission checklist

- [ ] `work/notebooks/capstone.ipynb` is present.
- [ ] `work/outputs/capstone_model_comparison.csv` is present.
- [ ] `work/outputs/capstone_ranked_recommendations.csv` is present.
- [ ] `docs/index.html` is present.
- [ ] GitHub Pages is deployed successfully.
- [ ] `submission/paper_url.txt` contains exactly one direct paper URL.
- [ ] No private client information is exposed.
- [ ] Claims use careful research language.
- [ ] The repository is pushed and clean.
