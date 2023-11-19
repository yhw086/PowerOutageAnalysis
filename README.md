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

#### Outage Duration Count Across Anomaly Levels
The line chart titled "Outage Duration Count Across Anomaly Levels" plots the count of power outage durations against various anomaly levels. The x-axis represents the anomaly levels, likely measured by the Oceanic Niño Index, with values ranging from less negative (La Niña conditions) to positive (El Niño conditions). The y-axis quantifies the number of outages recorded at each of these levels.

From the chart, we can observe a significant peak around the anomaly level of -0.5, suggesting a high count of outages when conditions are transitioning towards La Niña. This peak indicates that the onset of such climatic conditions might be associated with an increased number of power outages. Conversely, at higher anomaly levels indicative of El Niño conditions, the count of outages appears to be relatively. Overall, it appears that there are relatively more major outages hapening in normal climate conditions. This chart does suggest a potential correlation between the climatic conditions expressed by anomaly levels and the frequency of power outages. 

<iframe src="figures/bivar_line.html" width=800 height=600 frameBorder=0></iframe>

#### Climate Category Count by Year
The bar chart "Climate Category Count by Year" presents a comparison of the counts of power outages classified by climate categories—'extreme' and 'normal'—across different years. Each horizontal bar pair represents a year, with the count of outages for each climate category depicted in different colors.

From the visualization, we observe that in most years, the counts of outages in 'extreme' climate conditions tend to be higher than those in 'normal' conditions. This might suggest that extreme weather events, which are likely to include conditions such as heatwaves, or storms, can cause more frequent power disruptions. We also notice there are years that power outages only happened under one climate category(e.g. 2013, 2015). 
However, the chart also shows variability across years. Some years have a relatively balanced count between 'extreme' and 'normal' conditions, while others have a pronounced difference. Notably, in the earlier years of the dataset, the count of outages in normal conditions appears to be comparable or even higher than in extreme conditions, but this trend shifts in later years where extreme conditions account for a higher proportion of outages. 

<iframe src="figures/bivar_group.html" width=800 height=600 frameBorder=0></iframe>

### Interesting Aggregates

Below is a table that presents aggregated statistics of power outage durations across different climate categories—'cold', 'normal', and 'warm'. These categories relate to temperature anomalies such as those associated with El Niño and La Niña phenomena. The `Mean Duration` and `Median Duration` offer insights into the central tendency of outages, with mean values indicating the average duration and median values providing a midpoint that is less affected by extreme outliers. The `Standard Deviation` suggests the variability of the outage durations within each climate category, with larger values indicating more dispersed data. `Count of Outages` reflects the total number of recorded outages, which could indicate the frequency or likelihood of outages in each climate state. The `Min Duration` and `Max Duration` give the range of outage durations, showing the shortest and longest outages experienced in each category. 

The 'warm' category has the highest mean duration at approximately 2817 minutes. The 'cold' category has the longest maximum outage recorded at over 108,653 minutes. In contrast, the 'normal' climate category has the lowest mean duration of around 2531 minutes and a substantially lower maximum duration than the 'cold' category, indicating that outages during normal climate conditions might not last as long. Interestingly, the 'normal' category has the highest count of outages at 730, which could indicate a greater frequency of outages but of shorter duration. All categories have a minimum duration of 0 minutes, which could represent very brief outages or potentially data entry errors. 

| Climate Category | Mean Duration | Median Duration | Standard Deviation | Count of Outages | Min Duration | Max Duration |
|------------------|---------------|-----------------|--------------------|------------------|--------------|--------------|
| cold             | 2656.956803   | 816.0           | 6736.666752        | 463              | 0            | 108653       |
| normal           | 2530.980822   | 563.0           | 5365.118871        | 730              | 0            | 78377        |
| warm             | 2817.318021   | 881.0           | 5990.164205        | 283              | 0            | 49427        |


---
## Assessment of Missingness
### NMAR Analysis
We believe that the `Cause.category.detail` column in the power outage dataset is a prime candidate for data that might be Not Missing at Random (NMAR). This column contains detailed descriptions of the event categories causing the major power outages. This dataset was aquired by using different publicly available datasets. We believe that some of the raw datsets containing information regarding detailed cause categories might be missing for sensitive or controversial outages (e.g., due to sabotage or internal failures) that a utility company might not want to disclose. Utility companies may be inclined to withhold specific details that could lead to public relations issues, or reveal vulnerabilities in their infrastructure. 

