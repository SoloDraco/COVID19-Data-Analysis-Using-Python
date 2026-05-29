# COVID-19 & Global Happiness Data Analysis

This project explores the relationship between the spread of COVID-19 and the socioeconomic factors that determine national happiness. By aggregating global pandemic metrics with institutional scores, the analysis attempts to identify if citizens living in more developed, stable countries experienced different infection trajectories.

---

## 📌 Project Overview
The objective of this data analysis project is to find out whether there is any correlation between a country's macroeconomic/social metrics and the speed at which COVID-19 spread within its borders. 

* **The Core Question:** Do higher life expectancy, better social support, and robust GDP per capita correlate with lower or higher maximum infection rates?
* **Approach:** We calculate a custom proxy variable—**Maximum Infection Rate**—for each country from time-series pandemic data, and join it with institutional indices extracted from the World Happiness Report.

---

## 📊 Data Sources & Description
The analysis integrates two distinct datasets:
1. **COVID-19 Confirmed Dataset (`covid19_Confirmed_dataset.csv`):** * **Source:** Johns Hopkins University / Cumulative daily tracking.
   * **Details:** Contains cumulative confirmed cases on a daily timeline spanning early 2020 for 266 observations across global states and countries.
2. **Worldwide Happiness Report (`worldwide_happiness_report.csv`):**
   * **Source:** United Nations Sustainable Development Solutions Network.
   * **Details:** Measures 156 countries across socioeconomic dimensions like GDP per Capita, Social Support, Healthy Life Expectancy, and Freedom to Make Life Choices.

---

## 🛠️ Tech Stack & Tools
* **Programming Language:** Python 3.x
* **Data Manipulation:** `pandas`, `numpy`
* **Data Visualization:** `seaborn`, `matplotlib.pyplot`

---

## 🔄 Data Analysis & Engineering Process

### 1. Data Cleaning & Preprocessing
* **Feature Selection:** Dropped geographical coordinate indicators (`Lat`, `Long`) from the COVID-19 tracking dataset, as they were unnecessary for country-level aggregation.
* **Redundant Dimension Reduction:** Removed columns (`Overall rank`, `Score`, `Generosity`, `Perceptions of corruption`) from the Happiness dataset to narrow the focus to high-impact socioeconomic variables.
* **Index Alignment:** Adjusted dataframe schemas to utilize `Country/Region` as the standard index across both sources to guarantee flawless data merging.

### 2. Feature Engineering (Calculating the Proxy Metric)
To establish a concrete baseline for virus spread intensity per country, the dataset was transformed:
* Chronological rows were aggregated globally by grouping tracking data by country name.
* Derived the daily first derivative (daily new cases) using `.diff()`.
* Extracted the absolute maximum value (`.max()`) from this derivative to define a country's unique **Maximum Infection Rate** (e.g., China: 15,136.0, Italy: 6,557.0, Spain: 9,630.0).

### 3. Dataset Integration
An `inner` join merged the custom calculated COVID-19 metric frame with the processed Worldwide Happiness Report matrix, resulting in a cohesive correlation-ready dataset.

---

## 📈 Key Insights & Results

### Correlation Matrix Outcomes
Running a statistical Pearson correlation analysis over the merged features revealed surprising positive relationships:

| Metric | Correlation with Maximum Infection Rate |
| :--- | :---: |
| **Healthy Life Expectancy** | **0.289263** |
| **GDP per capita** | **0.250118** |
| **Social Support** | **0.191958** |
| **Freedom to Make Life Choices** | **0.078196** |

### Visual Discoveries (Logarithmic Regression Adjustments)
Because the raw infection rates scaled exponentially, a logarithmic transform (`np.log(y)`) was applied during visualization using Seaborn scatterplots and regression plots (`sns.regplot`):

* **Socioeconomic Vulnerability:** Countries boasting higher **GDP per capita** and stronger **Social support** systems exhibited an upward trend in maximum infection rates.
* **The Longevity Paradox:** There is a clear positive linear relationship when plotting **Healthy Life Expectancy vs. Maximum Infection Rate**. 
* **Conclusion Summary:** Developed nations likely experienced faster documented spreads due to extensive global travel hubs, highly urbanized populations, and significantly more aggressive testing/reporting infrastructure compared to lower-ranking nations.
