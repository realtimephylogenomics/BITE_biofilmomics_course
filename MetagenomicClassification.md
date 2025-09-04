### Classifying metagenomic data

This describes how to classify metagenomic sequence data wity Kraken2, and how to produce a simple barplot.

## Kraken

How to do a basic Kraken run
get DB
https://genome-idx.s3.amazonaws.com/kraken/k2_standard_08_GB_20250714.tar.gz

smaller
 wget https://genome-idx.s3.amazonaws.com/kraken/k2_viral_20250714.tar.gz
unzip
 tar -zxf gunzip k2_viral_20250714.tar.gz

 kraken2 --db k2_viral_20250714 /mnt/c/Users/jp1e18/Downloads/barcode09.fastq.gz --report test.kraken

## Interpret output

Filter by genus. pipe to .csv. set a cutoff

## Plotting with R

Counts for a stacked bar

Convert to relative abundance

Normalise for a PCA

## Pavian

See https://github.com/fbreitwieser/pavian?tab=readme-ov-file

```{r}
if (!require(remotes)) { install.packages("remotes") }
remotes::install_github("fbreitwieser/pavian")
```

