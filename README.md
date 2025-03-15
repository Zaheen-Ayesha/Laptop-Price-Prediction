# Laptop Price Prediction using Python

<b>Project Objective :</b>The goal of this project is to predict laptop prices based on various features like manufacturer, category, screen type, GPU, OS, CPU core, screen size, RAM, storage, and weight.

<b>Dataset :</b>The dataset contains 238 records and 12 columns, with attributes like Manufacturer, Category, Screen, GPU, OS, CPU_core, Screen_Size_cm, CPU_frequency, RAM_GB, Storage_GB_SSD, Weight_kg, and Price.

## Data Wrangling
<b>1. Loading Data:</b>
- Imported the dataset using pandas and examined the first few rows using .head().
- Assigned meaningful column headers for better readability.

<b>2. Handling Missing Values:</b>
- Identified null values in each column.
- Replaced missing values:
  
    - <b>Continuous variables:</b> Filled with the column mean.
    - <b>Categorical variables:</b> Replaced with the most frequently occurring value.

 <b>3. Data Standardization:</b>
 
 - <b>Converted Screen Size from cm to inches:</b>
 
       df["Screen_Size_cm"] = df["Screen_Size_cm"].astype("float")
       df["Screen_Size_inch"] = df["Screen_Size_cm"] * 2.54
       df.drop(columns=["Screen_Size_cm"], inplace=True)
   
- <b>Converted Weight from kg to pounds:</b>

      df["Weight_Pounds"] = df["Weight_kg"] * 2.205
      df.drop(columns=["Weight_kg"], inplace=True)

<b>4. Data Normalization:</b>
- Normalized CPU_frequency to scale it relative to the maximum value in the dataset.

      df["CPU_frequency"]=df["CPU_frequency"]/df["CPU_frequency"].max()

<b>5. Data Binning:</b>
- Created 3 price bins (Low, Medium, High) for better classification.

      bins=np.linspace(min(df["Price"]),max(df["Price"]),4)
      group_names=["Low","Medium","High"]
      df["Price-binned"]=pd.cut(df["Price"],bins=bins,labels=group_names,include_lowest=True)
      df[['Price','Price-binned']]

<b>6. Feature Engineering:</b>
- Converted categorical "Screen" attribute into two indicator variables:
     - Screen-IPS_panel
     - Screen-Full_HD
- Dropped the original Screen column after transformation.

## Exploratory Data Analysis
<b>Objective</b>

The goal of EDA is to analyze the relationship between different laptop features and their impact on price. 

### Steps Executed in EDA</b>

1.<b> Regression Analysis for Continuous Features</b>
   - Generated regression plots to analyze the relationship between Price and:
      - CPU_frequency, Screen_Size_inch, Weight_Pounds
   - Used .corr() function to calculate the correlation of each feature with Price.

2.<b> Box Plot Analysis for Categorical Features</b>
   - Created box plots to compare Price distribution across:
     - Category, GPU, OS, CPU_core, RAM_GB, Storage_GB_SSD
   - These plots help identify trends, such as which categories tend to have higher prices.

3.<b> Descriptive Statistical Analysis</b>
   - Computed summary statistics including mean, median, standard deviation, and range for numerical attributes.
  
4. Pivot Table & Heatmap
   - Grouped GPU, CPU_core, and Price into a pivot table.
   - Used a pcolor plot to visualize how GPU and CPU_core interact with price.

### Executive Summary of EDA

- <b>CPU Frequency vs. Price:</b> Higher CPU frequency is positively correlated with price, meaning laptops with higher processing power tend to be more expensive.
  
- <b>Screen Size vs. Price:</b> A weak correlation was observed, indicating that screen size alone does not significantly determine price.
  
- <b>Weight vs. Price:</b> Lighter laptops tend to be pricier, possibly due to premium ultrabook models.
  
- <b>RAM & Storage Impact:</b> Laptops with higher RAM and SSD storage generally have higher prices.
  
- <b>Category & GPU Influence:</b> Gaming and high-performance laptops (with powerful GPUs) tend to have a significantly higher price range.
  
- <b>Operating System Effect:</b> Laptops with macOS or Windows Pro are priced higher than Linux or FreeDOS-based laptops.
  

## Pearson Correlation Coefficient & P-Value Analysis

To further quantify the relationship between different attributes and laptop price, we computed the Pearson Correlation Coefficient and P-Value for numerical and categorical features.

      from scipy import stats
      for param in ['RAM_GB','CPU_frequency','Storage_GB_SSD','Screen_Size_inch','Weight_Pounds','CPU_core','OS','GPU','Category']:
          pearson_coef, p_value = stats.pearsonr(df[param], df['Price'])
          print(param)
          print("The Pearson Correlation Coefficient for ",param," is", pearson_coef, " with a P-value of P =", p_value)

### Conclusion

Analysis confirms that: 

✅ RAM and CPU specifications are the most important factors influencing laptop price.

✅ GPU, storage, and laptop category also play a role, but to a lesser extent.

✅ Screen size and weight do not significantly affect pricing.

