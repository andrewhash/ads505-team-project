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

- The raw file is already included at `data/listings.csv.gz`, so no download is needed
- Raw data: 13,225 listings and 90 columns
- 10,666 listings after cleaning and filtering to nightly prices between $10 and $1,000
- 9 features used in modeling: accommodates, bedrooms, bathrooms, review score rating, neighborhood, superhost status, minimum nights, number of reviews, and availability over the year
- Target variable: `price` (nightly rate, continuous)

## Setup
1. Clone the repo:
```
   git clone https://github.com/andrewhash/ads505-team-project.git
   cd ads505-team-project
```
2. Install dependencies:
```
   pip install pandas numpy scikit-learn matplotlib seaborn scipy jupyter
```

## How to run
1. Launch Jupyter from the project root:
```
   jupyter notebook
```
2. Open `notebooks/analysis.ipynb` and run all cells (Kernel > Restart & Run All).

The notebook runs the full analysis from top to bottom: loading and cleaning the data, EDA visualizations, the superhost t-test, the linear regression baseline, random forest tuning, and final test set metrics. A `random_state` of 42 is set throughout, so your results should match the table in the Results section. The random forest steps take a minute or two to run.

## Methods
- **Data cleaning:** price converted from text to numeric, host_response_rate dropped (100% empty), missing bedrooms filled with the median, missing review scores filled with the median and flagged, rows with missing bathrooms, minimum nights, or superhost status dropped, and bathroom counts extracted from the bathrooms text field
- **Outlier handling:** listings limited to nightly prices between $10 and $1,000
- **Train/validation/test split:** 60/20/20
- **Baseline model:** linear regression
- **Final model:** random forest (100 trees, tuned on the validation set)
- **Hypothesis testing:** Welch's t-test on the superhost price premium
- Final metrics reported on the held out test set

## Results
| Model | R² | MAE | RMSE |
|---|---|---|---|
| Linear regression (baseline) | 0.593 | $103.73 | $144.92 |
| Random forest (final) | 0.678 | $89.62 | $128.95 |

Superhost listings averaged $382.51/night vs. $311.70 for non-superhosts (Welch's t-test, t = 15.81, p < .001).

The random forest was selected as the final model. It explains about 68% of price variation and cuts average pricing error by roughly 14% compared to the linear regression baseline.

## Limitations
- Only 9 features were used, so things like amenities, photos, and description text were left out
- Prices were capped at $1,000/night, so the model isn't reliable for luxury listings
- No seasonal or time based features were included

## Requirements
Python 3 with the following packages: pandas, numpy, scikit-learn, matplotlib, seaborn, scipy, and jupyter. The install command is in the Setup section above.

## Team contributions
- **Andrew Hashoush:** exploratory data analysis (EDA)
- **Cassandra Avelar:** modeling (linear regression baseline and random forest)
- **Kalixta Escoto:** hypothesis testing and final metrics
