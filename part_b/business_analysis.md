## B1. Problem Formulation

### (a) Machine Learning Formulation

**Target Variable:**  
items_sold (number of items sold per store per month)

**Input Features:**
- Store attributes: store_size, location_type  
- Promotion details: promotion_type  
- Temporal features: month, is_weekend, is_festival  
- Market conditions: competition_density  

**Type of Problem:**  
This is a supervised regression problem — we're predicting items_sold, which is a continuous numerical value.

**Why Regression?**  
We need to estimate how many items will sell based on the features we have, and regression is the right tool for predicting numerical values like this.

---

### (b) Why Items Sold Instead of Revenue

Revenue numbers can be misleading because they are influenced by pricing strategies, discounts, and product mix — factors that do not directly measure promotion effectiveness.  

Items sold (sales volume) provides a clearer indication of how customers respond to promotions.

**The Bigger Picture:**
- The target variable should align with the business objective  
- Choosing the right target ensures the model produces meaningful and actionable results  

---

### (c) Alternative Modeling Strategy

Instead of building one global model for all stores, we can create:

- Separate models for each location_type (urban, semi-urban, rural)

**Why This Makes Sense:**
- Different store types respond differently to promotions  
- Segmented models capture location-specific patterns  
- This leads to better predictive performance  

---

## B2. Data and EDA Strategy

### (a) Data Integration

To combine the four tables (transactions, store attributes, promotions, and calendar), we would join them using:

- store_id → for store-level information  
- transaction_date → for time-based alignment  

**Final Dataset Grain:**
- One row per store per day (or per transaction depending on aggregation)

**Key Aggregations:**
- Total items_sold per store per day/month  
- Average basket size  
- Promotion indicators  

---

### (b) Exploratory Data Analysis

**1. Sales Distribution Plot**
- Shows how items_sold is distributed  
- Helps identify skewness and outliers  

**2. Promotion-wise Sales Comparison**
- Compares average sales across promotion types  
- Identifies which promotions perform better  

**3. Time Series Plot**
- Tracks sales over time  
- Reveals trends and seasonality  

**4. Correlation Heatmap**
- Shows relationships between features and items_sold  
- Helps in feature selection  

---

### (c) Handling Imbalance

- Around 80% of transactions have no promotion  
- This may bias the model toward predicting "no promotion"

**Solutions:**
- Use balanced sampling  
- Add a promotion indicator feature  
- Evaluate performance separately for promotion and non-promotion cases  

---

## B3. Model Evaluation and Deployment

### (a) Train-Test Split and Metrics

**Approach:**
- Use a temporal split:
  - Train → earlier data (e.g., first 2.5 years)  
  - Test → most recent data (e.g., last 6 months)  

**Why not random split?**
- Random split mixes past and future data  
- Causes data leakage and unrealistic evaluation  

**Evaluation Metrics:**
- RMSE → measures overall error and penalizes large mistakes  
- MAE → measures average error and is easier to interpret  

---

### (b) Explaining Model Decisions

- Use feature importance to understand model behavior  

**Example:**
- December → Loyalty Points may perform better (holiday season)  
- March → Discounts may perform better (end-of-season sales)  

**Why this matters:**
- Explains different promotion recommendations for the same store  
- Builds trust in the model  

---

### (c) Deployment Strategy

**Model Saving:**
- Save the trained model using joblib or pickle  

**Monthly Workflow:**
1. Collect new data  
2. Apply the same preprocessing pipeline  
3. Generate predictions for the next month  

**Monitoring:**
- Track RMSE and MAE over time  
- Detect performance degradation  

**Retraining:**
- Retrain periodically  
- Retrain immediately if performance drops significantly  
