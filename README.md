# Capital Allowance Priority System

![Analytics](https://img.shields.io/badge/Analytics-Prioritisation-blue)
![Model](https://img.shields.io/badge/Model-Explainable%20Scoring-green)
![CRM](https://img.shields.io/badge/Target-Salesforce-00A1E0)
![Market](https://img.shields.io/badge/Market-UK-red)

## Overview

This project explores the feasibility of building a **data-driven account prioritisation system for Capital Allowance opportunities in the UK market**.

The objective was to move away from static, rule-of-thumb prospecting criteria and determine whether a combination of financial, property and company-level signals could provide a more evidence-based way to rank accounts.

The project covers the full analytical journey:

**Business assumptions → Data feasibility → Feature engineering → Statistical calibration → Priority scoring → Explainable CRM output**

The resulting concept is designed to be:

- Explainable
- Auditable
- Scalable
- Business-readable
- Compatible with Salesforce
- Adaptable as additional outcome data becomes available

---

## Live Project

The full interactive analysis and findings report is available here:

**[View the Capital Allowance Priority System](https://rawa-elargab.github.io/CA_Prio_System/)**

---

# Business Problem

Sales teams need to determine which companies are most likely to present meaningful **Capital Allowance opportunities**.

Traditional targeting can rely on isolated rules such as:

- Company turnover
- Employee count
- Fixed assets
- Industry
- Property ownership

However, no individual metric provides sufficient evidence on its own.

The analytical question therefore became:

> **Can multiple observable company signals be combined into a transparent priority score that helps sales teams focus on the most promising accounts?**

The project was designed to answer four questions:

1. Is the necessary data available and sufficiently reliable?
2. Do existing business assumptions hold when tested against historical data?
3. Which signals appear most useful when combined?
4. Can those signals be converted into a transparent and operational prioritisation framework?

---

# Business Context

Capital Allowances can be associated with qualifying investment in areas such as:

- Plant and machinery
- Integral building features
- Commercial property fit-outs
- Heating and electrical installations
- Lifts and air-conditioning
- Property refurbishment
- Recent property acquisition
- Capital investment projects

The analysis therefore looks for company characteristics that may act as observable indicators of this type of investment activity.

Potential exclusions are also considered, including certain industries, residential-focused businesses and company profiles that are outside the desired commercial targeting population.

---

# Analytical Approach

The project follows a structured analytical workflow.

```text
Business Targeting Rules
          ↓
Data Source Assessment
          ↓
Historical Outcome Overlay
          ↓
Feature Engineering
          ↓
Signal Validation
          ↓
Statistical Weight Calibration
          ↓
Priority Score
          ↓
Priority Tier
          ↓
Explainable CRM Output
          ↓
Conversion Monitoring
```

This separates two important questions:

> **Does a signal appear useful?**

from:

> **How should that signal contribute to operational prioritisation?**

---

# Data Architecture

Multiple data sources were assessed to capture complementary signals.

## 1. Financial Data

Structured company financial information provides measures such as:

- Fixed assets
- Tangible assets
- Year-on-year asset movements
- Turnover
- Employee count

These variables form the core of the initial MVP.

---

## 2. Statutory Accounts

Companies House accounts can provide more detailed financial information including:

- Tangible asset notes
- Land and building information
- Asset additions
- Asset disposals
- Lease commitments

These can be used to refine the financial-investment signal.

---

## 3. Property Ownership Data

HM Land Registry information can provide:

- Property ownership
- Freehold / leasehold status
- Number of properties
- Property addresses
- Ownership timing
- Recent acquisitions

Property ownership provides an additional dimension that cannot be captured from financial statements alone.

---

## 4. External Event Signals

Additional signals could later include events such as:

- Relocation
- Refurbishment
- Expansion
- Property acquisition
- Major capital projects

Potential sources include company websites, news and other external information.

These signals are considered optional refinements rather than requirements for the initial MVP.

---

# Entity Matching Strategy

Datasets are linked using **company registration number** wherever possible.

This provides deterministic entity matching and improves:

- Traceability
- Reproducibility
- Auditability
- Data-quality control

Avoiding fuzzy company-name matching also reduces the risk of assigning property or financial information to the wrong legal entity.

---

# Feature Engineering

Raw company metrics exist on very different numerical scales.

For example:

```text
Employees              → hundreds / thousands
Turnover               → millions
Asset movements        → thousands / millions
Property count         → small integer
Ownership recency      → binary / time-based
```

Directly combining these values would allow high-scale variables to dominate the scoring model.

The project therefore transforms raw measures into **relative percentile-based features**.

---

## Core Features

| Feature | Purpose |
|---|---|
| **Tangible Investment Score** | Measures relative year-on-year tangible asset growth |
| **Fixed Asset Movement Score** | Measures relative movement in fixed assets |
| **Ownership Score** | Represents evidence of relevant property ownership |
| **Ownership Recency Score** | Captures recent property acquisitions |
| **Company Size Score** | Combines employee and turnover information |

Percentile transformation provides several advantages:

- Companies become comparable across different numerical scales
- Individual high-value variables cannot dominate simply because of their units
- Scores remain interpretable between approximately 0 and 1
- The scoring framework can scale as the company population grows

---

# Historical Validation

Historical signed accounts were overlaid with the engineered signals to determine whether the proposed business indicators showed meaningful patterns.

The feasibility sample contained:

- **115 companies analysed**
- **98 signed accounts**
- **17 not-signed accounts**

This represents a heavily signed-skewed sample, so the analysis is treated as **signal validation and calibration**, not as a final predictive performance assessment.

---

# Finding 1 — Financial Investment Signals

Historical signed accounts tended to appear more frequently in higher percentiles of:

- Tangible asset movement
- Fixed asset movement

The separation was particularly visible for tangible asset changes.

### Interpretation

This supports the business hypothesis that recent financial investment activity can provide useful evidence when assessing potential Capital Allowance opportunities.

However:

> **Financial investment is treated as a foundational signal rather than a sufficient prioritisation rule by itself.**

---

# Finding 2 — Property Ownership

Property ownership added another important dimension.

Within the historical signed population:

- Freehold ownership appeared frequently
- Recent property acquisition was also common

### Interpretation

Companies displaying both:

```text
Investment activity
        +
Property ownership
        +
Recent acquisition
```

may provide stronger evidence than companies identified through a financial metric alone.

This demonstrates why a **multi-signal approach** is preferable to individual threshold rules.

---

# Finding 3 — Company Scale

Employee count and turnover were also evaluated.

Historical signed accounts covered a relatively broad size spectrum but tended to cluster around mid-to-upper company-size ranges rather than at the extremes.

Company size was therefore treated primarily as:

- A normalisation signal
- A segmentation feature
- A potential exclusion mechanism

rather than the main predictor of Capital Allowance opportunity.

---

# Statistical Calibration

Once the candidate features were engineered, a logistic regression was used to assess their **relative relationship with historical signed outcomes**.

The purpose of the regression was not to deploy a black-box prediction model.

Instead, it was used as a calibration mechanism:

```text
Historical Outcomes
        ↓
Standardised Features
        ↓
Logistic Regression
        ↓
Relative Signal Importance
        ↓
Transparent Scoring Weights
```

This allows historical evidence to influence the score while keeping the final prioritisation logic understandable by business users.

---

# Why Not Use the Regression Directly?

A pure predictive model could return a probability such as:

```text
P(Signing) = 0.7342
```

but this alone would give sales users limited visibility into the reason behind the recommendation.

Instead, the project converts the learned importance of the signals into an explainable scoring framework.

For each company, users can see:

- Overall priority score
- Priority tier
- Individual feature values
- Main scoring drivers

This makes the result easier to challenge, validate and govern.

---

# Priority Scoring

The conceptual scoring model follows:

```text
Priority Score
    =
Σ (Feature Score × Calibrated Weight)
```

Each company therefore receives a combined score based on several independent business signals.

Example:

```text
Company A

Tangible Investment     0.91
Fixed Asset Movement    0.78
Ownership               0.74
Ownership Recency       0.60
Company Size            0.63

              ↓

Priority Score           0.82

              ↓

Priority Tier            HIGH
```

The exact contribution of each feature remains visible.

---

# Priority Tiers

To make the score operationally useful, companies are grouped into three priority levels.

| Priority | Population | Interpretation |
|---|---:|---|
| **High** | Top 15% | Strong multi-signal evidence |
| **Medium** | Next 35% | Moderate or partial evidence |
| **Low** | Remaining 50% | Weak, old or limited evidence |

Using percentile-based tiers provides several benefits:

- Avoids relying entirely on manually chosen absolute thresholds
- Keeps the distribution manageable as the account universe grows
- Can align the number of priority accounts with sales capacity
- Makes segmentation easy to communicate

---

# Explainability

Explainability was a core design requirement.

Every prioritised company can expose an output similar to:

```text
Account: Example Company Ltd

Priority Score: 0.82
Priority Tier: HIGH

Main Drivers:
• Tangible Investment Score = 0.91
• Ownership Score = 0.74
• Company Size Score = 0.63
```

A salesperson can therefore understand:

> **Why has this account been prioritised?**

rather than receiving only an unexplained model score.

---

# Governance

The system was designed with analytics governance in mind.

Each score should remain traceable to:

```text
Source Data
    ↓
Raw Signal
    ↓
Transformation
    ↓
Feature Score
    ↓
Weight
    ↓
Final Priority Score
    ↓
Priority Tier
```

This supports:

- Auditability
- Model review
- Business challenge
- Recalibration
- Sales adoption
- Data-quality investigation

---

# Salesforce Integration Concept

The final scoring output is intentionally simple enough to integrate into a CRM.

Example fields could include:

| Salesforce Field | Example |
|---|---|
| `CA Priority Score` | 0.82 |
| `CA Priority Tier` | High |
| `CA Tangible Signal` | 0.91 |
| `CA Ownership Signal` | 0.74 |
| `CA Size Signal` | 0.63 |
| `CA Score Date` | 2026-02-01 |
| `CA Priority Reason` | High investment + property ownership |

This allows sales users to filter and target accounts directly from their existing workflow.

---

# From Analytics to Action

The final product concept connects analysis to commercial execution.

```text
Data Signals
     ↓
Priority Score
     ↓
Priority Tier
     ↓
Salesforce
     ↓
Sales Outreach
     ↓
Opportunity / Conversion
     ↓
Outcome Data
     ↓
Model Recalibration
```

This feedback loop is important because the initial model should evolve as more real conversion outcomes become available.

---

# Monitoring Framework

Once operationalised, the scoring system should be monitored continuously.

### Suggested KPIs

| KPI | Purpose |
|---|---|
| Conversion Rate by Priority Tier | Tests whether prioritisation improves targeting |
| Opportunity Rate by Tier | Measures commercial engagement |
| Revenue by Tier | Evaluates value generated |
| High-Priority Coverage | Determines whether sales teams action recommendations |
| Signal Coverage | Measures availability of input data |
| Property Match Rate | Monitors Land Registry coverage |
| Score Distribution | Detects population shifts |
| Tier Migration | Tracks companies changing priority over time |
| Model Lift | Compares prioritisation with baseline/random targeting |

---

# Important Analytical Limitation

The feasibility dataset is strongly imbalanced toward historical signed accounts.

The sample contains:

```text
98 Signed
17 Not Signed
```

This means the initial results should **not be interpreted as proof of predictive accuracy**.

For example, signed rates across the initial priority tiers do not yet show a clean monotonic relationship where High consistently outperforms Medium and Low.

This suggests that the current analysis is best interpreted as:

> **Evidence that the proposed signals are feasible and potentially useful**

rather than:

> **A production-validated predictive model**

A stronger validation phase would require a larger and more representative population of both successful and unsuccessful historical prospects.

This distinction is important when moving from analytical feasibility to operational deployment.

---

# Recommended Next Steps

## 1. Business Validation

Review high-priority outputs with Capital Allowance specialists.

Questions include:

- Do the accounts make commercial sense?
- Are important businesses missing?
- Are obvious false positives present?

---

## 2. Finalise Exclusion Rules

Confirm business rules around:

- SIC exclusions
- Residential companies
- Property developers
- Company-size limits
- Asset-value thresholds

---

## 3. Expand the Historical Sample

Increase the number of:

- Signed accounts
- Non-signed accounts
- Lost opportunities
- Accounts approached but not converted

This will allow more robust statistical validation.

---

## 4. Back-Test the Model

Evaluate metrics such as:

```text
Precision
Recall
Lift
Conversion by tier
Top-decile capture
```

The most important business test is whether sales teams achieve better outcomes when working from the prioritised population.

---

## 5. Salesforce MVP

Expose the main outputs directly to sales:

```text
Priority
Score
Reason
Key Signals
Last Refresh
```

---

## 6. Monitor Outcomes

Track conversion by tier and compare against a baseline population.

---

## 7. Recalibrate

As new outcomes accumulate:

```text
New Sales Outcomes
      ↓
Updated Training Population
      ↓
Re-estimate Signal Importance
      ↓
Validate New Weights
      ↓
Deploy New Scoring Version
```

Model versions should be retained for traceability.

---

# What This Project Demonstrates

This project demonstrates competencies across several areas.

### Business Analysis

- Translating commercial assumptions into testable analytical hypotheses
- Defining inclusion and exclusion criteria
- Connecting analysis with operational business processes

### Data Analysis

- Multi-source data integration
- Exploratory analysis
- Distribution analysis
- Historical outcome comparison
- Percentile transformations

### Statistical Analysis

- Feature engineering
- Standardisation
- Logistic regression
- Relative signal calibration
- Score construction
- Tiering

### Data Product Thinking

- Designing an MVP
- Building for CRM integration
- Defining feedback loops
- Planning monitoring and recalibration

### Data Governance

- Deterministic company matching
- Explainable scoring
- Data lineage
- Traceable transformations
- Model versioning
- Auditable outputs

### Stakeholder Communication

- Translating statistical outputs into business-readable signals
- Presenting model drivers rather than black-box predictions
- Creating actionable priority tiers for sales teams

---

# Project Deliverables

The project produced:

- Data feasibility assessment
- Source mapping
- Analytical validation of business assumptions
- Feature engineering framework
- Statistical calibration approach
- Priority scoring methodology
- Priority segmentation
- Explainability framework
- Salesforce integration concept
- Recommendations for deployment and monitoring


# Data Confidentiality

This repository is intended to demonstrate the **analytical methodology and product design** behind the project.

Any publicly available version should use:

- Aggregated findings
- Anonymised examples
- Synthetic account examples
- Non-sensitive model logic

Confidential customer information, internal identifiers, credentials and proprietary datasets should not be included.


Areas demonstrated in this project:

- Business Analytics
- Sales Prioritisation
- Feature Engineering
- Statistical Analysis
- Explainable Scoring
- Data Governance
- CRM Analytics
- Data Product Design
