# PANORAMA Testing Dataset

## Overview

This repository contains datasets used by our developed tool in GitHub workflows. The datasets are essential for testing, validation, and demonstration purposes within our continuous integration and deployment pipelines.

## Dataset

### Description

The dataset is composed of Vibrio species presenting more than 50 complete genomes in NCBI databases: 
RefSeq and Genbank, at the date of 26.06.2025.

| Taxid | species                   | #complete genomes |
|-------|---------------------------|-------------------|
| 666   | *Vibrio_cholerae*         | 614               |
| 670   | *Vibrio_parahaemolyticus* | 280               |
| 663   | *Vibrio_alginolyticus*    | 107               |
| 669   | *Vibrio_harveyi*          | 69                |
| 47951 | *Vibrio_cyclitrophicus*   | 64                |
| 672   | *Vibrio_vulnificus*       | 63                |
| 680   | *Vibrio_campbellii*       | 51                |

### Files list

| Dataset | Description | Usage |
 |---------|-------------|-------|

### Reproduce the Dataset

First, we download the genomes based on the assembly summary with [genome_updater](https://github.com/pirovc/genome_updater)

```shell
# You must install genome_updater first
```

### Update the dataset

The update of the dataset must be done with precaution, because it will change the expected results.

### Update the genomes
```shell

# You should install genome_updater before
# 662 is the Vibrio Genus Taxid
mkdir -p Genomes/Vibrio
cd Genomes
# Download the genomes
genome_updater.sh -T 662 -M ncbi -o Vibrio -f 'assembly_report.txt' -l 'complete genome' -d 'refseq,genbank' -t 10
# Search the species with more than 50 genomes
cut -d$'\t' -f7,8 Vibrio/assembly_summary.txt | sed s"/Vibrio /Vibrio\_/"| sed 's/sp. /sp./' | sed -e "s/\(^[0-9]*\tVibrio\_[a-zA-Z0-9_.]*\) .*/\1/" | awk -v min_genomes=50 -F '\t' ' {c[$0]++} END{for (i in c) if (c[i]>=min_genomes) printf("%s\t%s\n",i,c[i])}' | sort -k3 -rn > select_species
# Download the genomes for each species
while read taxid sp nb_genomes;
do 
  mkdir $sp
  genome_updater.sh -T $taxid -M ncbi -o $sp -f 'genomic.gbff.gz' -l 'complete genome' -d 'refseq,genbank' -t 10
  [ $(ls -1 $sp/*/files/ | wc -l) -ne $nb_genomes ] && echo "The number of download genomes is incorrect."
done < select_species.tsv

#Update the pangenomes by following the next part
```

### Update the pangenomes

If you update the genomes before, follow the command below, else you can go to the next command.

## License
This repository is licensed under the CECILL License. By contributing to this repository, you agree to license your contributions under the same license.

## Contact
For questions or suggestions regarding this dataset repository, please open an issue or contact the repository maintainers.