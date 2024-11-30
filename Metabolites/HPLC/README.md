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

## Analysis Pipeline: quantify compound concentrations using HPLC

This analysis pipeline requires that all the .lcd file for an HPLC run are put in a directory along with an [information file](https://github.com/actolonen/Analysis_Lab/blob/main/Metabolites/HPLC/Data/information_HPLC_Test.xlsx) describing the files and the concentrations of the standards used for peak quantification.

**Compound detection**: our HPLC has 3 detectors: refractive index (RID), UV 210 nm, and UV 260 nm. Different compounds can be detected with the detectors:

-   Acids: detect by RID and UV 210 nm

    -   Formate: 16 min

    -   Acetate: 18 min

    -   Lactate: 15 min

    -   Butyrate: 25 min

-   Alcohols: RID

    -   Ethanol: 26 min

    -   Butanol: 44 min

-   Ketones: RID and UV 260 nm

    -   Acetone: 26 min

## 3 steps to visualize chromatograms and quantify compounds

-   Step 1: [Code](https://github.com/actolonen/Analysis_Lab/blob/main/Metabolites/HPLC/Code/01_chromatograms_Test.qmd) and [notebook](https://github.com/actolonen/Analysis_Lab/blob/main/Metabolites/HPLC/Code/01_chromatograms_Test.html) to plot HPLC chromatograms using all three detectors. Chromatogram data is extracted from .lcd files using [chromConverter](https://cran.rstudio.com/web/packages/chromConverter/index.html) and chromatograms are printed as .png files.
-   Step 2: build linear models (LMs) correlate peak areas and compound concentrations in standard solutions. We will apply this model to calculate compound concentrations based on peak areas in the samples.
    -   STD1 solution: [Code](https://github.com/actolonen/Analysis_Lab/blob/main/Metabolites/HPLC/Code/02_standards_STD1_Test.qmd) and [notebook](https://github.com/actolonen/Analysis_Lab/blob/main/Metabolites/HPLC/Code/02_standards_STD1_Test.html) to correlate peak areas and compound concentration (mM) in standard solution 1 (formate, acetate, lactate, ethanol, glucose).
    -   STD2 solution: [Code](https://github.com/actolonen/Analysis_Lab/blob/main/Metabolites/HPLC/Code/02_standards_STD2_Test.qmd) and [notebook](https://github.com/actolonen/Analysis_Lab/blob/main/Metabolites/HPLC/Code/02_standards_STD2_Test.html) to correlate peak areas and compound concentration (mM) in standard solution 1 (acetone, butyrate, butanol).
    -   STD12 solution: [Code](https://github.com/actolonen/Analysis_Lab/blob/main/Metabolites/HPLC/Code/02_standards_STD1-2_Test.qmd) and [notebook](https://github.com/actolonen/Analysis_Lab/blob/main/Metabolites/HPLC/Code/02_standards_STD1-2_Test.htm) to correlate peak areas and compound concentration (mM) mix of STD1 and STD2.
-   Step 3: [Code](https://github.com/actolonen/Analysis_Lab/blob/main/Metabolites/HPLC/Code/03_quantifyCompounds_Test.qmd) and [notebook](https://github.com/actolonen/Analysis_Lab/blob/main/Metabolites/HPLC/Code/03_quantifyCompounds_Test.html) to use linear models from step 2 to quantify compounds in test samples.

## File format convversion

Our Shimadzu HPLC software (Lab Solutions) outputs the chromatograms as .lcd files. Following [this conversation with Ethan Bass](https://github.com/ethanbass/chromConverter/issues/29#issuecomment-2313702224) the above pipeline was developed using chromConverter to extract chromatograms from .lcd files and print them as .txt files.

-   [Code](https://github.com/actolonen/Analysis_Lab/blob/main/Metabolites/HPLC/Code/ChromConverter-LabSolutions/2024.08_standards_chromConverter-LabSolutions.QMD) and [notebook](https://github.com/actolonen/Analysis_Lab/blob/main/Metabolites/HPLC/Code/ChromConverter-LabSolutions/2024.08_standards_chromConverter-LabSolutions.html) showing that Lab Solutions .lcd files that are converted to .txt using with Lab Solutions or chromConverter produce similar results.
