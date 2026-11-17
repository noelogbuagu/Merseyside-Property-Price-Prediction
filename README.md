# Merseyside Property Price Prediction

COMP 702 dissertation project in urban and spatial analytics: predicting property price valuation in Merseyside, UK. It combines sold-price records with local economic, crime and flood-risk data, trains a model to predict price, and presents it in a Streamlit app.

## Approach

1. **Prepare six data sources**, each in its own notebook, and merge them into one modelling table.
2. **Explore** price, property type and property age (univariate and bivariate).
3. **Split by time** (train on earlier sales, test on later ones), because prices trend over time.
4. **Model** price on a log scale. Linear Regression, Random Forest, Gradient Boosting and XGBoost were tuned with grid search; Random Forest was chosen (500 trees, `min_samples_split=5`).
5. **Result:** on the test set, MAE 0.236 and RMSE 0.304 on log price, R² 0.588. A baseline that always predicts the mean is the benchmark.

## Data

| Source | Notebook | Raw file |
|---|---|---|
| Price paid (property sales) | `price_paid_prep.ipynb` | `raw_data/prices_paid.csv` |
| UK House Price Index | `hpi_prep.ipynb` | `raw_data/uk_hpi.csv` |
| Income | `income_data_prep.ipynb` | `raw_data/income_data.csv` |
| Employment rate | `employment_data_prep.ipynb` | `raw_data/employment_data.csv` |
| Crime (street-level, 2013 to 2023) | `crime_data_prep.ipynb` | `raw_data/merseyside_csv_files/` (monthly files) |
| Flood risk areas | `flood_risk_areas_prep.ipynb` | shapefiles in `clean_data/` |

Cleaned outputs are in `clean_data/`, and `clean_data/final_df.csv` is the merged table used for modelling. Some large files are gitignored: `raw_data/prices_paid.csv`, `clean_data/merged_crime_data.csv`, the flood area files, and the trained model `*.pkl`. You will need to download the price paid data yourself and rerun the prep notebooks to recreate them.

## Layout

```
notebook.ipynb                 Main notebook: merge, explore, model, save the pipeline
*_prep.ipynb                   One data-preparation notebook per source
property_prices_app.py         Streamlit entry point (navigation between the two pages)
property_EDA_app.py            Page: data overview and filters, charts
property_prices_model_app.py   Page: enter a property's features, get a predicted price
                               and the top feature importances
raw_data/                      Source data
clean_data/                    Prepared data
```

## Running the app

Requires Python 3 with `streamlit`, `pandas`, `numpy`, `scikit-learn`, `xgboost`, `plotly`, `matplotlib` and `seaborn`.

1. Run `notebook.ipynb` to train the model and write the pipeline pickle.
2. Start the app:
   ```bash
   streamlit run property_prices_app.py
   ```

The apps load `clean_data/final_df.csv` and `final_model_pipeline.pkl` relative to the app files, so run them from a clone with the model already trained (step 1).
