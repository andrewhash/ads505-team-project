# ads505-team-project

Predicting nightly Airbnb prices in San Diego, built for the ADS-505 (Applied Statistics for Data Science) final team project at the University of San Diego.

## Team

- Andrew Hashoush
- Cassandra Avelar
- Kalixta Escoto

## Project overview

Airbnb hosts and small property managers in San Diego often set nightly prices by instinct or by eyeballing a few nearby listings. This project builds a model that recommends a competitive, data backed nightly price from a listing's own features and location, then translates the strongest price drivers into plain recommendations a host could actually act on.

**Goal:** predict nightly listing price (regression) from listing characteristics, and identify which features move price the most.

## Dataset

Inside Airbnb, San Diego listings snapshot (2026). http://insideairbnb.com/get-the-data.html

- 10,666 cleaned listings after preprocessing
- 12 features used in modeling, including room type, neighborhood, bedrooms/bathrooms, accommodates, review scores, and superhost status
- Target variable: `price` (nightly rate, continuous)

## Methods

- Data cleaning: price field converted from text to numeric, scrape metadata and raw HTML description fields dropped, missing review/host response values handled
- Train/validation/test split: 60/20/20
- Baseline model: linear regression
- Final model: random forest (100 trees, tuned on the validation set)
- Hypothesis testing: Welch's t-test on the superhost price premium
- Final metrics reported once, on the held out test set

## Results

| Model | R² | MAE | RMSE |
|---|---|---|---|
| Linear regression (baseline) | 0.593 | $103.73 | $144.92 |
| Random forest (final) | 0.678 | $89.62 | $128.95 |

Superhost listings averaged $382.51/night vs. $311.70 for non-superhosts (Welch's t-test, t = 15.81, p < .001).

The random forest model was selected as the final model: it explains about 68% of price variation and cuts average pricing error by roughly 14% versus the linear regression baseline.

## Repository structure

> Update this section to match the actual folders/notebooks in the repo.

\`\`\`
ads505-team-project/
├── data/              # raw and cleaned listings data
├── notebooks/          # EDA, cleaning, and modeling notebooks
├── reports/            # final deliverables (technical report, executive brief, slides)
└── README.md
\`\`\`

## Reports

- Technical report: for reproducing the analysis
- Executive brief: a short, non-technical summary for hosts/property managers

## Requirements

Python 3, pandas, numpy, scikit-learn, matplotlib/seaborn, scipy.

\`\`\`
pip install -r requirements.txt
\`\`\`
