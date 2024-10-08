# World Happiness Report Data Analysis (2019)

# World Happiness Report Data Analysis (2019)

## Business Task

### Statement
The objective of this analysis is to examine the World Happiness Report data for the year 2019. The goal is to derive insights and trends related to global happiness levels and identify the key factors that influence them across different countries. This analysis will leverage tools such as Google Cloud, BigQuery, R, and Tableau.

## Data Sources

### Dataset
- **Name:** World Happiness Report 2019
- **Table ID:** radiant-snow-431503-g4.World_Happiness_Report.happiness_2019

### Description
The dataset encompasses various indicators of happiness across different countries. These indicators include:
- **GDP per capita**
- **Social support**
- **Life expectancy**
- **Freedom to make life choices**
- **Generosity**
- **Perceptions of corruption**

These metrics provide a comprehensive overview of the factors contributing to happiness levels worldwide.

## Data Cleaning

### Process
The data cleaning process involves:
1. **Data Inspection:** Reviewing the dataset for missing values, inconsistencies, or errors using BigQuery.
2. **Handling Missing Values:** Deciding on the treatment of missing data, whether through imputation, deletion, or other methods, potentially using R for more advanced handling techniques.
3. **Standardization:** Ensuring that all data fields are in a consistent format, including numerical values and categorical labels.
4. **Outlier Detection:** Identifying and managing any outliers that could skew the analysis.
5. **Data Integration:** Combining related data fields or sources if needed to enrich the dataset.
6. **Final Review:** Verifying that the cleaned dataset is ready for analysis, with all relevant fields accurately prepared.

This cleaned data will serve as the foundation for subsequent analysis and visualization tasks aimed at understanding global happiness trends and influencing factors.




Data Cleaning:

Process:

Load Data:
``` SQL
SELECT *
FROM `radiant-snow-431503-g4.World_Happiness_Report.happiness_2019`;

```

list all columns 

``` SQL
SELECT
    column_name
FROM
    `radiant-snow-431503-g4.World_Happiness_Report.INFORMATION_SCHEMA.COLUMNS`
WHERE
    table_name = 'happiness_2019';
```

Check for Missing Values:
``` SQL
SELECT
    COUNTIF(`Overall rank` IS NULL) AS missing_overall_rank,
    COUNTIF(`Country or region` IS NULL) AS missing_country_or_region,
    COUNTIF(`Score` IS NULL) AS missing_score,
    COUNTIF(`GDP per capita` IS NULL) AS missing_gdp_per_capita,
    COUNTIF(`Social support` IS NULL) AS missing_social_support,
    COUNTIF(`Healthy life expectancy` IS NULL) AS missing_healthy_life_expectancy,
    COUNTIF(`Freedom to make life choices` IS NULL) AS missing_freedom_to_make_life_choices,
    COUNTIF(`Generosity` IS NULL) AS missing_generosity,
    COUNTIF(`Perceptions of corruption` IS NULL) AS missing_perceptions_of_corruption
FROM `radiant-snow-431503-g4.World_Happiness_Report.happiness_2019`;
```

Handle Missing Values:

- Drop rows with missing values if the number is insignificant.
- Use mean/median imputation for numerical columns.
- Use mode imputation for categorical columns.

Check for Duplicates:

``` SQL
SELECT
    `Country or region`, COUNT(*)
FROM `radiant-snow-431503-g4.World_Happiness_Report.happiness_2019`
GROUP BY `Country or region`
HAVING COUNT(*) > 1;
```
Remove Duplicates:

``` SQL
DELETE FROM `radiant-snow-431503-g4.World_Happiness_Report.happiness_2019`
WHERE row_number NOT IN (
    SELECT MIN(row_number)
    FROM (
        SELECT *,
            ROW_NUMBER() OVER (PARTITION BY Country, Region ORDER BY Country) AS row_number
        FROM `radiant-snow-431503-g4.World_Happiness_Report.happiness_2019`
    )
);
```
Analysis:

Summary:

Descriptive Statistics:

``` SQL

SELECT
    AVG(Score) AS avg_happiness_score,
    MIN(Score) AS min_happiness_score,
    MAX(Score) AS max_happiness_score,
    STDDEV(Score) AS stddev_happiness_score
FROM `radiant-snow-431503-g4.World_Happiness_Report.happiness_2019`;
```

