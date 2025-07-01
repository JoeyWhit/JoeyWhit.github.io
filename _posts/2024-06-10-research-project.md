---
layout: post
title: 2024 Research Project - On the 3:2 spin-orbit resonance of Mercury
image: "/posts/Mercury_in_true_color.jpg"
tags: [Python, Linux, Astrophysics, Mathematics]
---

Hello! This is just a brief post to share a high level overview of some of the findings and learnings from my research project completed in 2024. This was an Astrophysics project in collaboration with an associate professor at Monash University, which was on the phenomenon known as the '3:2 spin-orbit resonance of Mercury'. We use Python, virtual environment and simulations in Linux to see which key input variables affects this resonance, and also using LaTeX to write up a professional research project report.

# Table of contents

- [00. Project Overview](#overview-main)
    - [Context](#overview-context)
    - [Actions](#overview-actions)
    - [Results](#overview-results)
    - [Growth/Next Steps](#overview-growth)
- [01. Data Overview](#data-overview)
- [02. K-Means](#kmeans-title)
    - [Concept Overview](#kmeans-overview)
    - [Data Preprocessing](#kmeans-preprocessing)
    - [Finding A Good Value For K](#kmeans-k-value)
    - [Model Fitting](#kmeans-model-fitting)
    - [Appending Clusters To Customers](#kmeans-append-clusters)
    - [Segment Profiling](#kmeans-cluster-profiling)
- [03. Application](#kmeans-application)
- [04. Growth & Next Steps](#growth-next-steps)

___

# Project Overview  <a name="overview-main"></a>

### Context <a name="overview-context"></a>

<br>
<br>

### Actions <a name="overview-actions"></a>

<br>
<br>

### Results <a name="overview-results"></a>

Based upon...

<br>
<br>
### Growth/Next Steps <a name="overview-growth"></a>

It would be interesting... 
<br>
<br>

___

# Data Overview  <a name="data-overview"></a>

We are primarily looking to...

In the code below, we:

* Import the required python packages & libraries
* Import the tables from the database
* Merge the tables to tag on *product_area_name* which only exists in the *product_areas* table
* Drop the non-food categories
* Aggregate the sales data for each product area, at customer level
* Pivot the data to get it into the right format for clustering
* Change the values from raw dollars, into a percentage of spend for each customer (to ensure each customer is comparable)

<br>
```python

# import required Python packages
from sklearn.cluster import KMeans
from sklearn.preprocessing import MinMaxScaler
import pandas as pd
import matplotlib.pyplot as plt

# import tables from database
transactions = ...
product_areas = ...

# merge product_area_name on
transactions = pd.merge(transactions, product_areas, how = "inner", on = "product_area_id")

# drop the non-food category
transactions.drop(transactions[transactions["product_area_name"] == "Non-Food"].index, inplace = True)

# aggregate sales at customer level (by product area)
transaction_summary = transactions.groupby(["customer_id", "product_area_name"])["sales_cost"].sum().reset_index()

# pivot data to place product areas as columns
transaction_summary_pivot = transactions.pivot_table(index = "customer_id",
                                                    columns = "product_area_name",
                                                    values = "sales_cost",
                                                    aggfunc = "sum",
                                                    fill_value = 0,
                                                    margins = True,
                                                    margins_name = "Total").rename_axis(None,axis = 1)

# transform sales into % sales
transaction_summary_pivot = transaction_summary_pivot.div(transaction_summary_pivot["Total"], axis = 0)

# drop the "total" column as we don't need that for clustering
data_for_clustering = transaction_summary_pivot.drop(["Total"], axis = 1)

```

<br>
That code gives us the below plot - which visualises our results!

<br>
![alt text](/img/posts/kmeans-optimal-k-value-plot.png "K-Means Optimal k Value Plot")

<br>
Based upon the shape of the above plot...


<br>
### Model Fitting <a name="kmeans-model-fitting"></a>


```python

# instantiate our k-means object
kmeans = KMeans(n_clusters = 3, random_state = 42)

# fit to our data
kmeans.fit(data_for_clustering_scaled)

```

<br>
### Append Clusters To Customers <a name="kmeans-append-clusters"></a>

```python

# add cluster labels to our original data
data_for_clustering["cluster"] = kmeans.labels_

```

<br>
### Cluster Profiling <a name="kmeans-cluster-profiling"></a>


<br>
##### Cluster Sizes


<br>
```python

# check cluster sizes
data_for_clustering["cluster"].value_counts(normalize=True)

```
<br>

Running that code shows us that the three clusters are different in size, with the following proportions:

* Cluster 0: **73.6%** of customers
* Cluster 2: **14.6%** of customers
* Cluster 1: **11.8%** of customers

Based on these results, it does appear we do have a skew toward Cluster 0 with Cluster 1 & Cluster 2 being proportionally smaller.  This isn't right or wrong, it is simply showing up pockets of the customer base that are exhibiting different behaviours - and this is *exactly* what we want.

<br>
##### Cluster Attributes

To understand what these different behaviours or characteristics are, we can look to analyse the attributes of each cluster, in terms of the variables we fed into the k-means algorithm.

<br>
```python

# profile clusters (mean % sales for each product area)
cluster_summary = data_for_clustering.groupby("cluster")[["Dairy","Fruit","Meat","Vegetables"]].mean().reset_index()

```
<br>

___
<br>
# Application <a name="kmeans-application"></a>

Even those this is a simple solution, based upon high level product areas it will help leaders in the business, and category managers gain a clearer understanding of the customer base.

Tracking these clusters over time would allow the client to more quickly react to dietary trends, and adjust their messaging and inventory accordingly.

Based upon these clusters, the client will be able to target customers more accurately - promoting products & discounts to customers that are truly relevant to them - overall enabling a more customer focused communication strategy.

___
<br>
# Growth & Next Steps <a name="growth-next-steps"></a>

It would be interesting to run this clustering/segmentation at a lower level of product areas, so rather than just the four areas of Meat, Dairy, Fruit, Vegetables - clustering spend across the sub-categories *below* those categories.  This would mean we could create more specific clusters, and get an even more granular understanding of dietary preferences within the customer base.

Here we've just focused on variables that are linked directly to sales - it could be interesting to also include customer metrics such as distance to store, gender etc to give a even more well-rounded customer segmentation.

It would be useful to test other clustering approaches such as hierarchical clustering or DBSCAN to compare the results.
