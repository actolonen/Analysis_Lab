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
