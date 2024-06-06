# Clustering

## Hierarchial clustering in R
[Here](https://www.youtube.com/watch?v=MAUs4484TG8) is a nice tutorial on hierachial clustering.

Hierarchial clustering is an unsupervised clustering method. It is an alternative to K-means that does not require specifying number of clusters. Clustering can be top-down or bottom-up (agglomerative, AGNES). Distance metrics for hierarchial clustering: Euclidean, Manhattan, Max distance.

Steps:
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
