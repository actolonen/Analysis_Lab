# Methods for analysis of enzyme kinetics

**Michaelis-Menton equation** enables us to relate enzyme velocity (v) to substrate concentration using a simple equation:

$v = {vmax * S \over (S + Km)}$ 

* v = reaction rate
* vmax = max reaction rate (substrate not limiting)
* S = substrate concentration
* Km = substrate concentration where v = vmax / 2

Michaelis-menton kinetics requires that a few basic assumptions are met:
1. Substrate concentration is higher than the enzyme concentration (substrate not limiting).
2. The rate of product formation is constant (the product has no effect on the catalyzed reaction rate).
3. The reaction only goes forward (substrate concentration drops as it is converted into a product by the direct unidirectional reaction catalyzed by our enzyme).

To apply the Michaelis-Menton equation to calculate reaction velocity at different substrate concentrations for a given enzyme, we need to first calculate the Km and the Vmax for that enzyme. The R package [renz](https://bmcbioinformatics.biomedcentral.com/articles/10.1186/s12859-022-04729-4) is available from the CRAN repository for analysis of Michaelis-Menton enzyme kinetics data. It can be installed in R Studio as *install.packages("renz")*. Below, we discuss three methods to calculate Km and Vmax using renz.

Methods 2 and 3 below rely upon calculating the intial reaction rate at a range of substrate concentrations. As the enzyme converts substrate to product, there is an initial, linear reaction period at which the enzyme is working at max velocity. The reaction rate gradually declines as the substrate becomes limiting. This [script](https://github.com/actolonen/Analysis_Lab/blob/main/Enzyme_Kinetics/initialReactionRate_methods.md) provides two methods to identify the points corresponding to the initial, linear reaction and to calculate the reaction rate (substrate/min/enzyme) during this period.

## Method 1: calculate Km, Vmax directly from substrate versus time curves using fE.progress() 

fE.progress uses the Schnell-Mendoza equation to obtain the kinetic parameters of the enzyme from a single substrate versus time curve. 

## Method 2: calculate V vs S curves at different initial substrate concentrations, then calculate Km, Vmax using dir.MM()

We first need to calculate the enzyme initial velocities at a range of substrate concentrations (see below). We then can use dir.MM() from the renz package to perform a non-linear least square fitting of kinetic data to the Michaelis-Menten equation.

## Method 3: calculate V vs S curves at different initial substrate concentration, then calculate Km, Vmax by a linear transformation using Lineweaver-Burke.

1. Method 1: points are selected starting from t=0 into windows of increasing size. Each time a point is added, the slope of the linear model is re-calculated. The slopes are clustered. Points that cluster together with slopes are selected for the linear regression.
2. Method 2: points are selected based on a sliding window of size = n, slopes are calculated and clustered, points belowing to the cluster with the highest slopes are included in the linear correlation.
