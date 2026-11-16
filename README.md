# Car Price Prediction

A regression project estimating fair market value for used vehicles, benchmarking four model families and choosing between them on the basis of which prediction error is more expensive in the real world.



---

## Data

2,230 vehicle listings, 32 features covering make and model, year, physical characteristics, performance indicators, condition, fuel type, transmission and insurance status. Target: price.

Price distribution: mean £17,242, median £17,393, standard deviation £8,961, range £3,528 – £51,219.

## Data quality work

Real listings data, and it showed:

- Door counts above 8 (one record said 44) replaced with the median of 4
- Engine sizes above 8 litres capped at the median of 2.28L
- Negative monthly mileage values replaced with the median
- Missing categorical values imputed by mode, numerical by median
- Category standardisation — "Toyota Camary" → "Toyota Camry", "3rd Party" → "Third Party", whitespace stripped from cylinder counts

## Modelling

One-hot encoding expanded the feature space to 71 columns. Random Forest feature importance selected the top ten predictors; `Estimated_Mileage` was dropped for redundancy against `monthly_mileage` and replaced with `condition`.

| Model | Config | Test MAE |
|---|---|---|
| Decision Tree | default | 1432 |
| Decision Tree | max depth 10 | 1415 |
| **KNN** | **k = 8** | **1619** |
| KNN | k = 3 | 1690 |
| Neural Network | both layers, 3 nodes | 1402 |
| Random Forest | default | 1464 |
| Random Forest | 201 trees, depth 20 | 1483 |

Random Forest reached the lowest *training* error (941) while its test error rose — a clear overfitting signature. KNN at k=8 showed the smallest train/test gap (1613 vs 1619), which is why it was selected: stability on unseen listings mattered more than the best single test score.

## The stakeholder argument

Over-prediction and under-prediction are not symmetric. Over-pricing causes buyers to overpay, lenders to over-collateralise loans and dealers to lose sales — direct financial loss and reputational damage. Under-pricing costs margin but retains the customer. The model selection reflects that asymmetry rather than treating all error as equal.

Top predictor: monthly mileage, strongly negative against price. Also material: colour (silver commands a premium), number of previous owners, engine size, weight, insurance status.

## Stack

Python · pandas · NumPy · scikit-learn · seaborn · Matplotlib · IBM SPSS Modeler

## Repository

```
notebooks/    EDA, cleaning, visualisation, predictor selection
reports/      Written report
```

## Attribution

Group coursework produced collaboratively across modelling, evaluation and the written report. Contributor names and student identifiers have been removed from all published material.

---

*Grace Kagori — MSc Business Analytics, Aston University*
