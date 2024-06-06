# Data Clustering

## Hierarchial clustering in R
[Here](https://www.youtube.com/watch?v=MAUs4484TG8) is a nice tutorial on hierachial clustering.

Hierarchial clustering is an unsupervised clustering method (observations are unlabeled) that does not require specifying number of clusters. Clustering can be top-down or bottom-up (agglomerative, AGNES). Distance metrics for hierarchial clustering: Euclidean, Manhattan, Max distance.

Steps for hierarchical clustering of iris dataset
1. organize data into data.frame: rows = n observations, cols = m variables
2. Remove any non-numeric variables
3. Normalize each variable to Z-scores (mean = 0, sd = 1) using scale(). Returns n x m matrix
4. Calculate distance between each observations using dist(). Returns n x n matrix.
5. Perform hierarchial clustering usign hclust(). Returns a list of 7 elts.    

hclust() list:
1. merge: describes merging of clusters at each step of clustering.
2. height
3. order
4. labels
5. method
6. call: command used to run hclust()
7. dist.method Distance metric for hierarchial clustering: Euclidean, Manhattan, Max distance.

## K-means clustering in R

[Here](https://www.youtube.com/watch?v=NKQpVU1LTm8) is a nice tutorial on K-means clustering.

K-means is an unsupervised clustering method that groups observations into a user-defined number of clusters. The goal of k-means is to partition the data into 
k groups such that Euclidean distance from points to the assigned cluster centres is minimized. It is an iterative method whereby K centroids are placed in the data space and each data point is assigned to the nearest cluster based on Euclidean distance. The centroids are then moved to the centers of the new clusters and the observations are reassigned. This process is iterated until either the centroids no longer move or the number of iterations are reached.
