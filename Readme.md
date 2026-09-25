# Bengaluru Housing Market EDA

## 1. Problem Statement
To perform an exploratory data analysis on real estate listings in Bengaluru to uncover key pricing trends, handle inconsistent data entries, and identify the primary factors that influence property values in the city.

## 2. Dataset
- **Source:** Bengaluru House Price Data (Kaggle) — 13,320 listings, 9 raw columns
- **Note:** the dataset isn't included in this repo due to size/licensing; download it from Kaggle and place it at `data/Bengaluru_House_Data.csv` (see "How to Run Locally" below).

## 3. Key Approach
- **Data Cleaning:** Dropped columns with excessive missing data (`society`, `balcony`, `availability`, `area_type`), removed remaining nulls, and standardized the `size` column (e.g. "4 Bedroom", "4 BHK") into a single numeric `bhk` field.
- **Feature Engineering:** Converted messy `total_sqft` entries (ranges like "2100 - 2850", units like "34.46Sq. Meter") into a single numeric value, then derived `price_per_sqft`.
- **Outlier Removal:** Applied three passes of domain-driven and statistical filtering — removing listings with unrealistically low sqft-per-bedroom (<300 sqft/BHK), removing price-per-sqft outliers within each location using mean ± standard deviation, and removing listings where bathroom count exceeded bedroom count + 2.
- **Dimensionality Reduction:** Collapsed 1,287 unique locations, one-hot encoding the most common ones as model features.
- **Modeling:** Compared Linear Regression, Lasso, and Decision Tree using `GridSearchCV` with 5-fold cross-validation, then built a `predict_price()` function on the best-performing model.

## 4. Key Insights & Results
- Cleaning and outlier removal reduced the dataset from 13,320 to 9,176 usable listings (a ~31% reduction), removing entries that were data-entry errors or statistically extreme rather than genuine market variation.
- **Linear Regression was the best-performing model**, with a 5-fold cross-validation score of **~79.3%**, ahead of Lasso (~63.5%) and a Decision Tree (~51.6%).
- Individual cross-validation folds ranged from 76.2% to 84.4%, and the model's held-out test score was ~77.2%.
- Location and total square footage were the dominant price drivers, consistent with the one-hot encoded location features carrying the most weight in the final model.

## 5. Tech Stack
Python, Pandas, NumPy, Scikit-learn (Linear Regression, Lasso, Decision Tree, GridSearchCV)

## 6. How to Run Locally
```bash
# Clone the repository
git clone https://github.com/Hemanthm5678/bengaluru-housing-eda.git
cd bengaluru-housing-eda

# Add the dataset
mkdir data
# Download "Bengaluru House Price Data" from Kaggle and save it as:
# data/Bengaluru_House_Data.csv

# Set up a virtual environment and install dependencies
python -m venv venv
source venv/bin/activate  # On Windows use `venv\Scripts\activate`
pip install pandas numpy matplotlib seaborn scikit-learn jupyter

# Launch the notebook
jupyter notebook notebooks/01_data_cleaning.ipynb
```