To better understand and possibly reclassify this missingness as MAR, additional data would be necessary. This could include internal reports, regulatory filings or third-party investigations. Such information might provide the necessary context to determine if the missingness is related to the causes of outages or if it can be explained by observed variables within the dataset. 

### Missingness Dependency

#### 1. (MCAR)
Null Hypothesis: the distribution of the number of affected customers is the same whether power outage duration is missing or not.

Alternative hypothesis: the distribution of the number of affected customers is not the same whether power outage duration is missing or not.

Observed Statistics: the absolute difference between customers.affected and missing_duration.

We use permutation test to shuffle the missingness of number of affected customers 1000 times. We can get 1000 simulating absolute differences. Then we compare these 1000 absolute differences to the observed difference, and calculate the p-value that is the probability of observing a absolute difference as extreme or more extreme than the observed difference, assuming the null hypothesis is true. Observed statistics:  20599.732900432893

Finally, we get the p-value, which is 0.684. As 0.05 is our significance threshold, since 0.684 > 0.05, we fail to reject the null hypothesis that the distribution of the affected customers is the same whether power outage duration is missing or not. Based on our test result, we can see that the missingness of the power outage duration does not depend on the affected customers.

<iframe src="figures/perm_test_diff.html" width=800 height=600 frameBorder=0></iframe>

**Therefore, we conclude that the missingness of the power outage duration does not depend on the affected customers.**

#### 2. (MAR)

Null Hypothesis: the distribution of the climate(normal and extreme) is the same whether power outage duration is missing or not.

Alternative hypothesis: the distribution of the climate(normal and extreme) is not the same whether power outage duration is missing or not.

Observed Statistics: the Total Variation Distance(TVD) between two categorical features.

We use permutation test to shuffle the missingness of climate category 1000 times. We can get 1000 simulating TVDs. Then we compare these 1000 TVDs to the observed TVD, and calculate the p-value that is the probability of observing a TVD as extreme or more extreme than the observed TVD, assuming the null hypothesis is true.

Observed statistics: 0.2088656600851723

Finally, we get the p-value, which is 0.002. As 0.05 is our significance threshold, since 0.002 < 0.05, we reject the null hypothesis that the distribution of the climate(normal and extreme) is the same whether power outage duration is missing or not. Based on our test result, we can see that the missingness of the power outage duration depends on the climate.

<iframe src="figures/mar.html" width=800 height=600 frameBorder=0></iframe>

**Hence, we conclude that the missingness of the power outage duration depends on the climate.**

---
## Hypothesis Testing
### Permutation Test
Null Hypothesis (HO): The average duration of power outages during 'Extreme' climate conditions is the same as during 'Normal' climate conditions.

Alternative Hypothesis (H1): The average duration of power outages is significantly different between 'Extreme' and 'Normal' climate conditions

Observed Statistics: the absolute difference of power outage duration between extreme climate and normal climate.

According to the analysis on EDA and univariate duration plot above, we can get that there are some outliers in the data of extreme climate and normal climate. Therefore, we would remove these outliers to make the distribution of each climate more precisely.

We use permutation test to shuffle the climate 10000 times. We can get 10000 simulating absolute differences. Then we compare these 10000 absolute differences to the observed difference 186.81, and calculate the p-value that is the probability of observing a absolute difference as extreme or more extreme than the observed difference, assuming the null hypothesis is true.

<iframe src="figures/bivar_group.html" width=800 height=600 frameBorder=0></iframe>


## Conclusion:
Finally, we get the p-value, which is 0.5467. As 0.05 is our significance threshold, since 0.5467 > 0.05, we fail to reject the null hypothesis that the average duration of power outages during 'Extreme' climate conditions is the same as during 'Normal' climate conditions. 

This result could be resonable. Since there are some other possible reason such as internet attack to influence the power outage duration. Therefore, even in the normal climate, this will still increase the durations of power outages.