✅ The operating system shows a slight negative impact on price, possibly due to budget models running Linux or ChromeOS.

## Model Development

Model development was performed to create a predictive model for estimating laptop prices based on various features. This helps understand which attributes significantly impact pricing and enables accurate forecasting for unseen data.

1.<b> Simple Linear Regression (SLR)</b>

- A single-variable regression model was built using CPU_frequency as the independent variable.
  
- The model was trained to fit the data and predict Price.
  
- <b>Evaluation Metrics:</b>

   - Mean Squared Error (MSE)
   - R-Squared (R²) Score
     
  <b>Key Observations:</b>

   - The actual price distribution is more spread out, while the predicted price distribution is overly concentrated in certain price ranges.
     
   - This suggests the model fails to capture price variations, particularly at the high and low ends.
     
   - The Mean Squared Error (MSE) is high (284583.44), indicating large discrepancies between actual and predicted values.
     
   - The R-squared (R²) Score is very low (0.134), meaning the model explains only 13.4% of price variation, leaving 86.6% unexplained.

  <b>Conclusion:</b>
The model in its current state does not generalize well. Improving feature selection, transformations, and experimenting with more complex models could enhance predictive accuracy.

2.<b> Multiple Linear Regression (MLR)</b>

- The model was extended to include multiple features:
  - CPU_frequency, RAM_GB, Storage_GB_SSD, CPU_core, OS, GPU, and Category
    
- This allowed the model to capture a more complex relationship between features and price.

  <b>Key Observations</b>

- The actual price distribution (red line) is wider, while the predicted price distribution (blue line) is more concentrated, indicating that the model struggles with price variations, especially for high-end laptops.
  
- The model tends to underestimate higher-priced laptops, as seen in the mismatch at the upper end of the price range.
  
- The Mean Squared Error (MSE) has improved to 161,680.57 (from 284,583.44), meaning the model makes smaller errors on average.
  
- The R-Squared Score (R²) increased to 0.508, meaning the model now explains 50.8% of price variation (previously only 13.4%), but 49.2% of variations remain unexplained.

  <b>Conclusion:</b>
The model has improved significantly but still lacks accuracy in predicting high-end laptop prices. Further optimization with advanced models and better feature selection can enhance its predictive power.

3. <b>Polynomial Regression</b>

The Polynomial Regression model was tested at different degrees (1st, 3rd, and 5th) to analyze how well it captures price variations based on CPU frequency.

<b>Key Findings:</b>

✅ 1st Degree Polynomial (Linear Regression)

R² = 0.1344 (13.44%) → Poor fit, capturing only a small portion of price variation.
MSE = 284,583.44 → High prediction error.
Conclusion: A simple linear relationship is insufficient for accurate predictions.

✅ 3rd Degree Polynomial (Cubic Model)

R² = 0.2669 (26.69%) → Better fit, capturing more variation.
MSE = 241,024.86 → Lower error than the linear model.
Conclusion: Captures non-linear price trends better, improving accuracy.

✅ 5th Degree Polynomial (Quintic Model)

R² = 0.3080 (30.80%) → Best fit among tested models.
MSE = 229,137.29 → Further reduced error, but only a small improvement from the 3rd-degree model.
Conclusion: The model captures more complexity, but may risk overfitting, leading to poor generalization on new data.

<b>Final Insights</b>
- Higher-degree polynomials improve performance, but the gains diminish beyond a certain point.
- The R² values remain low (~30%), meaning other important features influencing price are missing.
- A more balanced model (e.g., 3rd-degree polynomial or alternative algorithms like Random Forest/XGBoost) may offer better performance without overfitting.

4. <b>Pipeline</b>
A machine learning pipeline was implemented to enhance model performance by scaling parameters, generating polynomial features, and applying Linear Regression using multiple input variables.

<b>Key Findings:</b>

✅ Best Model So Far – The multi-variable polynomial model achieved the lowest Mean Squared Error (MSE = 223,437.70) and highest R² (32.04%), making it the most accurate model tested.

✅ More Features Improved Accuracy – Incorporating multiple features (CPU frequency, RAM, Storage, GPU, OS, Category, etc.) improved the model’s ability to explain price variation compared to single-variable models.

⚠️ Still Room for Improvement – While R² = 32.04% is an improvement, 68% of price variation remains unexplained, suggesting other influential factors are missing.

## Model Evaluation and Refinement
To enhance model performance and prevent overfitting, multiple validation techniques were applied, including training/testing split, cross-validation, Ridge Regression, and Grid Search. The objective was to evaluate model generalization and identify the best-performing approach for predicting laptop prices.

<b>Key Findings:</b>

✅ Training vs. Testing Performance (Linear Regression with CPU Frequency)

- Training R² = 0.148 (14.8%) → CPU frequency alone explains very little variance in training data.
- Testing R² = -0.066 (-6.6%) → A negative score indicates the model performs worse than a simple mean prediction, failing to generalize.

⚠️ Cross-Validation Results (4-Fold Validation on CPU Frequency)

