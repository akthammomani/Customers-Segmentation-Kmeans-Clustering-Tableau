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
    * **Inertia:** It is the sum of squared distances of samples to their closest cluster center. Typically, **inertia_ attribute from kmeans is used.
    * Lastly, we look at the sum-of-squares error in each cluster against $K$. We compute the distance from each data point to the center of the cluster (centroid) to which the data point was assigned. 

  * Choosing  𝐾 : The Silhouette Method
    
    There exists another method that measures how well each datapoint  𝑥𝑖  "fits" its assigned cluster and also how poorly it fits into other clusters. This is a different way of looking at the same objective. Denote  𝑎𝑥𝑖  as the average distance from  𝑥𝑖  to all other points within its own cluster  𝑘 . The lower the value, the better. On the other hand  𝑏𝑥𝑖  is the minimum average distance from  𝑥𝑖  to points in a different cluster, minimized over clusters. That is, compute separately for each cluster the average distance from  𝑥𝑖  to the points within that cluster, and then take the minimum.
    
    The silhouette score is computed on every datapoint in every cluster. The silhouette score ranges from -1 (a poor clustering) to +1 (a very dense clustering) with 0 denoting the situation where clusters overlap. Some criteria for the silhouette coefficient is provided in the table below:
    
| Range       | Interpretation                                |
|:-------------|:-----------------------------------------------:|
| 0.71 - 1.0  | A strong structure has been found.            |
| 0.51 - 0.7  | A reasonable structure has been found.        |
| 0.26 - 0.5  | The structure is weak and could be artificial.|
| < 0.25      | No substantial structure has been found.      |

