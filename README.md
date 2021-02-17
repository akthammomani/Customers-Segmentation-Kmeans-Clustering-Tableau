# Project: Optimizing Wholesale Wine business by segmenting customers behavior using Machine Learning - Kmeans powered by Tableau

<p align="center">
  <img width="1000" height="400" src="https://user-images.githubusercontent.com/67468718/108179916-03ce9e80-70bb-11eb-937d-702f5c5151d9.JPG">
</p>

## 1. Introduction

This project is based on a book called [Data Smart](https://www.amazon.com/Data-Smart-Science-Transform-Information/dp/111866146X), written by John Foreman, head of product for MailChimp. This book is an excellent business analytics primer that walks you through a variety of machine learning use cases, complete with sample data sets and detailed instructions on how to set up and solve each case in Excel. 

One of these cases, John describes a fictitious wholesale wine business looking to optimize sales by segmenting customers — restaurant and liquor store owners, into groups based on buying behavior. This is a very common problem faced by all kinds of businesses from large retailers like Walmart and Target to your friendly neighborhood liquor store.

## 2. Dataset

The dataset contains information on marketing newsletters/e-mail campaigns (e-mail offers sent to customers) and transaction level data from customers. The transactional data shows which offer customers responded to, and what the customer ended up buying. The data is presented as an Excel workbook containing two worksheets. Each worksheet contains a different dataset.

We see that the first dataset <code>**WineKMC.xlsx sheet 0**</code>  contains information about each offer such as the month it is in effect and several attributes about the wine that the offer refers to: the variety, minimum quantity, discount, country of origin and whether or not it is past peak. 

The second dataset <code>**WineKMC.xlsx sheet 1**</code> in the second worksheet contains transactional data -- which offer each customer responded to.

## 3. Data Wrangling

We're trying to learn more about how our customers behave, so we can use their behavior (whether or not they purchased something based on an offer) as a way to group similar minded customers together. We can then study those groups to look for patterns and trends which can help us formulate future offers.

The first thing we need is a way to compare customers. To do this, we're going to create a matrix that contains each customer and a 0/1 indicator for whether or not they responded to a given offer.

  * Merge <code>**df_transactions**</code> & <code>**df_offers**</code> on=<code>**offer_id**</code> 
  * Make a pivot table, to show a matrix with customer names in the row index and offer numbers as columns headers and fill NANs with zeros.

## 4. K-Means Clustering (Choosing K)

Recall that in K-Means Clustering we want to maximize the distance between centroids and minimize the distance between data points and the respective centroid for the cluster they are in. True evaluation for unsupervised learning would require labeled data; however, we can use a variety of intuitive metrics to try to pick the number of clusters K. We will introduce two methods: the Elbow method, the Silhouette method and the gap statistic.

  * Choosing  𝐾 : The Elbow Method:
    * **Distortion:** It is calculated as the average of the squared distances from the cluster centers of the respective clusters. Typically, **the Euclidean distance metric is used.**

<p align="center">
  <img width="800" height="400" src="https://user-images.githubusercontent.com/67468718/108182880-59587a80-70be-11eb-8b28-222a1f0ad928.JPG">
</p>
    * **Inertia:** It is the sum of squared distances of samples to their closest cluster center. Typically, **inertia_ attribute from kmeans is used.
    * Lastly, we look at the sum-of-squares error in each cluster against $K$. We compute the distance from each data point to the center of the cluster (centroid) to which the data point was assigned. 

 ![SSE](https://user-images.githubusercontent.com/67468718/108182886-5a89a780-70be-11eb-9a92-875d1de81849.JPG)








