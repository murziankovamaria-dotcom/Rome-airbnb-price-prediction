# Rome Airbnb Price Prediction

Data analysis project in R that explores Airbnb listing prices in Rome and compares a linear regression, a regression tree, and a random forest for predicting nightly prices.

## Project Overview

Setting a nightly price is a trade-off: too high and demand falls, too low and revenue is lost. Using the Inside Airbnb data for Rome, this project asks two questions:

1. Which listing characteristics have the greatest influence on nightly prices?
2. How accurately can statistical and machine learning models predict the price?

## Dataset

- **Source:** [Inside Airbnb](https://insideairbnb.com/get-the-data/), Rome listings (snapshot from July 2026)
- **Size:** 37,228 listings, 19 columns
- **Target:** nightly price in euros, transformed with the natural logarithm
- **Predictors (7):** room type, neighbourhood, minimum nights, number of reviews, reviews per month, availability (days per year), host listing count
- **Not available in the data:** amenities, bedrooms, bathrooms, property size, photos, listing text

## Workflow

1. Data inspection and cleaning (missing values; empty `neighbourhood_group` column removed)
2. Exploratory data analysis (price distribution, room type, neighbourhood, correlations)
3. Log transformation of price
4. Linear regression (three nested specifications) for interpretation
5. Model comparison on a held-out test set: linear regression, regression tree, random forest
6. Interpretation and business recommendations

## Models and Evaluation

Listings with a missing price or missing reviews per month (listings that have never been reviewed) are removed before modelling, leaving **30,565 listings**. The data were split at random into 80% training and 20% test (`set.seed(123)`); all models use the same split. No cross-validation or hyperparameter tuning was applied.

| Model | RMSE (log price) |
|:--|--:|
| Multiple linear regression | 0.481 |
| Regression tree | 0.502 |
| **Random forest (300 trees)** | **0.465** |

RMSE is computed on log price, so it is not a percentage or euro error. The random forest has the lowest test error, about 3% below the linear baseline. The gain is modest.

## Key Findings

- **Neighbourhood and room type are the strongest drivers of price.** In the random forest, neighbourhood ranks first and room type second.
- Entire homes cost more than hotel, private or shared rooms, and the historic centre (I Centro Storico) has the highest average price.
- Prices are right-skewed. The log transformation makes the distribution much more symmetric.
- Numeric variables are weak individual predictors. In the full OLS model, reviews per month and minimum nights are negatively associated with price, and the host's listing count is positively associated. These are associations, not causal effects.
- The random forest predicts best, which suggests non-linear relationships that the linear model misses, but the gain over the baseline is small.
- The OLS residuals show mild heteroskedasticity and a heavy upper tail, driven by luxury listings.

## Limitations

- Cross-sectional data from one snapshot; no seasonality
- Listings with no reviews are excluded from the models
- No property size, amenities, photos or listing text
- Single train/test split, without cross-validation or tuning
- The full linear model explains about one third of the variation in log price (adjusted R² = 0.354)

## Figures

### Average Price by Neighbourhood

<img src="assets/figures/neighborhood_prices.png" width="850">

Average prices differ substantially across Rome's neighbourhoods, with the historic centre the highest.

### Distribution of Log-Transformed Prices

<img src="assets/figures/log_price_distribution.png" width="700">

The log transformation reduces the right skew of prices.

### Room Type vs Log Price

<img src="assets/figures/room_type_log_prices.png" width="700">

Entire homes command higher prices than private or shared rooms.

### Random Forest Variable Importance

<img src="assets/figures/feature_importance.png" width="800">

In the random forest, neighbourhood is the most important predictor, followed by room type.

### OLS Residual Diagnostics (full model)

<img src="assets/figures/residual-diagnostics.png" width="700">

Residuals are centred around zero, with mild heteroskedasticity and a heavy upper tail.

## Technologies

- **Language:** R, R Markdown
- **Packages:** tidyverse (dplyr, ggplot2), rpart, randomForest, knitr, kableExtra, stargazer

## Repository Structure

```text
rome-airbnb-price-prediction/
├── assets/figures/        figures used in this README
├── data/
│   └── listings.csv       Inside Airbnb Rome listings
├── documentation/         project documentation
├── notebooks/
│   ├── pricing.Rmd        full analysis (R Markdown)
│   └── pricing.html       knitted report
├── presentation/          executive summary slides
├── README.md
├── LICENSE
└── .gitignore
```

## How to Reproduce

1. Open `notebooks/pricing.Rmd` in RStudio.
2. Install the packages listed above.
3. Knit the document. It reads `../data/listings.csv`.

## Author

**Maria Murziankova**

[![GitHub](https://img.shields.io/badge/GitHub-murziankovamaria--dotcom-181717?style=for-the-badge&logo=github)](https://github.com/murziankovamaria-dotcom)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Maria%20Murziankova-0077B5?style=for-the-badge&logo=linkedin)](https://www.linkedin.com/in/maria-murziankova-004522327/)
[![Tableau Public](https://img.shields.io/badge/Tableau-Public-E97627?style=for-the-badge&logo=tableau)](https://public.tableau.com/app/profile/maria.murziankova/vizzes)