[Source](http://www.stat.berkeley.edu/~spector/s133/Clus.html)

<p align="center">
  <img width="800" height="400" src="https://user-images.githubusercontent.com/67468718/108182880-59587a80-70be-11eb-8b28-222a1f0ad928.JPG">
</p>

<p align="center">
  <img width="800" height="400" src="https://user-images.githubusercontent.com/67468718/108182885-59f11100-70be-11eb-95e0-f302c17df95d.JPG">
</p>

<p align="center">
  <img width="800" height="400" src="https://user-images.githubusercontent.com/67468718/108182886-5a89a780-70be-11eb-9a92-875d1de81849.JPG">
</p>


![silhouette](https://user-images.githubusercontent.com/67468718/108188526-8871ea80-70c4-11eb-9f63-d2dcbca2974f.JPG)

<p align="center">
  <img width="800" height="400" src="https://user-images.githubusercontent.com/67468718/108189239-6036bb80-70c5-11eb-9691-bb35849148e3.JPG">
</p>


## 5. Choosing  𝐾 Summary:
  * **Elbow Method using Distortion from Scipy** confirms that the <code>**elbow point k=3**</code> so the <code>**best k will be=4**</code> (plot starts descending much more slowly after k=3)
  * **Elbow Method using Inertia from kmeans** confirms that the <code>**elbow point k=3**</code> so the <code>**best k will be=4**</code> (plot starts descending much more slowly after k=3)
  * Unfortunately, **Elbow Method using Sum of Squares - SSE** , this plot doesn’t show a real clear “elbow point”, which means it is not straightforward to find the number of clusters using this Sum-of-Squares (SSE) Method. however, from above we can see that <code>k=3 is our most obvious elbow point</code>, Thus <code>best option for K will be 4</code> **(plot starts descending much more slowly after k=3).**
  * Again, The Silhouette Method, there does not appear to be a clear way to find best K in this method, which means it is not straightforward to find the number of clusters using this method. however, from above we can see <code>k=10, 3 and 9 </code>are our best option for now **(Closest to 1 = best Silhouette Score, but clearly Silhouette Scores are relatively small suggesting we have a weak structure!!!)**


## 6. Visualizing Clusters using PCA

How do we visualize clusters? If we only had two features, we could likely plot the data as is. But we have 100 data points each containing 32 features (dimensions). Principal Component Analysis (PCA) will help us reduce the dimensionality of our data from 32 to something lower. For a visualization on the coordinate plane, we will use 2 dimensions. In this exercise, we're going to use it to transform our multi-dimensional dataset into a 2 dimensional dataset.

This is only one use of PCA for dimension reduction. We can also use PCA when we want to perform regression but we have a set of highly correlated variables. PCA untangles these correlations into a smaller number of features/predictors all of which are orthogonal (not correlated). PCA is also used to reduce a large set of variables into a much smaller one.

<p align="center">
  <img width="800" height="400" src="https://user-images.githubusercontent.com/67468718/108194935-0be30a00-70cc-11eb-8ca1-e494895deeae.JPG">
</p>


## 7. Conclusions and Business Recommendations Powered by ML K-means and Tableau Dashboards

  * <code>**Cluster_0**</code> represents customers who loves both French Red Wine and Sparkling Wine (89% consumes  >= 72 Min Qty) specifically: Cabernet Sauvignon & Champagne.
  * <code>**Cluster_1**</code> represents customers who loves Sparkling Wine (75% consumes  >= 72 Min Qty) mainly Champagne but in general they enjoys French sparkling Wine.
  * Customers from both <code>**Cluster_0**</code> & <code>**Cluster_1**</code> (More than 75% of them, consumes >= 72 Min Qty) , they love Cabernet Sauvignon & Champagne  specially if there’s high Discounts since they’re heavy consumers **(Focus group of customers to increase sales by introducing more Discounts on Cabernet Sauvignon & Champagne)**
  * <code>**Cluster_2**</code> represents customers who are not heavy Wine consumers (Almost  100% of them, consumes  = 6 Min Qty) but still they enjoys French Wine in general (sparkling, Red, and some white). **(Focus group of customers to increase revenue by introducing more wine varieties because these guys are NOT wine specific who are not settled yet and they would try more or new varieties).**
  * <code>**Cluster_3**</code> represents customers who loves Red Wine specifically Pinot Noir. These guys will buy Pinot Noir regardless if there’s big Discount or not and they don’t care about the origin either. They’re just Pinot Noir lovers!! **(Focus group of customers to increase revenue by increasing Pinot Noir price!!).**
 
![main](https://user-images.githubusercontent.com/67468718/108069843-af72e280-7018-11eb-85bc-0d04f18e9378.JPG)
![ML_Tableau](https://user-images.githubusercontent.com/67468718/108069846-b0a40f80-7018-11eb-990f-35a9e15c2f5d.JPG) 

## 8.  Clustering Algorithms in Scikit-learn:

 * Affinity propagation
 * Spectral clustering
 * Agglomerative clustering
 * DBSCAN
 
 
k-means is only one of a ton of clustering algorithms. Below is a brief description of several clustering algorithms, and the table provides references to the other clustering algorithms in scikit-learn. 

* **Affinity Propagation** does not require the number of clusters $K$ to be known in advance! AP uses a "message passing" paradigm to cluster points based on their similarity. 

* **Spectral Clustering** uses the eigenvalues of a similarity matrix to reduce the dimensionality of the data before clustering in a lower dimensional space. This is tangentially similar to what we did to visualize k-means clusters using PCA. The number of clusters must be known a priori.

* **Ward's Method** applies to hierarchical clustering. Hierarchical clustering algorithms take a set of data and successively divide the observations into more and more clusters at each layer of the hierarchy. Ward's method is used to determine when two clusters in the hierarchy should be combined into one. It is basically an extension of hierarchical clustering. Hierarchical clustering is *divisive*, that is, all observations are part of the same cluster at first, and at each successive iteration, the clusters are made smaller and smaller. With hierarchical clustering, a hierarchy is constructed, and there is not really the concept of "number of clusters." The number of clusters simply determines how low or how high in the hierarchy we reference and can be determined empirically or by looking at the [dendogram](https://docs.scipy.org/doc/scipy-0.18.1/reference/generated/scipy.cluster.hierarchy.dendrogram.html).

* **Agglomerative Clustering** is similar to hierarchical clustering but but is not divisive, it is *agglomerative*. That is, every observation is placed into its own cluster and at each iteration or level or the hierarchy, observations are merged into fewer and fewer clusters until convergence. Similar to hierarchical clustering, the constructed hierarchy contains all possible numbers of clusters and it is up to the analyst to pick the number by reviewing statistics or the dendogram.

* **DBSCAN** is based on point density rather than distance. It groups together points with many nearby neighbors. DBSCAN is one of the most cited algorithms in the literature. It does not require knowing the number of clusters a priori, but does require specifying the neighborhood size. 

How do their results compare? Which performs the best? Tell a story why you think it performs the best.</p>


<table border="1">
<colgroup>
<col width="15%" />
<col width="16%" />
<col width="20%" />
<col width="27%" />
<col width="22%" />
</colgroup>
<thead valign="bottom">
<tr><th>Method name</th>
<th>Parameters</th>
<th>Scalability</th>
<th>Use Case</th>
<th>Geometry (metric used)</th>
</tr>
</thead>
<tbody valign="top">
<tr><td>K-Means</span></a></td>
<td>number of clusters</td>
<td>Very large<span class="pre">n_samples</span>, medium <span class="pre">n_clusters</span> with
MiniBatch code</td>
<td>General-purpose, even cluster size, flat geometry, not too many clusters</td>
<td>Distances between points</td>
</tr>
<tr><td>Affinity propagation</td>
<td>damping, sample preference</td>
<td>Not scalable with n_samples</td>
<td>Many clusters, uneven cluster size, non-flat geometry</td>
<td>Graph distance (e.g. nearest-neighbor graph)</td>
</tr>
<tr><td>Mean-shift</td>
<td>bandwidth</td>
<td>Not scalable with <span class="pre">n_samples</span></td>
<td>Many clusters, uneven cluster size, non-flat geometry</td>
<td>Distances between points</td>
</tr>
<tr><td>Spectral clustering</td>
<td>number of clusters</td>
<td>Medium <span class="pre">n_samples</span>, small <span class="pre">n_clusters</span></td>
<td>Few clusters, even cluster size, non-flat geometry</td>
<td>Graph distance (e.g. nearest-neighbor graph)</td>
</tr>
<tr><td>Ward hierarchical clustering</td>
<td>number of clusters</td>
<td>Large <span class="pre">n_samples</span> and <span class="pre">n_clusters</span></td>
<td>Many clusters, possibly connectivity constraints</td>
<td>Distances between points</td>
</tr>
<tr><td>Agglomerative clustering</td>
<td>number of clusters, linkage type, distance</td>
<td>Large <span class="pre">n_samples</span> and <span class="pre">n_clusters</span></td>
<td>Many clusters, possibly connectivity constraints, non Euclidean
distances</td>
<td>Any pairwise distance</td>
</tr>
<tr><td>DBSCAN</td>
<td>neighborhood size</td>
<td>Very large <span class="pre">n_samples</span>, medium <span class="pre">n_clusters</span></td>
<td>Non-flat geometry, uneven cluster sizes</td>
<td>Distances between nearest points</td>
</tr>
<tr><td>Gaussian mixtures</td>
<td>many</td>
<td>Not scalable</td>
<td>Flat geometry, good for density estimation</td>
<td>Mahalanobis distances to  centers</td>
</tr>
<tr><td>Birch</td>
<td>branching factor, threshold, optional global clusterer.</td>
<td>Large <span class="pre">n_clusters</span> and <span class="pre">n_samples</span></td>
<td>Large dataset, outlier removal, data reduction.</td>
<td>Euclidean distance between points</td>
</tr>
</tbody>
</table>

[Source](http://scikit-learn.org/stable/modules/clustering.html)





    








