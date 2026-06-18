# Nairobi Air Quality Time Series Analysis (2018)

## Overview

This project analyses how particulate matter, humidity, and temperature varied in Nairobi between January and March 2018 using data from low-cost air quality sensors.

The focus of this work is on preparing and cleaning the dataset as a foundation for time series analysis, including decomposition, stationarity testing, and forecasting in later stages.

The project investigates a central question:

**How did air quality and weather conditions evolve over time in Nairobi during early 2018, and what patterns can be observed in particulate matter and meteorological variables?**

---

## Why This Matters

Air pollution, particularly PM2.5 and PM10, is strongly associated with respiratory and cardiovascular disease. In many African cities, continuous air quality monitoring is limited due to the high cost of traditional monitoring systems.

Low-cost sensor networks, such as those from sensors.AFRICA, provide an alternative source of data, but the readings often require careful cleaning and validation before they can be used for analysis.

This project focuses on that preparation stage, treating data quality decisions as part of the analysis rather than a separate preprocessing step.

---

## Dataset

- Source: sensors.AFRICA Air Quality Archive
- Location: Nairobi, Kenya
- Period: January to March 2018
- Data type: Low-cost sensor readings

### Variables Included

- PM2.5 (fine particulate matter)
- PM10 (coarse particulate matter)
- Humidity (%)
- Temperature (°C)
- Sensor metadata (sensor ID, location, coordinates, timestamp)

All measurements are stored in a single `value` column and differentiated using a `value_type` field.

---

## Tools Used

- Python
- pandas
- matplotlib
- datetime handling in pandas
- Google Colab

---

## Data Preparation

### Combining and restructuring

The three monthly datasets were combined into a single dataframe. The timestamp column was converted to datetime format to support time series operations.

### Separating measurement types

Because all variables were stored in a single `value` column, the dataset was split into four subsets based on `value_type`:
- PM2.5
- PM10
- Humidity
- Temperature

This separation was necessary to ensure that each variable was analysed on its own scale.

---

### Outlier detection

IQR-based outlier detection was applied independently to each subset.

For PM10 and PM2.5, the IQR method produced negative lower bounds. However, a validation check showed that no negative values existed in the dataset. As a result, the lower bound was treated as a mathematical artefact rather than a real data issue.

### Handling extreme values

Upper-bound outliers for particulate matter were significantly higher than expected:

- PM10 reached values up to 1685.77 µg/m³
- PM2.5 reached values up to 628.63 µg/m³

These values were far above WHO guideline thresholds and were interpreted as likely sensor errors.

Instead of removing rows, values were capped using the IQR upper bound. This decision preserved the continuity of the time series, which is important for later decomposition and forecasting.

Temperature values were reviewed separately. Although some readings reached 37.8°C, these were not physically impossible and were retained.

Humidity showed no significant outliers.

---

### Time series formatting

The timestamp column was set as the index for each subset to enable proper time series operations such as resampling and trend analysis.

---

## Key Observations

### Particulate matter trends

PM10 and PM2.5 followed similar patterns throughout the period. Both showed higher levels in January, a decline through February, and a moderate increase in March.

The close relationship between both variables is expected, as they often share common emission sources.

---

### Humidity patterns

Humidity remained relatively lower and more variable in January and February, before increasing sharply in March. This pattern is consistent with the transition into Nairobi’s rainy season.

---

### Temperature variation

Temperature remained relatively stable, mostly within the 21°C to 25°C range. A sharp dip in late January was observed but not further investigated in this iteration.

---

### Relationship between variables

An inverse pattern was observed between humidity and particulate matter levels, where higher humidity periods corresponded with lower particulate concentrations.

This is consistent with known atmospheric behaviour, where increased humidity can contribute to particle aggregation and settling. This relationship is noted for further statistical testing in later stages of the project.

---

## Visualisation

![Four-panel time series of PM10, PM2.5, humidity, and temperature](images/air_quality_four_panel.png)

Daily averages were computed and plotted for each variable to support visual comparison across time.

---

## Limitations

- Sensor data may contain measurement errors due to the nature of low-cost monitoring systems.
- Temperature anomalies were not further validated against external weather data.
- The analysis is limited to a three-month period, which restricts long-term seasonal interpretation.

---

## Next Steps

This project forms the first stage of a broader time series analysis pipeline.

Future work will include:

- Seasonal decomposition of each variable
- Stationarity testing
- Time series forecasting (e.g., ARIMA models)
- Formal statistical testing of relationships between humidity and particulate matter
- Investigation of temperature anomalies using external climate data

---

## Repository Structure
```
nairobi-air-quality-timeseries/

│

├── notebooks/  
│   └── aiTime_Series_Analysis_Nairobi_Air_Quality_Dataset.ipynb  

├── images/  
│   └── air_quality_four_panel.png  

├── README.md

└── requirements.txt
```

---
## Author

**Faith Olaniyi**  
Computer Science Graduate | Data Science & AI for Healthcare  
[LinkedIn](https://www.linkedin.com/in/faith-oluwanifemi-olaniyi) · [GitHub](https://github.com/faithopia21)

## About the Author

Faith Olaniyi is a Computer Science graduate with interests in healthcare analytics, public health data, machine learning, and AI applications in real-world systems. She is currently building technical and research experience through data science projects focused on environmental and health-related datasets, while preparing for graduate studies in Data Science and Artificial Intelligence.