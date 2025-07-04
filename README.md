---
title: "SMC MYOCD"
author:
   name: "Dmytro Kryvokhyzha"
   email: dmytro.kryvokhyzha@med.lu.se
   affiliation: LUDC Bioinformatics Unit
date: "23 september, 2021"
output:
  html_document:
    keep_md: true
---

## Publication

[Li et al. 2021. **Regulation of the Muscarinic M3 Receptor by Myocardin-Related Transcription Factors**. _Frontiers in Physiology 12_](https://doi.org/10.3389/fphys.2021.710968)

## PI

Name: [Ola Hansson](https://www.ludc.lu.se/ola-hansson-pi) & 
      [Karl Swärd](https://www.lunduniversity.lu.se/lucat/user/d6a67258cd7d2448a83f44c29cbda4e8)

Email: [ola.hansson@med.lu.se](mailto:ola.hansson@med.lu.se), 
       [karl.sward@med.lu.se](mailto:karl.sward@med.lu.se)

## Project

Differential expression in the cultured human coronary artery smooth muscle cells (SMCs) induced by either the Ad-CMV-MYOCD viral vector.

Smooth muscle cells from the human coronary artery were treated either with *Ad-CMV-null* or *Ad-CMV-MYOCD*, i.e. adenovirsuses that express nothing or myocardin under control of CMV promoter. There were 4 virus induced and 4 control samples:

- CMV - control
- MYO - virus induced.

**Test how overexpression of myocarding affects other genes**.

Expression of myocardin (*MYOCD*) can be used as control.

## Data

The raw data of this project is stored at LUDC and will be made available upon request.

## Prerequisites

Install [Conda](https://conda.io) and load the pre-configured conda
environment. It should also install all the required programs.


```bash
conda env create -f conf/conda.yml
conda activate myocardin
```

Software versions are provided in `conf/conda.yml`.

## Quality Control

Quality Control of the fastq files and other data are summarized in
`results/reports/multiQC`

## Map and count reads

Mapping is performed with [STAR](https://github.com/alexdobin/STAR) in 2-pass mode.

Reads counting is performed with [featureCounts](http://bioinf.wehi.edu.au/featureCounts/).

See the rules in `code/Snakefile`.


```bash
snakemake -s code/Snakefile -p --use-conda
```

Results:

- `results/tables/featureCounts/featureCounts_counts_gene.csv.gz` - gene counts.

## Differential gene expression

Performed with [DESeq2](https://bioconductor.org/packages/release/bioc/vignettes/DESeq2/inst/doc/DESeq2.html):


```bash
R -e 'rmarkdown::render("code/DESeq.Rmd", output_dir="results/reports/")'
```

Results:

- `results/reports/DESeq.html` - report describing the analysis.

- `results/tables/DESeq/DESeq_all.xlsx` - differential expression results.

Column names in `DESeq_all.xlsx`:

- *baseMean* - mean of normalized counts for all samples. 
- *log2FoldChange* - log2 fold change (MLE): condition treatment vs control.
- *lfcSE* - standard error.
- *stat* - Wald statistic.
- *pvalue* - Wald test p-value.
- *padj* - BH adjusted p-values.
- *\_tpm* - transcript per million (TPM) normalized count data.
