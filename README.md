# CV Keyword Matching 
Naive keyword-matching baseline for ranking CVs against jobs, built as the performance floor for an AI screening tool for a recruitment process.

## Dataset

"Resume Dataset" by **Saugata Roy Arghya** on Kaggle: https://www.kaggle.com/datasets/saugataroyarghya/resume-dataset

Licence: *[add licence from the Kaggle page]*

The dataset is **not included** in this repo. Download `resume_data.csv` and place it in the `data/` folder.

Each row pairs one CV with one job and a `matched_score` (0–1): 9,544 pairs formed by 344 unique CVs scored against 28 jobs.

## Project structure

```
CV-Keyword-matching/
├── data/                    # not committed (gitignored)
│   ├── resume_data.csv      # raw dataset from Kaggle
│   └── resume_clean.pkl     # cleaned data, created by notebook 1
├── notebooks/
│   ├── 01_eda.ipynb         # EDA and cleaning
│   └── 02_baseline.ipynb    # baseline, evaluation, weaknesses, summary
├── requirements.txt
└── README.md
```

## Notebooks (run in order)

1. **`01_eda.ipynb`**: EDA and cleaning. Covers the column structure, the CV–job grid, real and hidden missing values, target (`matched_score`) analysis, cleaning, and keyword extraction. Saves `data/resume_clean.pkl`.
2. **`02_baseline.ipynb`**: builds the baseline, ranks CVs per job, evaluates against `matched_score` (Spearman and tie-aware precision@10 vs random), probes weaknesses, and summarises the results.

## Baseline

For each job, count how many of the job's keywords appear in each CV's skills, then rank CVs from highest to lowest count.

- **Job keywords:** job title + `skills_required` + job responsibilities
- **CV keywords:** `skills` + `related_skills_in_job`
- Lowercased, split into words, stopwords removed, each keyword counted once

It is deliberately naive: no weighting, no synonyms, no stemming, no learning.

## Key results

| | Spearman | Precision@10 | Random precision@10 |
|---|---|---|---|
| All 28 jobs | 0.285 | 0.33 | 0.125 |
| Business/HR/finance slice (8 jobs) | 0.385 | 0.35 | 0.093 |

The baseline beats random, but weakly: about 7 of every 10 shortlisted CVs are not true top candidates.

**Main weaknesses:**
- A keyword-stuffed CV (copied job advert) ranks 1st of 343.
- Genuine experience described in different words is missed.
- Generic words (`management`, `office`) create false positives.
- CVs that list more skills score higher (length bias).
- Heavy ties make shortlists partly arbitrary.

See the summary at the end of `02_baseline.ipynb` for full results and dataset limitations.

## Setup

```bash
python -m venv .venv
.venv\Scripts\activate
pip install -r requirements.txt
```

Then open the notebooks in VS Code or Jupyter, select the `.venv` kernel, and run `01_eda.ipynb` before `02_baseline.ipynb`.