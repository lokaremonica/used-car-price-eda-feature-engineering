# 🚗 Used Car Price EDA & Feature Engineering

This repository contains a comprehensive **Exploratory Data Analysis (EDA)** and feature transformation pipeline on a dataset of used cars to understand key attributes affecting price, mileage, and performance. It includes outlier handling, unit conversions, missing value imputation, correlation analysis, and visualizations.

---

## 📊 Dataset Overview

The dataset (`used_cars_data.csv`) contains information on used cars including:

* **Categorical Features**: Name, Location, Fuel Type, Transmission, Owner Type
* **Numerical Features**: Year, Kilometers Driven, Mileage, Engine, Power, Seats, Price

---

## ✅ Steps Performed

### 1. Data Cleaning

* Dropped irrelevant columns: `S.No.`, `New_Price`
* Created new feature:
  **`Car_Age = current_year - Year`**
* Extracted `Brand` and `Model` from `Name`
* Normalized brand names (e.g., ISUZU → Isuzu)

---

### 2. Unit Conversion

Converted units in:

* **Mileage**

  * `km/kg → kmpl` using:
    `kmpl = km/kg ÷ 1.4`
  * `km/kWh → kmpl` using:
    `kmpl = km/kWh × 8.9`
* **Engine**: `998 cc → 998`
  → `int(Engine.replace(" cc", ""))`
* **Power**: `58.16 bhp → 58.16`
  → `float(Power.replace(" bhp", ""))`

---

### 3. Outlier Detection

Checked normality using:

* **Shapiro-Wilk Test**
* **D’Agostino’s K² Test**

Outliers were detected using the IQR method:

* **Formula**:
  `IQR = Q3 - Q1`
  `Lower Limit = Q1 - 1.5 × IQR`
  `Upper Limit = Q3 + 1.5 × IQR`

📌 Example:

```text
Power:
Q1 = 75, Q3 = 138.1, IQR = 63.1
Upper Limit = 232.75 → 273 outliers
```

---

### 4. Box Plots

* Created grouped and separate box plots for features by scale.
* Insights: Power > 220, Price > 20L, Mileage < 6.2 or > 30 are flagged as outliers.

---

### 5. Mileage vs Fuel Type

* Boxplot shows:

  * EVs have the highest median mileage
  * Petrol has the widest spread

---

### 6. Pearson Correlation

**Formula**:

$$
r = \frac{\sum (x_i - \bar{x})(y_i - \bar{y})}{\sqrt{\sum (x_i - \bar{x})^2 \sum (y_i - \bar{y})^2}}
$$

📈 Key Correlations:

* `Engine vs Power`: **+0.89** (Strong positive)
* `Power vs Price`: **+0.60**
* `Car_Age vs Price`: **-0.45**
* `Mileage vs Engine`: **-0.47**

---

### 7. Scatter Plots

* Drawn between all numeric features with Pearson coefficient in title.

---

### 8. Handling Missing Values

* Missing values treated:

  * Engine: 111
  * Power: 448
  * Seats: 54
* Outliers treated as `NaN` before imputation
* **Imputation method**:

  * Subclass-based: Brand + Model
  * For Seats: global mode
  * If subclass count < 2, fallback to Fuel\_Type-based imputation

---

## 📉 Results Summary

| Feature    | Outliers Detected |
| ---------- | ----------------- |
| Car\_Age   | 58                |
| Kilometers | 258               |
| Mileage    | 82                |
| Engine     | 65                |
| Power      | 273               |
| Seats      | 1153              |
| Price      | 718               |

| Feature Pair      | Pearson r |
| ----------------- | --------- |
| Engine vs Power   | 0.89      |
| Power vs Price    | 0.60      |
| Car\_Age vs Price | -0.45     |
| Mileage vs Power  | -0.48     |

---
### 🧾 Final Conclusion from the EDA Project

The exploratory data analysis on the used car dataset yielded several important insights that can guide further modeling, data cleaning, and business decisions:

---

### 🔍 Key Findings

1. **Data Quality Issues Identified**

   * `New_Price` and `S.No.` were dropped due to irrelevance or missing values.
   * Typos and inconsistencies in brand names (e.g., "ISUZU" vs "Isuzu", "Land" → "Land Rover") were corrected.
   * Several zero or invalid values were treated as missing (e.g., `Seats = 0`, negative `Price`).

2. **Derived Features**

   * Created `Car_Age` as a useful numerical feature for age-related depreciation trends.

3. **Unit Standardization**

   * All units (Mileage, Engine, Power) were standardized for consistency:

     * Mileage: Converted `km/kg` and `km/kWh` to `kmpl`.
     * Engine and Power: Cleaned to numerical values.

4. **Outlier Detection**

   * None of the numerical features followed a normal distribution (validated using both Shapiro-Wilk and D’Agostino’s tests).
   * Outliers were detected using IQR method; notably:

     * `Seats`: over 1,100 outliers due to rare or incorrect values.
     * `Power` and `Price`: had extreme values due to luxury or performance cars.

5. **Missing Value Imputation**

   * Missing and outlier values in `Engine`, `Power`, and `Seats` were imputed based on:

     * Subclass (Brand + Model)
     * Global mode (for categorical)
     * Fuel Type (for fallback imputation)

6. **Correlation Insights**

   * Strongest positive correlation:
     `Engine vs Power → r = 0.89`
     `Power vs Price → r = 0.60`
   * Negative correlations:

     * `Car Age vs Price → r = -0.45`
     * `Mileage vs Engine → r = -0.47`
   * `Seats` had no significant correlation with `Price`.

7. **Visualizations**

   * Box plots clearly illustrated the presence of outliers across multiple features.
   * Heatmaps and scatterplots reinforced correlation findings visually.

---

### 📌 Overall Conclusion

The EDA process successfully:

* Cleaned and standardized the data.
* Identified key patterns in car performance and pricing.
* Highlighted strong predictors of car price (like `Power`, `Engine`, `Car_Age`) for downstream modeling.
* Flagged critical outliers and missing data issues to be addressed in the machine learning pipeline.

This refined dataset is now ready for regression or classification models to **predict car prices** or **classify car segments**.
