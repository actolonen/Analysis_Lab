# Correlating HPLC chromatogram peak areas with compound concentrations using standard solutions.

This [notebook](https://github.com/actolonen/Analysis_Lab/blob/main/Metabolites/HPLC/Standards/2024.08_standards_chromConverter-LabSolutions.html) shows how to identify and quantify peaks from two file formats: .LCD files produced by the Lab Solutions software, and .TXT files exported from Lab Solutions.

## Input files

* [file](https://github.com/actolonen/Analysis_Lab/blob/main/Metabolites/HPLC/Standards/information.xlsx) describing the experimental design and the concentrations of compounds in standard solution, which are injected at 5 concentrations (STD1-5).
* HPLC .LCD files for each HPLC run 
* HPLC .TXT files for each HPLC run

## Output file
[table](https://github.com/actolonen/Analysis_Lab/blob/main/Metabolites/HPLC/Standards/standards_regressions.tsv) showing the linear model parameters (slope, y-intercept) to calculate compound concentrations from HPLC peak areas (compound_mM = slope * peak area + y-intercept);
