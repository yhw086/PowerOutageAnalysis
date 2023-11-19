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
The dataset


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

| Year | Anomaly.level | Climate.category |   Cause.category   | Outage.duration | Customers.affected | Climate |
|-----:|--------------:|:-----------------|-------------------:|----------------:|-------------------:|--------:|
| 2011 |     -0.3      | 	normal        | severe weather     |	  3060   	 |		70000.0	      | normal  |
| 2014 |     -0.1      | 	normal        | intentional attack |		1        |		NaN			  | normal  |
| 2010 |     -1.5      | 	cold          | severe weather     |	  3000       |		70000.0		  |	extreme |
| 2012 |     -0.1      | 	normal        | severe weather     |	  2550       |		68200.0		  | normal  |
| 2015 |      1.2      | 	warm          | severe weather     |	  1740       |		250000.0	  |	extreme |
 

### Univariate Analysis
With data cleaned for analyzation, we looked at the overall distribution of the calories of the recipes and the distribution of years in which the recipes has been commented on.
#### Distribution of Years
H

#### Distribution of Calories
H


### Bivariate Analysis
T

### Interesting Aggregates
S

---
## Assessment of Missingness
### NMAR Analysis
This

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