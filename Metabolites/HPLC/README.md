# HPLC at Genoscope-CEA

Here is an image of the Shimadzu HPLC at the Genoscope-CEA

![](https://github.com/actolonen/Analysis_Lab/blob/main/Metabolites/HPLC/2024.03_HPLC.png)

## Manuals

-   [Column oven (CTO-20A)](https://github.com/actolonen/Analysis_Lab/blob/main/Metabolites/HPLC/Manuals/CTO-20A_IM_EN.pdf)
-   [Liquid chromatograph (LC-20AB)](https://github.com/actolonen/Analysis_Lab/Metabolites/blob/main/HPLC/Manuals/LC-20AB_IM_EN.pdf)
-   [Autosampler (SIL-20AC)](https://github.com/actolonen/Analysis_Lab/blob/main/Metabolites/HPLC/Manuals/SIL_20A_IM_VerA_ocr_EN.pdf)
-   [UV-VIS detector (SPD-20A)](https://github.com/actolonen/Analysis_Lab/blob/main/Metabolites/HPLC/Manuals/SPD-20A20AV_IM_EN.pdf)
-   [Aminex HPX-87H column](https://github.com/actolonen/Analysis_Lab/blob/main/HPLC/Metabolites/Manuals/LIT42D.PDF)

## Experimental procedure

-   Here is [Tom's protocol](https://github.com/actolonen/Analysis_Lab/blob/main/Metabolites/HPLC/2024.09_protocolHPLC.pdf) to run the HPLC.
-   Here is [Magali's protocol](https://github.com/actolonen/Analysis_Lab/blob/main/Metabolites/HPLC/2024.10_protocole_HPLC_MB.docx) to run the HPLC.

## Compound detection by HPLC

**Compound detection**: our HPLC has 3 detectors: refractive index (RID), UV 210 nm, and UV 260 nm. Different compounds can be detected with the detectors. The table below shows peak elution times for each compound.

| Compound | Type | RID | UV 210 nm | UV 260 nm |
|:----------------:|:----------------:|-----------------:|-----------------:|-----------------:|
| Lactate | Acid | 15.5 | 15.5  | none |
| Formate | Acid | 16.8 | 16.8 | none |
| Acetate | Acid | 18.25 | 18.25 | none |
| Propionate | Acid | 21 | 21  | none |
| Butyrate | Acid | 25.5 | 25.5 | none |
| Ethanol | Alcohol | 26 | none | none |
| 1-Propanol | Alcohol | 33.5 | none | none |
| 1-Butanol | Alcohol | 45 | none | none |
| Acetone | Ketone | 26.4 | none | 26.4 |
| Glucose | Sugar | 11.2 | none | none |

## Analysis pipeline

**Files required for analysis** 
1. LCD file produced by HPLC.
2. Excel file containing 3 pages
    1. *Plan de plaque* plate map of the HPLC experiment. It is not read by the analysis code.
    2. *Standards* the compounds and their concentration in each standards. This page must have these three columns
      1. Sample: name of the standard
      2. Compound: name of compound in the standard
      3. Concentration_mM: the concentration of the compound in mM in that standard
   3. *Samples* description of samples. This page must have these 3 columns:
          1. Sample: name of the sample. NOTE: the names of the standards samples must be the same as on the "Standards" page.
          2. File: the name of the LCD file.
          3. Description: the description of the sample.
      
**File format conversion**
Our Shimadzu HPLC software (Lab Solutions) outputs the chromatograms as .lcd files. Following [this conversation with Ethan Bass](https://github.com/ethanbass/chromConverter/issues/29#issuecomment-2313702224) the above pipeline was developed using chromConverter to extract chromatograms from .lcd files and print them as .txt files.

**3 steps to visualize chromatograms and quantify compounds**

-   Step 1: [Code](/Code/01_chromatograms_Test.qmd) and [notebook](Code/01_chromatograms_Test.html) to plot HPLC chromatograms using all three detectors. Chromatogram data is extracted from LCD files using [chromConverter](https://cran.rstudio.com/web/packages/chromConverter/index.html). Chromatograms outputed as .txt files and are printed as .png files.
-   Step 2: build linear models (LMs) correlate peak areas and compound concentrations in standard solutions. We will apply this model to calculate compound concentrations based on peak areas in the samples.
    -   STD1 solution: [Code](https://github.com/actolonen/Analysis_Lab/blob/main/Metabolites/HPLC/Code/02_standards_STD1_Test.qmd) and [notebook](https://github.com/actolonen/Analysis_Lab/blob/main/Metabolites/HPLC/Code/02_standards_STD1_Test.html) to correlate peak areas and compound concentration (mM) in standard solution 1 (formate, acetate, lactate, ethanol, glucose).
    -   STD2 solution: [Code](https://github.com/actolonen/Analysis_Lab/blob/main/Metabolites/HPLC/Code/02_standards_STD2_Test.qmd) and [notebook](https://github.com/actolonen/Analysis_Lab/blob/main/Metabolites/HPLC/Code/02_standards_STD2_Test.html) to correlate peak areas and compound concentration (mM) in standard solution 1 (acetone, butyrate, butanol).
    -   STD12 solution: [Code](https://github.com/actolonen/Analysis_Lab/blob/main/Metabolites/HPLC/Code/02_standards_STD1-2_Test.qmd) and [notebook](https://github.com/actolonen/Analysis_Lab/blob/main/Metabolites/HPLC/Code/02_standards_STD1-2_Test.htm) to correlate peak areas and compound concentration (mM) mix of STD1 and STD2.
-   Step 3: [Code](https://github.com/actolonen/Analysis_Lab/blob/main/Metabolites/HPLC/Code/03_quantifyCompounds_Test.qmd) and [notebook](https://github.com/actolonen/Analysis_Lab/blob/main/Metabolites/HPLC/Code/03_quantifyCompounds_Test.html) to use linear models from step 2 to quantify compounds in test samples.



-   [Code](https://github.com/actolonen/Analysis_Lab/blob/main/Metabolites/HPLC/Code/ChromConverter-LabSolutions/2024.08_standards_chromConverter-LabSolutions.QMD) and [notebook](https://github.com/actolonen/Analysis_Lab/blob/main/Metabolites/HPLC/Code/ChromConverter-LabSolutions/2024.08_standards_chromConverter-LabSolutions.html) showing that Lab Solutions .lcd files that are converted to .txt using with Lab Solutions or chromConverter produce similar results.
