# Data Clustering

## Hierarchial clustering in R
Here is a [script](https://github.com/actolonen/Analysis_Lab/blob/main/Clustering/hierarchicalClustering_method.md) and a nice [video tutorial](https://www.youtube.com/watch?v=MAUs4484TG8) on implementation of hierarchical clustering in R.

Hierarchial clustering is an unsupervised clustering method (observations are unlabeled) that does not require specifying number of clusters. Clustering can be top-down or bottom-up (agglomerative, AGNES). Distance metrics for hierarchial clustering: Euclidean, Manhattan, Max distance.

Steps for hierarchical clustering of iris dataset
1. Organize data into data.frame of numerical data: rows = n observations, cols = m variables
2. Normalize each variable to Z-scores (mean = 0, sd = 1) using scale(). Returns n x m matrix
3. Calculate distance between each observations using dist(). Returns n x n matrix.
4. Perform hierarchial clustering usign hclust(). Returns a list of 7 elts.    

hclust() list:
1. merge: describes merging of clusters at each step of clustering.
2. height
3. order
4. labels
5. method
6. call: command used to run hclust()
7. dist.method Distance metric for hierarchial clustering: Euclidean, Manhattan, Max distance.

## K-means clustering in R

Here is a [script](https://github.com/actolonen/Analysis_Lab/blob/main/Clustering/kmeans_method.md) and nice [video tutorial](https://www.youtube.com/watch?v=NKQpVU1LTm8) on implementation of K-means clustering in R.

K-means is an unsupervised clustering method that groups observations into a user-defined number of clusters. The goal of k-means is to partition the data into 
k groups such that Euclidean distance from points to the assigned cluster centres is minimized. It is an iterative method whereby K centroids are placed in the data space and each data point is assigned to the nearest cluster based on Euclidean distance. The centroids are then moved to the centers of the new clusters and the observations are reassigned. This process is iterated until either the centroids no longer move or the number of iterations are reached.

Steps for K-means clustering of iris dataset.
1. Organize data into data.frame of numerical data: rows = n observations, cols = m variables
2. Normalize each variable to Z-scores (mean = 0, sd = 1) using scale(). Returns n x m matrix.
3. Calculate the Euclidean distance between data points using factoextra::get_dist().
4. Identify the 'proper' number of clusters using the elbow method with factoextra::fviz_nbclust().
5. Perform K-means clustering with kmeans() to make a list of 9 elts (see below).
6. Plot clusters using principal components  using factoextra::fviz_cluster().

kmeans() returns a list of 9 elts
1. cluster = vector of integers showing cluster membership of each observation.
2. centers = matrix of k rows showing positions of cluster centers.
3. totss = total sum of squares for distances.
4. withinss = vector of within-cluster sum of squares for each cluster.
5. tot.withinss = total within-cluster sum of squares
6. betweenss = between cluster sum of squares
7. size = number data points in each cluster
8. iter = number of iterations.
9. ifault = integer indicating potential problems.
