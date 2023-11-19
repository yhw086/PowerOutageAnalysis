# PowerOutageAnalysis
This is a project for DSC80. 

By Jiayi Zhu & Yihuan Wang. 

## Project Overview
This is a data science project on investigating how climate conditions influence the power outages in the United States? 
The dataset used to investigate the topic can be find [here](https://www.sciencedirect.com/science/article/pii/S2352340918307182#s0015). This project is for DSC80 at UCSD.

---
## Investigating Topic and Introduction
Research Question
**"Does the occurrence of 'Extreme' climate conditions (including both El Niño and La Niña), as defined by the Oceanic Niño Index,  versus 'Normal' conditions significantly influence the average duration of power outages in the United States?"**.

### Introduction to the Datasets in this Study
Power outage represents a significant challenge, which impacts to our environment. Understanding the factors influencing power outage duration is significant for developing effective strategies to enhance the time to restoration and minimize its disruptions. In this project, we aim to investigate the relationship between the normal and extreme conditions and the duration of power outages.

Before investigating our project, we read the excel data into the notebook. The dataset is sourced from the Purdue University laboratory, titled Major Power Outage Risks in the U.S.. The dataset has 1534 rows which represents the number of time the power outage happen in the U.S. The dataset has 56 columns. Since we aim to learn the relationship between the climate and power outages, we select 7 columns to use, including 'Year', 'Anomaly.level', 'Climate.category', 'Cause.category', 'Cause.category.detail', 'Outage.duration', 'Customers.affected', 'Climate'. 

- `Year`: Indicates the year when the outage event occurred.
- `Anomaly.level`: This represents the oceanic El Niño/La Niña (ONI) index referring to the cold and warm episodes by season. It is estimated as a 3-month running mean of ERSST.v4 SST anomalies in the Niño 3.4 region (5°N to 5°S, 120–170°W).
- `Climate.category`: This represents the climate episodes corresponding to the years. The categories—"Warm", "Cold" or "Normal" episodes of the climate are based on a threshold of ±0.5°C for the Oceanic Niño Index (ONI).
- `Cause.category`: Categories of all the events causing the major power outages.
- `Cause.category.detail`: Detailed description of the event categories causing the major power outages.
- `Outage.duration`: Duration of outage events (in minutes).
- `Customers.affected`: Number of customers affected by the power outage event.
- `Climate`: Replaces all 'warm' and 'cold' conditions in 'Limate.category' column by 'extreme', keep 'normal' as it is, according to Oceanic Niño Index.

After setting the dataset, we begin to process the data. Firstly, we will do the Data cleaning and conduct the exploratory data analysis, including Univariate Analysis, Bivariate Analysis, and interesting aggregates.

Later then, we are going to assess the missingness analysis to analysis the column's missing dependency. In the missingness analysis, we will first discuss the NMAR, which is 'Cause.category.detail`. Then we will discuss the MCAR and MAR analysis, and we mainly implement the test to explore the dependency of the missing power outage duration on the 'Climate' and 'Customers.affected' columns.

Moreover, we generate the hypothesis test according to the research question. We would analyze if the average duration of power outages during 'Extreme' climate conditions is the same as during 'Normal' climate conditions. Our research question is important, since it addresses the immediate impact of climate on power outages and also contributes valuable insights for proactive planning and risk reduction in the face of improving climate patterns. This investigation can enhance the resilience of power infrastructures in diverse climatic scenarios.


---
## Cleaning Data and Exploratory Data Analysis
### Data Cleaning
In order to increase the readability and narrow the information we need from the dataset, we follow the following steps to clean our DataFrame: 

1. **Load the data in pandas as a csv file**: The data is given as an Excel file. We inspect the data and decide to skip the first 5 rows which are description of the dataset. The first column `variables` and the first row are descriptions of the columns and `OBS` indicates the id of the records. We drop them since they are not necessary. We also change each column name to lowercase starting with a capital letter. This step increases the readability of the dataset. 

2. **Add column `Climate`**: Since we would like to look at how extreme and normal conditions influence the outage durations in the United States, we add a column `Climate` that replaces all 'warm' and 'cold' conditions by 'extreme' since they are occurences of El Niño and La Niña, as defined by the Oceanic Niño Index. We keep 'normal' as it is. This step allows as to investigate the climate conditions more relevant to our research question. 

3. **Convert type of year from float to int**: This step is to make the information of the column `year` more accurate, we convert the type of the column `Year` from float to int.

4. **Keep only columns relevant to the research question**: Because the original dataset contains 55 variables and we decide to keep only the columns relevant to our study. We keep the columns `Year`, 
`Anomaly.level`, `Climate.category`, `Cause.category`, `Outage.duration`, `Customers.affected`, `Climate`.

We will remove the outliers after investigating the columns more closely. 

After data cleaning, the combined DataFrame looks like the following (only showing the first 5 rows for illustration, the actual combined DataFrame has 1534 rows):

| Year | Anomaly.level | Climate.category |   Cause.category   | Cause.category.detail| Outage.duration | Customers.affected | Climate |
|-----:|--------------:|:-----------------|-------------------:|---------------------:|----------------:|-------------------:|--------:|
| 2011 |     -0.3      | 	normal        | severe weather     |		NaN		      |	     3060   	|		70000.0	     | normal  |
| 2014 |     -0.1      | 	normal        | intentional attack |	   vandalism	  |       1         |		NaN			 | normal  |
| 2010 |     -1.5      | 	cold          | severe weather     |	   heavy wind     |      3000       |		70000.0		 | extreme |
| 2012 |     -0.1      | 	normal        | severe weather     |	   thunderstorm   |      2550       |		68200.0		 | normal  |
| 2015 |      1.2      | 	warm          | severe weather     |	    NaN           |      1740       |		250000.0	 | extreme |
 

### Univariate Analysis
With data cleaned for analysis, we looked at the overall frequency of the climate categories and the distribution of outage durations. 

#### Distribution of Climate Categories
This is a vertical bar plot showing the distribution of Climate categories ('normal' and 'extreme') on the x-axis and the Number of Occurences' of each climate category on the y-axis. There is approximately a balanced distribution. Specifically, it indicates that the power outages recorded in the dataset are not predominantly occurring in one type of climate over another. A 50-50 split in the Climate variable implies that any patterns, correlations, or causal relationships identified between climate conditions and power outages are reflective of a broad range of conditions. This balanced representation enhances the generalizability of the findings. It ensures that the conclusions drawn are not biased towards a particular climatic condition. 

<iframe src="figures/uni_climate.html" width=800 height=600 frameBorder=0></iframe>

#### Distribution of Outage durations
The histogram provided shows the distribution of 'Outage Duration' on the x-axis and the corresponding 'Count' on the y-axis. From the histogram, it appears that the vast majority of power outages are of short duration, as evidenced by the tall bar at the left side of the chart. This initial bar likely represents a range of very short outage durations. As the duration increases (moving right on the x-axis), the frequency of such outages drastically decreases, indicated by lower bars. This indicates that most power outages are brief and resolved quickly, while long-lasting outages are relatively rare. Long-duration outages are much rarer, which may indicate that they are due to more severe issues and less frequent events. This suggests that we need to handle the outliers later in the analysis. 

<iframe src="figures/uni_duration.html" width=800 height=600 frameBorder=0></iframe>

### Bivariate Analysis
T

### Interesting Aggregates
S

---
## Assessment of Missingness
### NMAR Analysis
We believe that the `Cause.category.detail` column in the power outage dataset is a prime candidate for data that might be Not Missing at Random (NMAR). This column contains detailed descriptions of the event categories causing the major power outages. This dataset was aquired by using different publicly available datasets. We believe that some of the raw datsets containing information regarding detailed cause categories might be missing for sensitive or controversial outages (e.g., due to sabotage or internal failures) that a utility company might not want to disclose. Utility companies may be inclined to withhold specific details that could lead to public relations issues, or reveal vulnerabilities in their infrastructure. 

To better understand and possibly reclassify this missingness as MAR, additional data would be necessary. This could include internal reports, regulatory filings or third-party investigations. Such information might provide the necessary context to determine if the missingness is related to the causes of outages or if it can be explained by observed variables within the dataset. 

### Missingness Dependency
This

#### 1. (MCAR)
Null Hypothesis:

**Therefore, we conclude that.**

#### 2. (MAR)

Null Hypothesis: 

**Hence, we conclude that**

---
## Hypothesis Testing
### Permutation Test



## Conclusion:
This