Correlation Analysis:

``` SQL
SELECT
    CORR(Score, `GDP per capita`) AS corr_gdp_happiness,
    CORR(Score, `Social support`) AS corr_social_support_happiness,
    CORR(Score, `Healthy life expectancy`) AS corr_life_expectancy_happiness,
    CORR(Score, `Freedom to make life choices`) AS corr_freedom_happiness,
    CORR(Score, Generosity) AS corr_generosity_happiness,
    CORR(Score, `Perceptions of corruption`) AS corr_corruption_happiness
FROM `radiant-snow-431503-g4.World_Happiness_Report.happiness_2019`;
```

Regional Analysis:

``` SQL
SELECT
    `Country or region` AS Region,
    AVG(Score) AS avg_happiness_score,
    COUNT(*) AS num_countries
FROM `radiant-snow-431503-g4.World_Happiness_Report.happiness_2019`
GROUP BY `Country or region`
ORDER BY avg_happiness_score DESC;
```
## Visualization

The following visualizations will be created using Tableau:

### Happiness Score Distribution
- **Histogram:** Displaying the distribution of happiness scores across countries to understand the overall spread and identify any common ranges.

### Happiness Score by Region
- **Bar Chart:** Showing the average happiness scores by region, which will highlight the geographical differences in happiness levels.

### Factors Influencing Happiness
- **Scatter Plots:** Displaying correlations between the happiness score and each factor (GDP per capita, social support, etc.) to identify the most significant contributors to happiness.

### Global Distribution of Happiness Scores
- **Map Visualization:** This map shows the geographic distribution of happiness scores by country or region.  

### Bar Chart by Region
- This bar chart visualizes the average happiness scores across different countries and regions.

### Box Plot
- The box plot visualizes the distribution of happiness scores within each region.

### Happiness Score Distribution

