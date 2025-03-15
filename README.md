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

✅ The operating system shows a slight negative impact on price, possibly due to budget models running Linux or ChromeOS

      



