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



