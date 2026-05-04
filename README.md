# Infliximab (Remicade) vs Inflectra (Biosimilar) — Pharmacovigilance Analysis Using FAERS

![Python](https://img.shields.io/badge/Python-3.x-blue)
![Power BI](https://img.shields.io/badge/Power%20BI-Certified-yellow)
![Data Source](https://img.shields.io/badge/Data-FDA%20AEMS-red)
![Status](https://img.shields.io/badge/Status-Completed-green)

## 📋 Project Overview
A real-world pharmacovigilance analysis comparing the adverse event profile of 
Infliximab (Remicade) — the originator biologic — with its biosimilar Inflectra, 
using data from the FDA Adverse Event Monitoring System (AEMS), formerly known as FAERS.

This project demonstrates end-to-end data analytics skills combining:
- **Python** for data cleaning, transformation and signal detection
- **Power BI** for interactive dashboard development
- **PharmD domain knowledge** for clinical interpretation of findings

 ## 🎯 Objectives
- Compare adverse drug reaction (ADR) profiles between Infliximab and Inflectra
- Perform disproportionality analysis using Reporting Odds Ratio (ROR) and 
  Proportional Reporting Ratio (PRR)
- Identify statistically significant pharmacovigilance signals
- Assess outcome severity and demographic characteristics
- Evaluate biosimilar equivalence from a real-world safety perspective

 ## 📊 Data Source
**FDA Adverse Event Monitoring System (AEMS)**
- Time Period: Q4 2025 (October–December 2025) and Q1 2026 (January–March 2026)
- Format: ASCII quarterly data files
- Tables used: DEMO, DRUG, REAC, OUTC, INDI
- Download: https://www.fda.gov/drugs/fda-adverse-event-monitoring-system-aems/fda-adverse-event-monitoring-system-aems-latest-quarterly-data-files

> Note: Raw data files are not included in this repository due to size constraints.
> Please download directly from the FDA website using the link above.

## 🔬 Methodology

### Data Pipeline (Python)

**Step 1 — Data Loading**
- Loaded 5 core FAERS tables for each quarter
- Used `$` delimiter and `latin-1` encoding specific to FAERS ASCII format

**Step 2 — Data Combination**
- Combined Q4 2025 and Q1 2026 using `pd.concat`
- Total cases before cleaning: 782,512

**Step 3 — Deduplication**
- FAERS contains multiple versions of the same case (initial + follow-up reports)
- Retained only the latest `caseversion` per `caseid`
- Removed 26,530 duplicate case versions
- Final unique cases: 755,982

**Step 4 — Data Synchronization**
- Filtered all tables to match deduplicated DEMO table using `primaryid`
- Removed orphaned rows from duplicate case versions

**Step 5 — Drug Filtering**
- Identified all name variations of Infliximab and Inflectra
- Filtered for Primary Suspect (PS) role only
- Originator cases: 4,051 | Biosimilar cases: 587

**Step 6 — Data Cleaning & Standardization**
- Age unit conversion (Years, Decades, Months → Years)
- Age group categorization (<18, 18-35, 36-50, 51-65, >65)
- Sex standardization (M/F/NaN → Male/Female/Unknown)
- Country code mapping to full names
- Reporter type standardization
- Outcome code mapping to readable labels
- Noise term removal from reaction data

**Step 7 — Signal Detection**
- Background: Entire cleaned FAERS database (755,982 cases)
- Method: Disproportionality analysis using ROR and PRR
- Significance criteria: A ≥ 3 AND ROR lower 95% CI > 1
- Significant signals detected: 676 (Infliximab) | 186 (Inflectra)

## 📈 Key Findings

### Adverse Reaction Profile
- ADR profiles are broadly similar between Infliximab and Inflectra
- Top reactions in both groups: Condition aggravated, Arthralgia, Fatigue, 
  Blood pressure increased, Nausea
- Similarity in ADR profiles supports biosimilar therapeutic equivalence

### Signal Detection
| Signal | Infliximab ROR | Inflectra ROR | Interpretation |
|--------|---------------|---------------|----------------|
| Drug level below therapeutic | 151.18 | 65.35 | Immunogenicity/TDM signal |
| Anal fissure | 132.33 | 77.77 | Disease related (Crohn's) |
| Erythema nodosum | 104.66 | 91.87 | Paradoxical anti-TNF reaction |
| Drug specific antibody present | 68.29 | — | Immunogenicity confirmed |
| Fistula | 81.65 | 60.84 | Disease related (Crohn's) |

### Clinical Outcomes
| Outcome | Infliximab | Inflectra |
|---------|-----------|-----------|
| Other | 70% | 70% |
| Hospitalization | 23% | 26% |
| Death | 2.4% | 1.1% |
| Life Threatening | 1.9% | 1.3% |
| Disability | 1.3% | 2.0% |

### Demographics
- Mean age: ~45 years in both groups (consistent with autoimmune disease epidemiology)
- Canada dominates reporting in both groups — reflects manufacturer-initiated 
  pharmacovigilance reporting obligations
- Significant demographic data missingness: Sex missing 72% (Infliximab) 
  and 97% (Inflectra)

## 🏁 Conclusion
Inflectra demonstrates a broadly comparable safety profile to its reference 
product Infliximab across adverse reaction types, signal patterns and outcome 
severity. Key findings support biosimilar equivalence while highlighting the 
importance of:

1. **Routine Therapeutic Drug Monitoring (TDM)** for long-term patients
2. **Causality assessment** for disease-related signals (anal fissure, fistula)
3. **Continued post-marketing surveillance** with expanded data collection

## ⚠️ Limitations
- FAERS captures reported events not confirmed ADRs — causality cannot be established
- Entire FAERS database used as background comparator — restricted comparator 
  (same MOA drugs) would reduce confounding by indication
- Significant demographic data missingness limits subgroup analysis
- Small biosimilar sample size (587 cases) limits statistical power
- Geographic bias toward North America — European Inflectra use underrepresented 
  as ADRs are primarily reported to EMA
- March 2026 reporting spike reflects EU/UK batch reporting artifact, 
  not a real safety signal

  ## 🛠️ Tools & Libraries
  **Python:**
- pandas
- numpy
- scipy
- os
  
**Power BI:**
- Power BI Desktop
- DAX calculated columns
- Custom date table
- Filled map visualization
- Bookmark toggle buttons
  
## 📁 Repository Structure
FAERS-Infliximab-Inflectra-Analysis/
│
├── README.md
├── Infliximab_Vs_Inflectra.ipynb    ← Complete Python pipeline
├── requirements.txt
│
├── data/
│   └── README.md                    ← Data download instructions
│
├── exports/
│   └── README.md                    ← Exported CSV descriptions
│
└── dashboard/
└── screenshots/
├── 01_overview.png
├── 02_adverse_reactions.png
├── 03_demographics.png
└── 04_outcomes_signals.png

## 👤 Author
**[Nick Iver Majaw]**
- PharmD Graduate
- Power BI Certified
- Python Data Analytics

📧 [nickivermajaw086@gmail.com]
🔗 [www.linkedin.com/in/nick-iver-majaw]

---

## 📚 References

1. FDA Adverse Event Monitoring System (AEMS): 
   https://www.fda.gov/drugs/surveillance/fda-adverse-event-monitoring-system-aems
2. MedDRA Terminology: https://www.meddra.org
3. EMA Guidelines on Similar Biological Medicinal Products
4. Van Puijenbroek EP et al. A comparison of measures of disproportionality 
   for signal detection in spontaneous reporting systems for adverse drug reactions. 
   Pharmacoepidemiol Drug Saf. 2002