- Mean R² = -0.161 → Confirms poor predictive power, as the model underperforms across all validation sets.
- Std Dev = 0.385 → High variance in results suggests the model is unstable and possibly overfitting.

### Identifying Overfitting in Laptop Price Prediction
To evaluate the model’s generalization ability, the dataset was split into 50% training and 50% testing, and R² scores were analyzed across different polynomial degrees. The goal was to determine the optimal complexity level before the model starts overfitting.

<b>Key Findings:</b>
<b>📉 Overfitting Observed at Higher Polynomial Degrees (4 & 5).</b>

- Drastic drop in R² values (negative scores worse than -1) on test data, indicating that the model memorizes training data but fails on unseen data.
- Higher-degree polynomials (4 & 5) learn noise instead of real patterns, making the model unreliable for predictions.
  
<b>✅ Optimal Model Complexity</b>

- Linear (degree = 1) and cubic (degree = 3) regression perform better, balancing bias and variance.
- Higher-degree models add complexity but do not improve predictive accuracy.

<b>Conclusion</b>

Overfitting occurs when model complexity exceeds optimal levels. A simpler cubic regression (degree = 3) or regularized model is recommended for better generalization and reliability in predicting laptop prices.

##  Ridge Regression
To improve model generalization and prevent overfitting, a Ridge Regression model was applied using polynomial features (degree = 2) with multiple predictors:
- CPU Frequency, RAM, Storage (SSD), CPU Cores, OS, GPU, and Category.

<b>Key Findings:</b>

✅ Regularization Reduced Overfitting:
- Training R² (~0.7) remained stable, showing a strong fit to the training data.
- Validation R² (~0.4) improved with increasing alpha, meaning regularization helped prevent overfitting.

✅ Effect of Alpha (Regularization Strength):
- Higher alpha values constrained large coefficients, improving generalization on test data.
- Small alpha values led to higher variance, allowing overfitting.

✅ Why Regularization Matters?
- Controls large coefficient values, reducing sensitivity to noise.
- Helps when dealing with many correlated features (e.g., RAM & Storage).
- Improves predictive accuracy on unseen data, making the model more reliable.

<b>Conclusion & Next Steps:</b>
- Optimal Ridge Model: Moderate alpha values (~0.1 to 0.5) give the best trade-off between bias and variance.
- Further Improvements: Try Lasso Regression to perform feature selection and enhance interpretability.
- Alternative Models: Random Forest or XGBoost may capture non-linear relationships better than polynomial Ridge Regression.

Regularization significantly improves model generalization by preventing overfitting. A balanced Ridge Regression model is a stronger predictor of laptop prices than high-degree polynomial models alone.

## Grid Search Optimization for Ridge Regression
To further enhance model performance, GridSearchCV was used to identify the optimal alpha value for Ridge Regression.

<b>Key Findings:</b>

✅ Optimal Alpha Selection:
- A grid search was conducted over a set of alpha values: {0.0001, 0.001, 0.01, 0.1, 1, 10}.
- The best-performing model was selected based on 4-fold cross-validation.

✅ Model Performance After Optimization:
- The best Ridge Regression model achieved an R² score of 0.399 on the test set.
- This is a significant improvement compared to previous models, showing better price variation explanation.

✅ Why This Matters?
- Grid search helps find the best hyperparameters, improving model generalization.
- Ridge Regression with an optimal alpha prevents overfitting while maintaining prediction accuracy.

Using GridSearchCV, we optimized Ridge Regression to explain 39.9% of the price variation, making it the best-performing model so far. 

### Final Conclusion: Laptop Price Prediction Project
This project aimed to develop a predictive model for laptop prices using various machine learning techniques and feature engineering methods. Through systematic experimentation and model optimization, we explored different regression techniques and evaluated their effectiveness.

✅ Feature Engineering & Initial Regression Models:
- Polynomial Regression showed better performance than Linear Regression, with higher-degree polynomials capturing more complexity.
- However, excessive polynomial degrees led to overfitting, reducing model reliability.

✅ Multi-Variable Polynomial Regression & Pipeline Optimization:
- Incorporating multiple features (CPU frequency, RAM, storage, etc.) improved model accuracy.
- A Pipeline for feature scaling, transformation, and regression helped streamline the workflow.

✅ Cross-Validation & Overfitting Analysis:
- Cross-validation confirmed that CPU frequency alone was a weak predictor.
- Overfitting was detected in higher-degree polynomial models, reinforcing the need for regularization techniques.

✅ Regularization with Ridge Regression & Hyperparameter Tuning:
- Ridge Regression prevented overfitting by controlling large coefficients.
- GridSearchCV was used to identify the optimal alpha value, improving the model's ability to generalize.

✅ Best Model Performance:
- The final Ridge Regression model with an optimized alpha value achieved an R² score of 0.399, explaining 39.9% of the variation in laptop prices.

🔹<b> Next Steps for Enhancement:</b>
- Incorporate additional features (brand, display size, GPU type, battery life) for better price prediction.
- Explore ensemble models like Random Forest or XGBoost for improved accuracy.
- Experiment with Lasso Regression for feature selection to reduce complexity.



