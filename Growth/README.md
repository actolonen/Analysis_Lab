# Microbial Growth Analysis

## Plot growth curves from different instruments
In our lab at Genoscope, we grow cells using various types of media that we prepare using these [media recipes](https://github.com/actolonen/Analysis_Lab/blob/main/Growth/Media_Recipes).

We primarily measure microbial growth using two instruments: a [Bioscreen C](https://www.bioscreen.fi/) and a [CLARIOstar microplate reader](https://www.bmglabtech.com/en/clariostar-plus/). The Bioscreen only measures absorbance, but can read 2x100 well, custom honeycomb plates at the same time with temperature control. The Bioscreen is also well-adapted for anaerobic cultures because the wells can contain 500 ul mediuum. The CLARIOstar uses standard, 96 well plates and can measure both absorbance and fluorescence. 

* Here is an [example Bioscreen file](https://github.com/actolonen/Analysis_Lab/blob/main/Growth/Data/growthData_Bioscreen.xlsx) in .xlsx format.
* Here is an [example Clario file](https://github.com/actolonen/Analysis_Lab/blob/main/Growth/Data/growthData_Clario.xlsx) in .xlsx format.


## Methods for growth analysis

### Plot growth curves
This [code](https://github.com/actolonen/Analysis_Lab/blob/main/Growth/Code/plotGrowth_means.qmd) and [Quarto notebook](https://github.com/actolonen/Analysis_Lab/blob/main/Growth/Code/plotGrowth_means.html) shows how to visualize growth curves in three steps. The code can be adapted to either Clario or Bioscreen data by selecting one of the "Organize Data" chunks and setting the other to 'eval=F'.
Outputs:

1. Plot growth in each well.
2. Plot treatments means +/-SD.
3. Compare treatment means.

### Calculate growth parameters
In addition to visualizing the growth curves for different treatments, it is also useful to calculate growth paramaters (e.g. intrinsic growth rate, generation time, carrying capacity). The Growthcurver R package as described [here](https://github.com/actolonen/Analysis_Lab/blob/main/Growth/Logistic_Fit/2024.02_growthcurver.md) calculates parameters for growth curves by performing a logistic fit.

This [code](https://github.com/actolonen/Analysis_Lab/blob/main/Growth/Code/plotGrowth_means_growthCurver_Bioscreen.qmd) and [Quarto notebook](https://github.com/actolonen/Analysis_Lab/blob/main/Growth/Code/plotGrowth_means_growthCurver_Bioscreen.html) shows how to calculate growth parameters using GrowthCurver.

