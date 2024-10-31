# Methods for analysis of enzyme kinetics

**Michaelis-Menten equation** relates enzyme velocity (v) to substrate concentration (S) using this equation:

$v = {vmax * S \over (S + Km)}$

-   v = enzyme velocity (reaction rate)
-   vmax = max reaction rate (substrate not limiting)
-   S = substrate concentration
-   Km = substrate concentration where v = vmax / 2

![see the V0 vs S curvFig: V vs S curve. As the enzyme converts substrate to product, there is an initial, linear reaction period at which the enzyme is working at max velocity (V0). The reaction rate gradually declines as the substrate becomes limiting.](https://github.com/actolonen/Analysis_Lab/blob/main/Enzymes/Images/plotMM.png)

Michaelis-Menten kinetics requires that a few basic assumptions are met:

1.  Substrate concentration is higher than the enzyme concentration (all enzyme molecules occupied).
2.  No product inhibition (the product has no effect on the catalyzed reaction rate).
3.  The reaction only goes forward (substrate concentration drops as it is converted into a product by the unidirectional reaction catalyzed by our enzyme).

We typically measure the consumption of substrate or accumulation of product over time (time vs substrate/product). However, the Michaelis-Menten equation requires measuring V as the initial reaction rate (V0) at a range of substrate concentrations.

This [script](https://github.com/actolonen/Analysis_Lab/blob/main/Enzymes/Code/initialReactionRate_methods.RMD) provides two methods to identify the points corresponding to the initial, linear portion of the curve of substrate (or product) changes over time. Once these points are selected, we fit a linear regression and calculate the initial reaction rate based on the slope of the regression line. Here is a description of these two methods:

1.  Method 1: points are selected starting from t=0 into windows of increasing size. Each time a point is added, the slope of a linear regression is re-calculated. The slopes are clustered. Points that cluster together with slopes are selected for the linear regression.
2.  Method 2: points are selected based on a sliding window of size = n, slopes are calculated and points belowing to the window with the highest slope are included in the linear correlation.

Once we have determined the initial reaction rate at a range of substrate concentrations, we can apply the Michaelis-Menten equation to calculate the enzyme kinetic parameters Vmax and Km. The R package [renz](https://bmcbioinformatics.biomedcentral.com/articles/10.1186/s12859-022-04729-4) is available from the CRAN repository to calculate parameters for Michaelis-Menten enzyme kinetics data. Below, we discuss three methods to calculate Km and Vmax using functions in the renz package.

This [script](https://github.com/actolonen/Analysis_Lab/blob/main/Enzymes/Code/renz_methods.RMD) demonstrates the calculation of enzyme kinetic parameters Km and Vmax using the below three methods using functions in the renz package. For each method, the script compares results using toy data provided with the renz package and actual data from our lab.

## Method 1: calculate Km, Vmax directly from substrate versus time curves

fE.progress uses the Schnell-Mendoza equation to obtain the kinetic parameters of the enzyme from a single substrate versus time curve. In this method, we relate the variation in the concentration of substrate over time integrating the velocity equation as follows:

![](https://github.com/actolonen/Analysis_Lab/blob/main/Enzymes/Images/integration_MM.png)

This yields a linear equation from which the slope can be used to calculate Km and the y-intercept is (Vmax/Km). Since the method does not require calculation of initial rates, it avoid the bias introduced by underestimating initial rates.

## Method 2: non-linear least squares to fit line to S vs V curve

We first need to calculate the enzyme initial velocities at a range of substrate concentrations (S vs V curve). We then can use dir.MM() from the renz package to perform a non-linear least square fitting of kinetic data to the Michaelis-Menten equation.

## Method 3: Lineweaver-Burk

As in method 2, we first need to calculate the enzyme initial velocities at a range of substrate concentrations (S vs V curve). We can then calculate Km and Vmax based on a linear transformation of the Michaelis-Menton equation. We plot the S and V data as 1/V versus 1/S. The y-intercept yields 1/Vmax and the slope is Km/Vmax.

![](https://github.com/actolonen/Analysis_Lab/blob/main/Enzymes/Images/lmTransformation.png)
