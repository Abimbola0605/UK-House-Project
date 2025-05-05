# UK-House-Project
This project explores and forecasts UK housing market trends using Exploratory Data Analysis (EDA), feature engineering, Predictive modeling and ARIMA time series modeling. The dataset covers housing price and volume statistics between February 2024 and January 2025.

## 📁 Dataset
The dataset was sourced from the UK House Price Index and includes:
- Average house prices across property types
- Sales volume
- Buyer type breakdown (cash, mortgage, first-time buyers, former owners)
- Price trends for new builds vs existing homes
- Monthly and yearly price changes

## 🛠️ Feature Engineering For EDA
To support deeper insights for EDA, several new features were created:
- Price per Sale:
   - Average_price / sales_volume
- Annualized Growth:
   - Based on the monthly percentage change: ((1 + monthly_change)^12 - 1) * 100
- Cash-to-Mortgage Ratio:
   - Cash_purchases / mortgage_purchases
- New Build Premium:
   - Average_price_new_build - average_price_existing_properties

 ## 📊 Exploratory Data Analysis (EDA)
 Using Plotly, Seaborn, and Matplotlib, we conducted a series of visual analyses to uncover trends, outliers, and relationships:
 -  Average House Price Over Time (All Buyers)
 -  Sales Volume Trends
 -  Distribution of Prices by Buyer Type
 -  Sales Volume vs Average Price
 -  New Build Premium Over Time
 -  Annualized Growth Over Time
 -  Cash-to-Mortgage Ratio Over Time
 -  Property Type Price Spread
 -  Price vs. Yearly Change Correlation
 -  New Build vs Existing Properties: Average Price Trend
 -  Time Series Decomposition of Average Price
 -  Monthly Percentage Change vs Sales Volume
 -  A strong correlation was found between monthly price change and sales volume: 0.707.

## 💡 Business Insights
1.  Prioritize High-Value, High-Growth Segments:
     - Detached Houses: Highest median price (~£429k) and strongest annual growth (6.0%). Ideal for premium, long-term investments.
     - New Builds: Command a premium of up to £110k, especially in Nov 2024. Show higher growth (12.7%) compared to existing homes (3.1%).
2. Timing the Market Is Critical:
     - Sales Peak in August (~70k transactions) — align marketing and listing efforts here.
     - July shows highest annualized price growth (20%), but it drops sharply by November (0%). Plan exits before volatility hits.
3. Watch for Seasonal & Volatility Risks:
     - Avoid November (lowest sales volume ~56k and 0% growth).
     - Use monthly price changes as an indicator — sales volume correlates strongly (0.707) with price increases over 1%.
4.  Consider Budget Buyer Segments:
     - Flats and Maisonettes offer affordable entry points (~£193k median) with low but stable growth (2.3%). Suitable for first-time buyer products or rental portfolios.
5. Avoid Overreliance on Imputed Data:
     - Buyer type pricing and cash/mortgage splits are imputed — may obscure real market patterns.
      Recommendation: collect more granular transaction-level data.
6. Diversify to Manage Risk:
     - Spread across property types to reduce exposure to sharp growth declines.
     - Focus on both high-growth months and stable segments like semi-detached houses (~£265k, consistent).


## ⚙️ Feature Engineering for Predictive Modeling
Engineered features to enhance model performance:
- Lag Features:
   - average_price_lag_1, sales_volume_lag_1, etc.
- Rolling Statistics:
   - 3-month rolling means: avg_price_roll3, sales_volume_roll3
- Trend Feature:
   - A simple sequential index: trend = np.arange(len(house))
- Time Features:
   - Extracted month, quarter, and year from pivotable_date for seasonal modeling
These features enabled models to better capture patterns and seasonality in price movements.

## 🤖 Predictive Modeling & Evaluation
We developed supervised machine learning models to predict average house prices using multiple features:
#### Train/Test Split

### Models Used
- Linear Regression 
- Random Forest Regressor
- XGBoost Regressor
- GradientBoostingRegressor

#### Evaluation Metrics
| Metric | Description |
| --- | --- |
| MAE | Mean Absolute Error |
| RMSE | Root Mean Squared Error |
| R² Score | Explained variance |


#### Results 
| Model | MAE | RMSE | R² Score |
| -- | -- | -- | -- | 
| Linear Regression | 2021.95| 2657.89 |  0.6223 |
| Random Forest Regressor | 817.78 | 1155.66 | 0.9286 |
| XGBoost Regressor | 1889.53 | 2423.59 | 0.6860 |
| GradientBoostingRegressor | 598.45 | 609.20 | 0.9802 |

Best Overall Performer: GradientBoostingRegressor
- Lowest MAE (598.45): Smallest average prediction error.
- Lowest RMSE (609.20): Lowest root mean squared error, indicating better handling of large errors.
- Highest R² Score (0.9802): Explains ~98% of the variance in the data, showing excellent generalization.
  
GradientBoostingRegressor delivered the best overall performance, showing robust accuracy and generalization across all key metrics.

### Time Series Forecasting (ARIMA)
To forecast average UK house prices, the ARIMA (AutoRegressive Integrated Moving Average) model was applied after ensuring stationarity and proper preprocessing.
#### Steps Taken:
1. Data Preparation:
   - Set pivotable_date as the time index.
   - Selected average_price_all_property_types as the target variable.
2. Transformation & Stationarity Checks:
   - Applied log transformation to stabilize variance.
   - Conducted ADF tests at each stage:
       - After log transform: p-value = 0.4529 → not stationary.
       - After first differencing: p-value = 0.4444 → still not stationary.
       - After second differencing: p-value = 0.0333 → achieved stationarity.
    
3. Modeling:
   - Fit ARIMA(1,1,0) model.
   - Model summary:
       - AIC = -65.408
       - AR(1) coefficient = 0.6284 (p = 0.001)
       - Ljung-Box p-value = 0.50 → no autocorrelation
       - JB p-value = 0.92 → residuals are normally distributed
    
4. Forecasting (Feb – May 2025):
   Forecasted average house prices (exponentiated from log):
   | Month | Forecast (£) |
   | -- | -- |
   | 2025-02-01 | 268,944.03 |
   | 2025-03-01 | 269,193.20 |
   | 2025-04-01 | 269,349.90 |
   | 2025-05-01 | 269,448.42 |

#### Key Insight:
Despite needing two levels of differencing, the ARIMA(1,1,0) model achieved reasonable forecasting accuracy, with stable upward trends forecasted for early 2025.

#### Technologies Used:
- Programming Language:
    - Python
- Data Manipulation & Analysis:
    - Pandas
    - NumPy
- Data Visualization:
    - Matplotlib
    - Seaborn
    - Plotly
- Statistical Modeling & Time Series Analysis:
    - Statsmodels
       - ARIMA
       - Seasonal Decomposition
- Machine Learning & Predictive Modeling:
    - Scikit-learn
       - Linear Regression
       - Random Forest Regressor
       - Gradient Boosting Regressor
    - XGBoost (XGBoost Regressor)
#### Future Work
- Incorporate regional analysis across the UK
- Include macroeconomic indicators (interest rates, inflation)
- Build an interactive dashboard using Streamlit or Dash

### 👤 Author
  Abimbola Asala
  
  MSc Data Science | BCS Member






