# ADS1002-wche0270

## Run the project notebook

Clone or download this repository, open `project_notebook.ipynb` from its root,
and run all cells in order using Python with pandas, matplotlib and scikit-learn.
All required input data are in this repository; no separate desktop folder or
GitHub login is needed to run a local checkout.

- `data/raw/Rossmann_train.csv.gz`: original supplied training CSV, losslessly
  compressed. The notebook loads this file and rebuilds the cleaned analytical
  data in memory before EDA and modelling.
- `data/cleaned/Rossmann_train_clean.csv.gz`: retained cleaned output reference.
- `data/cleaned/Rossmann_store_clean.csv`: retained reference only; not used by
  the notebook. The competition test dataset is not needed for this project.

Raw and cleaned training files are two stages of the same dataset. Notebook
execution does not overwrite either file. Pandas reads `.csv.gz` directly.

Original file supplied on 6 September 2026: `Rossmann_train.csv`.
SHA-256 of the **decompressed original CSV**:
`f6e4597c142d7d909a13d53b68a8e85c00b9a4c7b5ff40adbb37d6829cc1f4cc`.
## Key Results

### Data and Store Selection

- The dataset contains **1,017,209 store-day observations** from **1,115 Rossmann stores**.
- The top **20% highest-sales stores (223 stores)** were selected using sales from 1 January to 3 July 2015.
- These 223 stores contributed approximately **30.5% of total sales** across all 1,115 stores during the ranking period.
- Data quality checks found no missing cells, exact duplicate rows, or duplicate Store-Date records.

### Exploratory Data Analysis

- Customer traffic showed the strongest observed association with sales, with a Pearson correlation of **0.859** across all recorded days.
- Among open stores, average daily sales were approximately:
  - **12,178** during promotion periods
  - **9,080** without promotion
- This represents an observed sales difference of approximately **34%** during promotional periods.
- Mean daily portfolio sales increased from approximately **1.863 million in 2013** to **1.984 million in 2015** for comparable January-June periods.

### Baseline vs Multiple Linear Regression

A weekday-mean baseline was compared with a multiple linear regression model using weekday, store opening, promotion, public-holiday and school-holiday information.

| Model | MAE | RMSE | R² |
|---|---:|---:|---:|
| Weekday Baseline | 264,217 | 333,843 | 0.838 |
| Multiple Linear Regression | **109,611** | **141,682** | **0.971** |

The regression model:

- Reduced MAE by approximately **58.5%**
- Reduced RMSE by approximately **57.6%**
- Achieved an evaluation **R² of 0.971**
- Forecast four-week total sales of **57.33 million**, compared with actual sales of **56.54 million**
- Produced a total forecast error of approximately **+1.40%**

### Extended Model Comparison

Seven forecasting approaches were compared using chronological validation:

- Weekday Baseline
- Multiple Linear Regression
- Trend Regression
- Ridge Regression
- Tuned Ridge Regression
- Decision Tree
- Random Forest

| Model | Mean MAE | Mean RMSE | Mean Absolute Total Error |
|---|---:|---:|---:|
| Random Forest | **160,118** | 255,617 | 4.87% |
| Trend | 161,659 | **223,369** | **2.10%** |
| Regression | 164,256 | 246,373 | 5.62% |
| Tuned Ridge | 165,836 | 248,878 | 5.71% |
| Ridge | 166,285 | 249,400 | 5.90% |
| Decision Tree | 197,203 | 302,425 | 5.53% |
| Baseline | 397,693 | 573,664 | 5.13% |

Under the predefined mean-MAE selection rule, **Random Forest achieved the best overall development performance**, with a mean MAE of approximately **160,118**.

### Random Forest Performance

The Random Forest model used:

- `n_estimators = 100`
- `max_depth = 4`
- `min_samples_leaf = 5`
- Bootstrap sampling with 50% of observations per tree

Key results:

- Mean development MAE: **160,118**
- Approximately **2.5% lower MAE than the original regression**
- Approximately **18.8% lower MAE than the Decision Tree**
- July 28-day MAE: **138,087**
- July total forecast error: **-1.07%**
- No negative sales forecasts were produced.

### Holiday Forecasting

Holiday dates were particularly challenging for the linear models.

Random Forest improved performance on four observed holiday dates:

- Holiday MAE: **48,195**
- Approximately **80% lower than Regression**
- Approximately **85.7% lower than Decision Tree**
- No negative holiday forecasts

For 14 May 2015:

- Actual sales: **219,056**
- Random Forest prediction: **212,782**
- Absolute error: approximately **6,274**

### Overall Conclusion

The results show that simple linear regression performs very strongly on the July evaluation period, while Random Forest provides more consistent performance across multiple historical validation windows and produces more realistic forecasts during holiday periods.

Random Forest was therefore selected as the leading candidate under the project's mean-MAE selection rule, while Trend Regression and Multiple Linear Regression remain useful benchmark models because they achieve lower RMSE or stronger performance in specific evaluation periods.

