# Seq_analysis_R
R scripts to analyze DNA/RNA/protein sequence files

This repository is a collection of R scripts to analyze biological sequence data including DNA, RNA, and protein sequence files.

## findsites_CRISPR.Rmd

This script maps the distribution of Cas protein PAMS across a genome. The FASTA DNA sequence file is downloaded from genbank, converted into a data.frame, and the sequences are searched for PAM sites using regular expressions.
