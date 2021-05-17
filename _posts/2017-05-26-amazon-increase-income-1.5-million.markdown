---
layout: post
title:  Healthcare Resource Allocation
date:   2020-05-26 10:05:55 +0300
image:  /assets/images/blog/post-1.jpg
author: Raye Liu
tags:   Udell Group
---

**To identify high resource use patients and to predict the length of stay and total cost of patients, and to explore whether clustering the patients into homogeneous groups can yield better predictions than in the general body. Clusters of commonly diagnosed diseases like diabetes, diseases of high mortality rates like cancer, and differences between male and female patients are analyzed. Regression models, random forest, k-means clustering, and hierarchical clustering are used.**

> Dataset

Statewide Planning and Research Cooperative System of New York (SPARCS) Hospital Inpatient Discharges contains information about patients discharged from hospitals in New York State in 2012. The dataset has features including race, age group, type of admission, diagnosis, severity index, length of stay, and total charges. To narrow down the scope we are starting with, we will only perform data analysis on cancer patients. The SPARCS cancer data has 35,804 patients.

> Feature Engineering

For feature engineering, our first target feature Length of Stay is continuous. Its values are capped by 120+ which we replaced with 120. The other target feature Total Cost is continuous, ranging from 1,562.44 to 2,193,723.15, with a mean of 5,8879.05. APR Severity of Illness and Risk of Mortality are ordinal in nature and rewritten as integers 1 through 4 with 1 as Minor and 4 as Extreme. For Age Group, its 5 groups are translated into ordinal form in increasing age order. The remaining features are categorical, and the nominal values are converted to numbers using one-hot encoding which creates a column for each possible value and puts a 1 in the applicable column, 0 otherwise.


> Hierarchical Clustering 

Bottom-up agglomerative is used in finding a potential cluster that represents the high-resource-consumption patients with Total Charges, Length of Stay, APR Mortality Risk, and APR Severity of Illness these four target variables as reference. Use Elbow and Silhouetten methods and the gap statistics to locate the number of clusters to be 100. 

Cluster #73 of cardinality 134 is very likely to be the HRU with the following statistics compared with the general mean: All patients in Cluster #73 have been diagnosed with APR-DRG code 4--Tracheostomy with ECMO, and they have major surgeries.  One common diagnosis is respiratory malignancy, which is presumably the cause of ECMO use.

76% of the clusters have a strictly lower standard deviation on Total Charges, 82% on APR Mortality Risk, 91% on APR Severity of Illness, 69% on Length of Stay. This indicates that the clustering method has done a decent job at correctly identifying the patients with less discrepancy and assigning them to the same group.

For the top 10 HRU clusters sorted in descending order by the target columns as well as age and gender, 7 of them are male-dominant in gender. 

> Random Forest

Random forest classifier is used to find the relevant features of high resource users and the triggers behind the high consumption of health care resources. Inspired by Dr. Rosella’s report, we select 1790 patients with mortality risk level 3 and 4 to be the positive target community, approximately 0.05% of the total population but takes 23% of the total charges. 

> K-means

After applying K-means clustering with 100 clusters, we see a significant decrease in the standard deviation for most of the clusters. 76% of the clusters have a strictly lower standard deviation on Total Charges, 80% on Length of Stay. 

Cluster 28 has the highest median total charges of $519,933.78 and length of stay of 35.50 days. Patients in cluster 28 are newborn babies with liver cancer, major surgeries, multiple major procedures.
Cluster 2 has the second highest median total charges of $190,934.4 and length of stay of 21.00 days. Patients in cluster 2 have Tracheostomy with ECMO and major surgeries. This cluster is similar
to the high resource use cluster from hierarchical clustering.

> Conclusion

With clustering, homogeneous and interpretable clusters are found. Standard deviation improved for approximately 80% of the clusters. The reliability of the clusters is confirmed by analyzing the characteristics of patients within the clusters. This project is part of the Canada Ontario healthcare population segmentation project launched by the University of Toronto. The result of the research will also be used for Canada to optimize health resource allocation. The models can aid hospitals in anticipating hospital management in staffing and purchase planning. 

Once Canada Ontario hospital dataset is obtained, features including socioeconomic information and geographic information will be added. We are developing models for population segmentation including sparse tree ensembles. Because health data pertains to life and death, the results should be used with consideration of fairness. Doctors should not diagnose or treat patients solely based on our estimates.




