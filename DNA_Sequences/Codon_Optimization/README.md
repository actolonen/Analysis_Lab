# DNA sequence codon optimization

Codon usage can vary widely across different species. It is thus important to customize the codon usage in DNA sequences to facilitate proper expression in the host of interest.

## Codon utilization in the organism of interst

To optimizate the codon usage in a sequence for a given organism, we first need a table of codon utilization for that organism. Here is [code](https://github.com/actolonen/Analysis_Lab/blob/main/DNA_Sequences/Codon_Optimization/codon_frequencies.qmd) to read a fasta-formated list of gene sequences and output a codon utilization table.

We applied this code to generate a [codon utilization table for C. phytofermentans](https://github.com/actolonen/Analysis_Lab/blob/main/DNA_Sequences/Codon_Optimization/Data/codons_frequencies_cphy.csv).

## Tools to optimize the codon usage for a sequence of interest.

[Codon harmonizer](https://codonharmonizer.systemsbiology.nl/)
