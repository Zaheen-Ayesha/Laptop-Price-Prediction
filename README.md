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

The goal of EDA is to analyze the relationship between different laptop features and their impact on price. This involves:
- Regression analysis for continuous features.
- Box plots for categorical features.
- Correlation analysis.
- Pivot table visualization.

<b>Steps Executed in EDA</b>
1. Regression Analysis for Continuous Features
   - Generated regression plots to analyze the relationship between Price and:
      - CPU_frequency, Screen_Size_inch, Weight_Pounds
   - Used .corr() function to calculate the correlation of each feature with Price.

2. Box Plot Analysis for Categorical Features
   - Created box plots to compare Price distribution across:
     - Category, GPU, OS, CPU_core, RAM_GB, Storage_GB_SSD
   - These plots help identify trends, such as which categories tend to have higher prices.

3. Descriptive Statistical Analysis
   - Computed summary statistics including mean, median, standard deviation, and range for numerical attributes.
  
4. Pivot Table & Heatmap
   - Grouped GPU, CPU_core, and Price into a pivot table.
   - Used a pcolor plot to visualize how GPU and CPU_core interact with price.
      



