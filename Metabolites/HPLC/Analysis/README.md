# Quantification of compounds from HPLC data


**Files required for analysis** 
* LCD file produced by the HPLC
* Excel file (see [example](Data/information_HPLC_DATE.xlsx)) containing 3 pages:
1. <ins>Plan de plaque</ins> plate map of the HPLC experiment. It is not read by the analysis code.
2. <ins>Standards</ins> the compounds and their concentration in each standards. This page must have these three columns
    1. *File* name of the LCD file
    2. *Sample* name of the standard
    3. *Compound* name of compound in the standard
    4. *Concentration_mM* the concentration of the compound in mM in that standard
3. <ins>Samples</ins> description of samples. This page must have these 3 columns:
    1. *File* the name of the LCD file.
    2. *Description* the description of the sample. NOTE: the *Sample* name of the standards samples on the <ins>Standards</ins> page must exactly match the description in this column.
            
Our Shimadzu HPLC software (Lab Solutions) outputs the chromatograms as .lcd files. Following [this conversation with Ethan Bass](https://github.com/ethanbass/chromConverter/issues/29#issuecomment-2313702224) the above pipeline was developed using chromConverter to extract chromatograms from .lcd files and print them as .txt files.

**3 steps to visualize chromatograms and quantify compounds**

-   Step 1: [Code](/Code/01_chromatograms_Test.qmd) to plot HPLC chromatograms using all three detectors. Chromatogram data is extracted from LCD files using [chromConverter](https://cran.rstudio.com/web/packages/chromConverter/index.html). Chromatograms outputed as .txt files and are printed as .png files.
-   Step 2: build linear models (LMs) correlate peak areas and compound concentrations in standard solutions. We will apply this model to calculate compound concentrations based on peak areas in the samples.
    -   STD1 solution: [Code](https://github.com/actolonen/Analysis_Lab/blob/main/Metabolites/HPLC/Code/02_standards_STD1_Test.qmd) and [notebook](https://github.com/actolonen/Analysis_Lab/blob/main/Metabolites/HPLC/Code/02_standards_STD1_Test.html) to correlate peak areas and compound concentration (mM) in standard solution 1 (formate, acetate, lactate, ethanol, glucose).
    -   STD2 solution: [Code](https://github.com/actolonen/Analysis_Lab/blob/main/Metabolites/HPLC/Code/02_standards_STD2_Test.qmd) and [notebook](https://github.com/actolonen/Analysis_Lab/blob/main/Metabolites/HPLC/Code/02_standards_STD2_Test.html) to correlate peak areas and compound concentration (mM) in standard solution 1 (acetone, butyrate, butanol).
    -   STD12 solution: [Code](https://github.com/actolonen/Analysis_Lab/blob/main/Metabolites/HPLC/Code/02_standards_STD1-2_Test.qmd) and [notebook](https://github.com/actolonen/Analysis_Lab/blob/main/Metabolites/HPLC/Code/02_standards_STD1-2_Test.htm) to correlate peak areas and compound concentration (mM) mix of STD1 and STD2.
-   Step 3: [Code](https://github.com/actolonen/Analysis_Lab/blob/main/Metabolites/HPLC/Code/03_quantifyCompounds_Test.qmd) and [notebook](https://github.com/actolonen/Analysis_Lab/blob/main/Metabolites/HPLC/Code/03_quantifyCompounds_Test.html) to use linear models from step 2 to quantify compounds in test samples.



-   [Code](https://github.com/actolonen/Analysis_Lab/blob/main/Metabolites/HPLC/Code/ChromConverter-LabSolutions/2024.08_standards_chromConverter-LabSolutions.QMD) and [notebook](https://github.com/actolonen/Analysis_Lab/blob/main/Metabolites/HPLC/Code/ChromConverter-LabSolutions/2024.08_standards_chromConverter-LabSolutions.html) showing that Lab Solutions .lcd files that are converted to .txt using with Lab Solutions or chromConverter produce similar results.
