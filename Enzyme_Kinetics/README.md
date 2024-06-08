# Methods for analysis of enzyme kinetics

**Michaelis-Menton equation** is a simple way to calculate enzyme activities using this equation:

$v = {vmax * S \over (S + Km)}$ where

* v = reaction rate
* vmax = max reaction rate (substrate not limiting)
* S = substrate concentration
* Km = substrate concentration where v = vmax / 2

Michaelis-menton kinetics requires that a few basic assumptions are met:
1. Substrate concentration is higher than the enzyme concentration (substrate not limiting)
2. The rate of product formation is constant
3. The reaction only goes forward.  

[renz](https://bmcbioinformatics.biomedcentral.com/articles/10.1186/s12859-022-04729-4) is an R package available from the CRAN repository for analysis of Michaelis-Menton enzyme kinetics data. It can be installed in R Studio usiong *install.packages("renz")*. 

## Calculate the initial reaction rate of an enzyme (vmax)

As the enzyme converts substrate to product, there is an initial, linear reaction period at which the enzyme is working at max velocity. The reaction rate gradually declines as the substrate becomes limiting. This [script](https://github.com/actolonen/Analysis_Lab/blob/main/Enzyme_Kinetics/reactionRate_method.html) provides two methods to identify the points corresponding to the initial, linear reaction and to calculate the reaction rate (substrate/min/enzyme) during this period:

1. Method 1: points are selected starting from t=0 into windows of increasing size. Each time a point is added, the slope of the linear model is re-calculated. The slopes are clustered. Points that cluster together with slopes are selected for the linear regression.
2. Method 2: points are selected based on a sliding window of size = n, slopes are calculated and clustered, points belowing to the cluster with the highest slopes are included in the linear correlation.
