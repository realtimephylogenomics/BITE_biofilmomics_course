## Installers and downloads:

### Programs - install locally on Windows/Mac OSX

Fastqc - for checking run QC stats
https://www.bioinformatics.babraham.ac.uk/projects/fastqc/

IGV - for viewing read mapping files produced by bam/samtools
https://igv.org/

RStudio (free one) - for stats and visualisations
https://posit.co/products/open-source/rstudio/?sid=1


### Programs - install through WSL ubuntu:

```
wsl -d Ubuntu

sudo apt-get update && sudo apt-get upgrade -y

sudo apt-get install kraken2 canu spades seqtk 

```

### Text editors

Sublimetext - one text editor
https://www.sublimetext.com/

PyCharm - a different text editor
https://pycharm-community-edition.en.softonic.com/


### Databases and example files:

**Example files:** It will help us to have some small example files to work with:

Navigate to https://github.com/epi2me-labs/wf-bacterial-genomes/blob/master/test_data/fastq/barcode01/subset.fastq.gz

Then to https://figshare.com/articles/online_resource/MinION_run/27048820?file=49256347

The files will be in (for me) `ls  /mnt/c/Users/jp1e18/Downloads/`
e.g. `ls /mnt/c/Users/jp1e18/Downloads/subset.fastq.gz`
`  ls /mnt/c/Users/jp1e18/Downloads/barcode09.fastq.gz`
so therefore
```
zcat /mnt/c/Users/jp1e18/Downloads/subset.fastq.gz | head
zcat /mnt/c/Users/jp1e18/Downloads/barcode09.fastq.gz | head

```

**Kraken2 database:** Ideally, it will also help if they you download the 'standard-8' Kraken2 database from this page https://benlangmead.github.io/aws-indexes/k2. It's 5.5Gb so will take a while to download. You should click on the 'tar.gz' link. Then, in WSL, you need to uncompress this database into a new folder:
```
mkdir standard-8
mv <path to your downloaded KrakenDB, for instance I have /home/jp1e18/k2_standard-8_20250714.tar.gz> standard-8
tar -xzf standard-8 k2_standard-8_20250714.tar.gz
```
From now on you will call `kraken2` by pointing to this folder, e.g.
`kraken2 --db standard-8 /mnt/c/Users/jp1e18/Downloads/barcode09.fastq.gz --report test.kraken`

## Genome assembly

This worksheet goes through a basic genome assembly including QC.



## Using fastqc

Open fastQC by clicking on the .bat file in the fastQC folder. You can use 'File > Open' to look at sequence reads.

## Using kraken 

We can use Kraken for a quick contamination check on our data:
`kraken2 --db standard-8 /mnt/c/Users/jp1e18/Downloads/barcode09.fastq.gz --report test.kraken.report --output test.kraken.output`

Now look at the .report file with `less test.kraken.report`. What do we see? Hint: https://github.com/DerrickWood/kraken2/wiki/Manual#output-formats

## Assembly with Canu

We can try an assembly with Canu (from the quickstart page - https://canu.readthedocs.io/en/latest/quick-start.html):

```
# pacbio library
curl -L -o pacbio.fastq http://gembox.cbcb.umd.edu/mhap/raw/ecoli_p6_25x.filtered.fastq
canu  -p ecoli -d ecoli-pacbio  genomeSize=4.8m  -pacbio pacbio.fastq

# nanopore library (download source data from https://figshare.com/articles/dataset/Ecoli_K12_MG1655_R10_3_HAC/11823087?file=21623145)
canu  -p ecoli -d ecoli-oxford  genomeSize=1.8m maxInputCoverage=100  -nanopore /mnt/c/Users/jp1e18/Downloads/ecolk12mg1655_R10_3_guppy_345_HAC.fastq.gz minInputCoverage=0.5 stopOnLowCoverage=1
```

## Assembly with spades

Quick run (if canu isn't feasible)

```
sudo apt-get install spades
spades.py --test

 * Corrected reads are in /home/jp1e18/spades_test/corrected/
 * Assembled contigs are in /home/jp1e18/spades_test/contigs.fasta
 * Assembled scaffolds are in /home/jp1e18/spades_test/scaffolds.fasta
 * Paths in the assembly graph corresponding to the contigs are in /home/jp1e18/spades_test/contigs.paths
 * Paths in the assembly graph corresponding to the scaffolds are in /home/jp1e18/spades_test/scaffolds.paths
 * Assembly graph is in /home/jp1e18/spades_test/assembly_graph.fastg
 * Assembly graph in GFA format is in /home/jp1e18/spades_test/assembly_graph_with_scaffolds.gfa
```

## Installing Quast

Look at completeness with quast.py (https://sourceforge.net/projects/quast/files/latest/download). You will need to download it and unzip, then navigate to the Quast folder.
```
###download quast
mv /mnt/c/Users/jp1e18/Downloads/quast-5.3.0.tar.gz .
tar -zxf quast-5.3.0.tar.gz
./quast-5.3.0/quast.py -o test-assembly-qc ecoli-oxford/ecoli.contigs.fasta
```

You should have created a report.html in the folder 'test-assembly-qc'. Open it in a web browser.


