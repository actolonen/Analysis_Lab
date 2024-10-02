# Microbial Growth Analysis

## Plot growth curves from different instruments
In our lab at Genoscope, we grow cells using various types of media that we prepare using these [media recipes](https://github.com/actolonen/Analysis_Lab/blob/main/Growth/Media_Recipes).

We primarily measure microbial growth using two instruments: a [Bioscreen C](https://www.bioscreen.fi/) and a [CLARIOstar microplate reader](https://www.bmglabtech.com/en/clariostar-plus/). The Bioscreen only measures absorbance, but can read 2x100 well, custom honeycomb plates at the same time with temperature control. The Bioscreen is also well-adapted for anaerobic cultures because the wells can contain 500 ul mediuum. The CLARIOstar uses standard, 96 well plates and can measure both absorbance and fluorescence. 


In addition to plotting the growth curves for different treatments, these scripts also calculate growth paramaters (e.g. intrinsic growth rate, generation time, carrying capacity) using the Growthcurver R package as described [here](https://github.com/actolonen/Analysis_Lab/blob/main/Growth/Logistic_Fit/2024.02_growthcurver.md). 

## Bioscreen growth analysis
* Here is an [example file](https://github.com/actolonen/Analysis_Lab/blob/main/Growth/Data/growthData_Bioscreen.xlsx) in .xlsx format of Bioscreen data.
* Bioscreen [code](https://github.com/actolonen/Analysis_Lab/blob/main/Growth/Code/plotGrowth_means_Bioscreen.qmd) and [output](https://github.com/actolonen/Analysis_Lab/blob/main/Growth/Code/plotGrowth_means_Bioscreen.html) to visualize growth curves: simple plots shows growth of each well and comparing treatment means.
* Bioscreen [code](https://github.com/actolonen/Analysis_Lab/blob/main/Growth/Data/2024.08_cphyWT_butanol.qmd): to plot of growth curves and perform ++and Growthcurver calculations.

## Clario growth analysis
* Clario [code](https://github.com/actolonen/Analysis_Lab/blob/main/Growth/Code/plotGrowth_means.qmd) and [output](https://github.com/actolonen/Analysis_Lab/blob/main/Growth/Code/plotGrowth_means.html) to visualize growth curves: simple plots shows growth of each well and comparing treatment means.
* Clario [code](https://github.com/actolonen/Analysis_Lab/blob/main/Growth/Code/2024.08_growth_butanol_dec23.qmd) and [output](https://github.com/actolonen/Analysis_Lab/blob/main/Growth/HTML/01_2024.08_growth_butanol_dec23.html) to plot growth curves from Clario datafile and perform Growthcurver calculations.


