# Power Outage Analysis
This is a project for DSC80. 

By Jiayi Zhu & Yihuan Wang. 

## Project Overview
This is a data science project investigating how climate conditions influence power outages in the United States.  
The dataset used to investigate the topic can be found [here](https://www.sciencedirect.com/science/article/pii/S2352340918307182#s0015). This project is for DSC80 at UCSD.

---
## Investigating Topic and Introduction
Research Question
**"Does the occurrence of 'Extreme' climate conditions (including both El Niño and La Niña), as defined by the Oceanic Niño Index,  versus 'Normal' conditions significantly influence the average duration of power outages in the United States?"**.

### Introduction to the Datasets in this Study and Overview of Project
Power outages pose significant challenges and have a profound impact on our environment. Understanding the factors that influence the duration of power outages is crucial for developing effective strategies to enhance restoration times and minimize disruptions. In this project, we aim to explore the correlation between normal and extreme weather conditions and the duration of power outages.

### Data Source:
The dataset used in this study is obtained from Purdue University's laboratory and is titled "Major Power Outage Risks in the U.S.". It comprises 1,534 records, each representing a power outage event in the United States, and includes 55 columns. To focus on the relationship between climate and power outages, we have selected 8 key columns for analysis: including `Year`, `Anomaly.level`, `Climate.category`, `Cause.category`, `Cause.category.detail`, `Outage.duration`, `Customers.affected`, `Climate`. 

- `Year`: Indicates the year when the outage event occurred.
- `Anomaly.level`: This represents the oceanic El Niño/La Niña (ONI) index referring to the cold and warm episodes by season. It is estimated as a 3-month running mean of ERSST.v4 SST anomalies in the Niño 3.4 region (5°N to 5°S, 120–170°W).
- `Climate.category`: This represents the climate episodes corresponding to the years. The categories—"Warm", "Cold" or "Normal" episodes of the climate are based on a threshold of ±0.5°C for the Oceanic Niño Index (ONI).
- `Cause.category`: Categories of all the events causing the major power outages.
- `Cause.category.detail`: Detailed description of the event categories causing the major power outages.
- `Outage.duration`: Duration of outage events (in minutes).
- `Customers.affected`: Number of customers affected by the power outage event.
- `Climate`: Replaces all 'warm' and 'cold' conditions in `Climate.category` column by 'extreme', keep 'normal' as it is, according to Oceanic Niño Index.

### Data Preprocessing:
We have replaced all occurrences of "warm" and "cold" conditions in the `Climate Category` column with "extreme," while retaining "normal" as it is, based on the Oceanic Niño Index.

### Data Cleaning: 
We begin by cleaning the dataset and conducting exploratory data analysis, including univariate and bivariate analysis, as well as exploring interesting aggregates.

### Missingness Analysis: 
We assess missing data and dependencies within the dataset. This includes analyzing the `Cause.category.detail` column for Not Missing at Random (NMAR) patterns and testing for Missing Completely at Random (MCAR) and Missing at Random (MAR) in relation to the `Climate` and `Customers.affected` columns. 

### Hypothesis Testing: 
We formulate hypothesis tests to answer specific research questions. One such question is **whether the average duration of power outages during "Extreme" climate conditions is statistically different from that during "Normal" climate conditions.** This analysis is vital as it sheds light on the immediate impact of climate on power outages and contributes valuable insights for proactive planning and risk reduction. It has the potential to enhance the resilience of power infrastructures under diverse climatic scenarios. 

### Why people should care about this investigation
By conducting this investigation, we aim to provide valuable information that can inform decision-makers and stakeholders in the power industry, helping them better understand the relationship between climate conditions and power outage durations and enabling them to develop strategies for improved power grid resilience and reliability.


---
## Cleaning Data and Exploratory Data Analysis
### Data Cleaning
In order to increase the readability and narrow down the information we need from the dataset, we follow the following steps to clean our DataFrame: 

1. **Load the data in pandas as a csv file**: We begin by loading the dataset from an Excel file into a Pandas DataFrame. We inspect the data and decide to skip the first 5 rows which are descriptions of the dataset. The first column `variables` and the first row are descriptions of the columns. Additionally, the `OBS` column represents record identifiers. Since these elements do not contribute to our analysis, we drop them from the DataFrame. To improve readability, we transform each column name to lowercase, with words starting with a capital letter. This step simplifies column naming conventions. 

2. **Add column `Climate`**: As our analysis focuses on understanding how extreme and normal climate conditions influence power outage durations in the United States, we introduce a new column, `Climate`. In this `Climate` column, we replace occurrences of 'warm' and 'cold' conditions with 'extreme.' These conditions are associated with El Niño and La Niña events, as defined by the Oceanic Niño Index. We keep 'normal' conditions as they are. This step allows as to investigate the climate conditions more relevant to our research question. 

3. **Convert type of year from float to int**: To ensure the accuracy of the 'Year' column, we convert its data type from float to int. This adjustment provides more precise information regarding the year in which each power outage event occurred. 

4. **Keep only columns relevant to the research question**: Because the original dataset contains 55 variables we decided to keep only the columns relevant to our study. We keep the columns `Year`, 
`Anomaly.level`, `Climate.category`, `Cause.category`, `Cause.category.detail`, `Outage.duration`, `Customers.affected`, `Climate`.

Our next step involves the removal of outliers, where we will closely examine and address any anomalous data points that may affect the accuracy and reliability of our analysis.

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
This is a vertical bar plot showing the distribution of Climate categories ('normal' and 'extreme') on the x-axis and the Number of Occurences' of each climate category on the y-axis. There is approximately a balanced distribution. Specifically, it indicates that the power outages recorded in the dataset are not predominantly occurring in one type of climate over another. An almost 50-50 split in the Climate variable implies that any patterns, correlations, or causal relationships identified between climate conditions and power outages are reflective of a broad range of conditions. This balanced representation enhances the generalizability of the findings. It ensures that the conclusions drawn are not biased towards a particular climatic condition. 

<iframe src="figures/uni_climate.html" width=800 height=600 frameBorder=0></iframe>

#### Distribution of Outage durations
The histogram provided shows the distribution of 'Outage Duration' on the x-axis and the corresponding 'Count' on the y-axis. From the histogram, it appears that the vast majority of power outages are of short duration, as evidenced by the tall bar on the left side of the chart. This initial bar likely represents a range of very short outage durations. As the duration increases (moving right on the x-axis), the frequency of such outages drastically decreases, as indicated by lower bars. This indicates that most power outages are brief and resolved quickly, while long-lasting outages are relatively rare. Long-duration outages are much rarer, which may indicate that they are due to more severe issues and less frequent events. This suggests that we need to handle the outliers later in the analysis. 

<iframe src="figures/uni_duration.html" width=800 height=600 frameBorder=0></iframe>

### Bivariate Analysis

#### Outage Duration Count Across Anomaly Levels
The line chart titled "Outage Duration Count Across Anomaly Levels" plots the count of power outage durations against various anomaly levels. The x-axis represents the anomaly levels, likely measured by the Oceanic Niño Index, with values ranging from less negative (La Niña conditions) to positive (El Niño conditions). The y-axis quantifies the number of outages recorded at each of these levels.

From the chart, we can observe a significant peak around the anomaly level of -0.5, suggesting a high count of outages when conditions are transitioning towards La Niña. This peak indicates that the onset of such climatic conditions might be associated with an increased number of power outages. Conversely, at higher anomaly levels indicative of El Niño conditions, the count of outages appears to be relatively low. Overall, it appears that there are relatively more major outages happening in normal climate conditions. This chart does suggest a potential correlation between the climatic conditions expressed by anomaly levels and the frequency of power outages. 

<iframe src="figures/bivar_line.html" width=800 height=600 frameBorder=0></iframe>

#### Climate Category Count by Year
The bar chart "Climate Category Count by Year" presents a comparison of the counts of power outages classified by climate categories—'extreme' and 'normal'—across different years. Each horizontal bar pair represents a year, with the count of outages for each climate category depicted in different colors.

From the visualization, we observe that in most years, the counts of outages in 'extreme' climate conditions tend to be higher than those in 'normal' conditions. This might suggest that extreme weather events, which are likely to include conditions such as heatwaves, or storms, can cause more frequent power disruptions. We also notice there are years that power outages only happened under one climate category(e.g. 2013, 2015). 
However, the chart also shows variability across years. Some years have a relatively balanced count between 'extreme' and 'normal' conditions, while others have a pronounced difference. Notably, in the earlier years of the dataset, the count of outages in normal conditions appears to be comparable or even higher than in extreme conditions, but this trend shifts in later years where extreme conditions account for a higher proportion of outages. 

<iframe src="figures/bivar_group.html" width=800 height=600 frameBorder=0></iframe>

### Interesting Aggregates

Below is a table that presents aggregated statistics of power outage durations across different climate categories—'cold', 'normal', and 'warm'. These categories relate to temperature anomalies such as those associated with El Niño and La Niña phenomena. The `Mean Duration` and `Median Duration` offer insights into the central tendency of outages, with mean values indicating the average duration and median values providing a midpoint that is less affected by extreme outliers. The `Standard Deviation` suggests the variability of the outage durations within each climate category, with larger values indicating more dispersed data. `Count of Outages` reflects the total number of recorded outages, which could indicate the frequency or likelihood of outages in each climate state. The `Min Duration` and `Max Duration` give the range of outage durations, showing the shortest and longest outages experienced in each category. 

The 'warm' category has the highest mean duration at approximately 2817 minutes. The 'cold' category has the longest maximum outage recorded at over 108,653 minutes. In contrast, the 'normal' climate category has the lowest mean duration of around 2531 minutes and substantially lower maximum duration than the 'cold' category, indicating that outages during normal climate conditions might not last as long. Interestingly, the 'normal' category has the highest count of outages at 730, which could indicate a greater frequency of outages but of shorter duration. All categories have a minimum duration of 0 minutes, which could represent very brief outages or potential data entry errors. 

| Climate Category | Mean Duration | Median Duration | Standard Deviation | Count of Outages | Min Duration | Max Duration |
|------------------|---------------|-----------------|--------------------|------------------|--------------|--------------|
| cold             | 2656.956803   | 816.0           | 6736.666752        | 463              | 0            | 108653       |
| normal           | 2530.980822   | 563.0           | 5365.118871        | 730              | 0            | 78377        |
| warm             | 2817.318021   | 881.0           | 5990.164205        | 283              | 0            | 49427        |


---
## Assessment of Missingness
### NMAR Analysis
We believe that the `Cause.category.detail` column in the power outage dataset is a prime candidate for data that might be Not Missing at Random (NMAR). This column contains detailed descriptions of the event categories causing the major power outages. This dataset was acquired by using different publicly available datasets. We believe that some of the raw data containing information regarding detailed cause categories might be missing for sensitive or controversial outages (e.g., due to sabotage or internal failures) that a utility company might not want to disclose. Utility companies may be inclined to withhold specific details that could lead to public relations issues, or reveal vulnerabilities in their infrastructure. 

To better understand and possibly reclassify this missingness as NMAR, additional data would be necessary. This could include internal reports, regulatory filings, or third-party investigations. Such information might provide the necessary context to determine if the missingness is related to the causes of outages or if it can be explained by observed variables within the dataset. 

### Missingness Dependency

#### 1. `Outage.duration` and `Customers.affected`
Null Hypothesis: the distribution of the number of affected customers is the same whether power outage duration is missing or not.

Alternative hypothesis: the distribution of the number of affected customers is not the same whether power outage duration is missing or not.

Observed Statistics: the absolute difference between Customers.affected and missing_duration.

We use a permutation test to shuffle the missingness of the number of affected customers 1000 times. We can get 1000 simulating absolute differences. Then we compare these 1000 absolute differences to the observed difference, and calculate the p-value which is the probability of observing an absolute difference as extreme or more extreme than the observed difference, assuming the null hypothesis is true. Observed statistic:  20599.733

Finally, we get the p-value, which is 0.68. As 0.05 is our significance threshold, since 0.68 > 0.05, we fail to reject the null hypothesis that the distribution of the affected customers is the same whether power outage duration is missing or not. The result suggests that the distribution of the number of customers affected is not statistically different between the groups with missing and non-missing outage duration data. In essence, the missingness of power outage duration data does not appear to be related to the number of customers affected.

Based on our test result, we can see that the missingness of the power outage duration **does not depend on the affected customers**.

<iframe src="figures/perm_test_diff.html" width=800 height=600 frameBorder=0></iframe>

**Therefore, we conclude that the missingness of the power outage duration does not depend on the affected customers.**

#### 2. `Outage.duration` and `Climate`

Null Hypothesis: the distribution of the climate(normal and extreme) is the same whether power outage duration is missing or not.

Alternative hypothesis: the distribution of the climate(normal and extreme) is not the same whether power outage duration is missing or not.

Observed Statistics: the Total Variation Distance(TVD) between two categorical features.

We use the permutation test to shuffle the missingness of the climate category 1000 times. We can get 1000 simulating TVDs. Then we compare these 1000 TVDs to the observed TVD, and calculate the p-value which is the probability of observing a TVD as extreme or more extreme than the observed TVD, assuming the null hypothesis is true.

Observed statistic: 0.20887

Finally, we get the p-value, which is 0.001. As 0.05 is our significance threshold, since 0.001 < 0.05, we reject the null hypothesis that the distribution of the climate(normal and extreme) is the same whether power outage duration is missing or not. The analysis indicates that the distribution of climate categories is statistically different between cases with and without missing power outage duration data. The result suggests a dependency of the missingness of power outage duration on the climate category.

Based on our test result, we can see that the missingness of the power outage duration **depends on the climate**.

<iframe src="figures/mar.html" width=800 height=600 frameBorder=0></iframe>

**Hence, we conclude that the missingness of the power outage duration depends on the climate.**

---
## Hypothesis Testing
### Permutation Test
Null Hypothesis (HO): The average duration of power outages during 'Extreme' climate conditions is the same as during 'Normal' climate conditions.

Alternative Hypothesis (H1): The average duration of power outages is significantly different between 'Extreme' and 'Normal' climate conditions

Observed Statistics: the absolute difference in power outage duration between extreme climate and normal climate.

According to the analysis of EDA and univariate duration plot above, we can see that there are some outliers in the data of extreme climate and normal climate. Therefore, we would remove these outliers to make the distribution of each climate more precise.

#### Outlier Analysis and Removal: 
Initial analysis showed outliers in the data for both 'Extreme' and 'Normal' climates. A total of 144 outliers were identified and removed using the Interquartile Range (IQR) method. This step aimed to refine the data for a more accurate comparison of power outage durations.

#### Observed Statistics Before Outlier Removal:
Observed Difference in Means: 186.81
P-Value: 0.5618
This initial result, with a p-value higher than the conventional alpha level of 0.05, suggested failing to reject the null hypothesis.

#### Observed Statistics After Outlier Removal:
Observed Difference in Means: 91.42
P-Value: 0.3105
Post outlier removal, the observed difference in means decreased significantly, and the p-value, while lower, still suggested failing to reject the null hypothesis.

**Permutation Test:** A permutation test with 10,000 shuffles was used to assess the probability of observing an absolute difference in power outage durations as extreme as, or more extreme than, the observed differences, under the null hypothesis.

Before Removing Outliers: The data suggested no significant difference in the average duration of power outages between the two climate conditions.
<iframe src="figures/hypo1.html" width=800 height=600 frameBorder=0></iframe>

After Removing Outliers: The decrease in the observed difference and the p-value further support the conclusion that there is no statistically significant difference in power outage durations between 'Extreme' and 'Normal' climate conditions.
The analysis, therefore, fails to reject the null hypothesis in both cases. 

<iframe src="figures/hypo.html" width=800 height=600 frameBorder=0></iframe>


## Conclusion:
By detecting the outliers, we find that the number of outliers in `Outage.duration` is 144. Then we would like to address the situation with the original data and with the data removed outilers. Therefore, we find that the observed difference for the original data is 186.81, while the observed difference for the data after removing the outliers is 91.42. The p-value is 0.5618 for the original data, and 0.334 for the data removed outliers. Finally, as 0.05 is our significance threshold, both 0.5618 and 0.3105 are greater than 0.05, then we fail to reject the null hypothesis that the average duration of power outages during 'Extreme' climate conditions is the same as during 'Normal' climate conditions. Since the p-value for the data removed outliers is less than that of the original data, it is possible that the probability of observing an absolute difference as extreme or more extreme in outlier-removed data will be less than the original data. This could be reasonable. Consequently, based on the data and the statistical test conducted, we conclude that there is no statistically significant difference in the average duration of power outages between 'Extreme' and 'Normal' climate conditions.

While the statistical analysis suggests similar outage durations across climate conditions, it's important to acknowledge the role of other potential factors. For instance, external influences like cyberattacks or infrastructure vulnerabilities could affect outage durations, irrespective of climate conditions. As with any statistical analysis, this conclusion is limited to the scope of the data and the specific statistical method used. It does not rule out the possibility of climate having an impact under different conditions or in different contexts.
