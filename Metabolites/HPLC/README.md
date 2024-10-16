# HPLC at Genoscope-CEA
Here is an image of the Shimadzu HPLC at the Genoscope-CEA 
 
![](https://github.com/actolonen/Analysis_Lab/blob/main/Metabolites/HPLC/2024.03_HPLC.png)

## Manuals
* [Column oven (CTO-20A)](https://github.com/actolonen/Analysis_Lab/blob/main/Metabolites/HPLC/Manuals/CTO-20A_IM_EN.pdf)
* [Liquid chromatograph (LC-20AB)](https://github.com/actolonen/Analysis_Lab/Metabolites/blob/main/HPLC/Manuals/LC-20AB_IM_EN.pdf)
* [Autosampler (SIL-20AC)](https://github.com/actolonen/Analysis_Lab/blob/main/Metabolites/HPLC/Manuals/SIL_20A_IM_VerA_ocr_EN.pdf)
* [UV-VIS detector (SPD-20A)](https://github.com/actolonen/Analysis_Lab/blob/main/Metabolites/HPLC/Manuals/SPD-20A20AV_IM_EN.pdf)
* [Aminex HPX-87H column](https://github.com/actolonen/Analysis_Lab/blob/main/HPLC/Metabolites/Manuals/LIT42D.PDF)

## Experimental procedure

* Here is [Tom's protocol](https://github.com/actolonen/Analysis_Lab/blob/main/Metabolites/HPLC/2024.09_protocolHPLC.pdf) to run the HPLC.
* Here is [Magali's protocol](https://github.com/actolonen/Analysis_Lab/blob/main/Metabolites/HPLC/2024.10_protocole_HPLC_MB.docx) to run the HPLC.

## Analysis Pipeline: quantify compound concentrations using HPLC
* Step 1: [Code](https://github.com/actolonen/Analysis_Lab/blob/main/Metabolites/HPLC/Code/01_chromatograms_Test.qmd) and [notebook](https://github.com/actolonen/Analysis_Lab/blob/main/Metabolites/HPLC/Code/01_chromatograms_Test.html) to plot HPLC chromatograms for our three detectors for a set of samples. Chromatograms are printed as .png files.
* Step 2: [Code](https://github.com/actolonen/Analysis_Lab/blob/main/Metabolites/HPLC/Code/02_standards_Test.qmd) and [notebook](https://github.com/actolonen/Analysis_Lab/blob/main/Metabolites/HPLC/Code/02_standards_Test.html) to correlate peak areas and compound concentration (mM) using standard solutions.
* Step 3: [Code](https://github.com/actolonen/Analysis_Lab/blob/main/Metabolites/HPLC/Code/03_quantifyCompounds_Test.qmd) and [notebook](https://github.com/actolonen/Analysis_Lab/blob/main/Metabolites/HPLC/Code/03_quantifyCompounds_Test.html) to use standard curves to quantify compounds in a set of samples.
  
## Analysis comparing quantifications by ChromConverter and Lab Solutions

*[Code](https://github.com/actolonen/Analysis_Lab/blob/main/Metabolites/HPLC/Code/ChromConverter-LabSolutions/2024.08_standards_chromConverter-LabSolutions.QMD) and [notebook](https://github.com/actolonen/Analysis_Lab/blob/main/Metabolites/HPLC/Code/ChromConverter-LabSolutions/2024.08_standards_chromConverter-LabSolutions.html) showing that Lab Solutions .lcd files that are converted to .txt using with Lab Solutions or [chromConverter](https://cran.rstudio.com/web/packages/chromConverter/index.html) produce similar results.