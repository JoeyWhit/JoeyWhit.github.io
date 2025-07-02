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

This is a work in progress - still need to add text below.

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

# import tables from database

# merge product_area_name on

# drop the non-food category

# aggregate sales at customer level (by product area)

# pivot data to place product areas as columns

# transform sales into % sales

# drop the "total" column as we don't need that for clustering

```

<br>
That code gives us the below plot - which visualises our results!

<br>
![alt text](/img/posts/kmeans-optimal-k-value-plot.png "K-Means Optimal k Value Plot")

<br>
Based upon the shape of the above plot...


<br>
### Model Fitting <a name="kmeans-model-fitting"></a>

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


___
<br>
# Growth & Next Steps <a name="growth-next-steps"></a>

It would be interesting to run... 

It would be useful to test other... 