![Happiness Score Distribution (Histogram) (4)](https://github.com/user-attachments/assets/3dc7ec58-e9c7-454b-a32e-dc65a54f3509)

1. ### Happiness Score Distribution (Histogram)
- **Purpose:** This histogram visualizes the distribution of happiness scores across various countries, providing insight into the global spread of happiness levels.

- **Observations:**

- The majority of countries fall within the happiness score range of **4.5** to **6.5**.
- The highest concentration is in the **6.0** score range, with **174** countries falling within this range.
- Very few countries have scores below **3.5** or above **7.5**, indicating that extreme levels of happiness or unhappiness are rare.
- The color gradient (red to green) indicates that higher happiness scores are associated with a greater number of countries, suggesting that a moderately high level of happiness is common worldwide.


### Happiness Score by Region

![Happiness Score by Region (Bar Chart) (3)](https://github.com/user-attachments/assets/bf6f536c-0c9f-45c9-afd9-b34fd1f7d235)

2. ### Happiness Score by Region (Bar Chart)
- **Purpose:** This bar chart ranks countries by their average happiness scores in 2019, highlighting regional differences in happiness.

- **Observations:**

- **Finland** has the highest average happiness score at **7.769**, followed closely by other Nordic countries like **Denmark** and **Norway**. This suggests a high level of overall happiness in these regions.
- The lowest scores are observed in countries like **South Sudan** and the **Central African Republic**, with scores around **2.853**, indicating significant challenges in these regions.
- There is a clear gradient in happiness scores, with countries in Europe and the Americas generally scoring higher, while those in Africa and parts of Asia score lower.
- The color gradient from green to red visually emphasizes the disparity in happiness levels across different regions.

### Factors Influencing Happiness

![Factors Influencing Happiness (Scatter Plots) (2)](https://github.com/user-attachments/assets/c92e9329-5f85-4aef-b14e-ea2f984b3997)

3. ### Factors Influencing Happiness (Scatter Plots)
- **Purpose:** These scatter plots analyze the correlation between GDP per capita and happiness scores, aiming to explore the relationship between economic prosperity and happiness.

- **Observations:**

- There is a **positive correlation** between GDP per capita and happiness score, with countries that have higher GDP per capita generally scoring higher in happiness. This is evident from the upward trend in the scatter plot.
- Countries with high GDP per capita like **Luxembourg**, **Norway**, and **Switzerland** are clustered towards the top-right of the plot, indicating that economic wealth is a significant factor in achieving higher happiness scores.
- Conversely, countries with lower GDP per capita, such as **Afghanistan** and **South Sudan**, are found in the lower-left corner, suggesting that lower economic prosperity is associated with lower happiness.
- The scatter plot also reveals outliers, where some countries with moderate GDP per capita achieve relatively high happiness scores, indicating that other factors beyond economic wealth contribute to happiness.

### Global Distribution of Happiness Scores

![Global Distribution of Happiness Scores](https://github.com/user-attachments/assets/f477d443-abc5-4279-ae7d-c3771c18db68)

4. ### Global Distribution of Happiness Scores (Map Visualization)
**Description:**

- This map shows the geographic distribution of happiness scores by country or region. Each country is represented by a colored circle, with the color indicating the happiness score, ranging from **2.853** to **7.769**.
- Countries are categorized by their happiness scores, and a legend is provided to match the colors with the score ranges.
**Key Insights:**

- **High Happiness Scores:** Countries in Northern and Western Europe (e.g., Finland, Norway, Denmark) generally have high happiness scores, indicating a strong sense of well-being.
- **Moderate Happiness Scores:** Countries in the Americas and parts of Asia (e.g., the United States, Canada, Japan) display moderate happiness scores.
- **Low Happiness Scores:** Some countries in Africa and the Middle East have lower happiness scores, reflecting challenges in well-being.
**Observations:**

- The map effectively illustrates the global variation in happiness, showing a concentration of high scores in Europe and lower scores in certain regions of Africa and the Middle East.

  ### Bar Chart by Region

  ![Bar Chart by Region](https://github.com/user-attachments/assets/34bd5bc9-4e63-45d8-9f98-49e1134d0d17)

  5. ### Bar Chart by Region
**Description:**

- This bar chart visualizes the average happiness scores across different countries and regions. Each bar represents a country, with the length of the bar corresponding to the happiness score.
- The countries are arranged in ascending order of happiness scores.
**Key Insights:**

- **Highest Happiness Scores:** **Finland (7.769)**, **Denmark (7.600)**, and **Norway (7.554)** are among the top-ranked countries with the highest happiness scores.
- **Lowest Happiness Scores:** Countries like **South Sudan (2.853)**, **Central African Republic (3.083)**, and **Afghanistan (3.203)** have the lowest happiness scores.
- **Regional Averages:** The chart also shows regional trends, with Northern European countries generally scoring higher compared to other regions.
**Observations:**

- The bar chart effectively highlights the disparity in happiness scores between different countries and regions. It allows for an easy comparison of happiness levels across the globe.


### Box Plot

![Box Plot (1)](https://github.com/user-attachments/assets/0529b4d2-8e8b-48c2-a9e9-d8bf9f664ed1)

6. ### Box Plot
**Description:**

- The box plot visualizes the distribution of happiness scores within each region. Each box represents the interquartile range (IQR) of scores for a specific region, with the median indicated by a line inside the box.
- Outliers, if any, are marked outside the whiskers of the plot.
**Key Insights:**

- **Regions with High Variability:** Some regions, like **Sub-Saharan Africa**, display a wide range of happiness scores, indicating high variability in well-being within the region.
- **Regions with Low Variability:** **Northern European** countries show less variability, with consistently high happiness scores across the region.
- **Median Happiness Scores:** The median scores provide insight into the central tendency of happiness within each region, with **Northern Europe** having the highest median scores.
**Observations:**

- The box plot is useful for understanding the spread and distribution of happiness scores within different regions, highlighting both the consistency and variability of well-being across the world.


## Summary

The visualizations collectively provide a comprehensive view of the global distribution of happiness in 2019. The map reveals geographic trends in happiness scores across countries, highlighting areas of both high and low well-being. The bar chart allows for a detailed comparison of happiness levels among individual countries, showcasing significant disparities. The box plot offers insights into the regional distributions and variability of happiness scores, illustrating how well-being differs across regions.

Together, these visualizations reveal substantial global disparities in happiness, with certain regions consistently ranking higher or lower in terms of overall well-being. The findings suggest that while wealth is a significant contributor to happiness, it is not the only factor. Other elements, such as social support, freedom, and life expectancy, also play crucial roles in shaping happiness levels across the world.
