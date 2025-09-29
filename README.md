# PANORAMA Test Datasets

## Overview

This repository contains test datasets for PANORAMA project CI testing and validation.

**Philosophy**: "As small as possible, as large as necessary" - Test data are minimized but still functional for testing purposes.

## Contents

- **3 Vibrio pangenomes** from GTDB RefSeq PanGBank collection (in `pangenomes/`)
- **9 defense system models** from [Defense Finder](https://github.com/mdmparis/defense-finder) (in `models/`)
- **34 HMM profiles** from [Defense Finder](https://github.com/mdmparis/defense-finder) for model annotation (in `hmms/`) 

## Setup and Usage

### Clone the Repository

```bash
git clone https://github.com/labgem/PANORAMA_test.git
cd PANORAMA_test
```

### Running PANORAMA Commands

1. **Format HMM profiles:**
```bash
panorama utils --hmm hmms -o hmms_formatted/
```

2. **Run annotation:**
```bash
panorama annotation \
--pangenomes pangenomes_list.tsv \
--source defensefinder \
--hmm hmms_formatted/hmm_list.tsv \
--k_best_hit 3 \
--output results/
```

3. **Prepare models and run systems detection:**
```bash
panorama utils --models models/* -o .  --force
panorama systems \
--pangenomes pangenomes_list.tsv \
--models models_list.tsv \
--source defensefinder \
--annotation_sources  defensefinder 
```

4. **Write systems results:**
```bash
panorama write_systems  \
--pangenomes pangenomes_list.tsv \
--output write_systems_outdir \
--models models_list.tsv \
--sources defensefinder \
--projection \
--association all
```

## License
This repository is licensed under the CECILL License.

## Contact
For questions or suggestions regarding this dataset repository, please open an issue.   