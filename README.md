[README.md](https://github.com/user-attachments/files/33124344/README.md)
# AMR Determinant Distribution Across One Health Sources

## Overview

This repository contains an R workflow for analysing and visualising the
distribution of antimicrobial resistance (AMR) determinants across
**human, animal, and environmental sources** in a merged
AMR/MLST/MOB-suite dataset.

The script cleans and classifies source metadata, counts the number of
**unique isolates** carrying each AMR determinant, constructs complete
determinant-by-source matrices, and generates publication-quality
presence/absence and dot-heatmap figures. Several plotting versions are
included, progressing from a simple presence/absence heatmap to compact
and zero-gap publication-style dot heatmaps.

## Main analysis

The workflow:

1.  Imports the merged dataset from `Merged_AMR_MLST_Mobsuitclean.csv` /
    `Merged_AMR_MLST_Mobsuiteclean.csv`.
2.  Converts blank values and `-` entries to missing values.
3.  Classifies isolates into three One Health source categories:
    -   **Human**
    -   **Animal** --- Beef cattle and Chicken
    -   **Environment**
4.  Extracts the AMR determinant and isolate identifier.
5.  Removes incomplete observations.
6.  Counts each isolate only once for each AMR determinant/source
    combination to avoid inflation from duplicate records.
7.  Completes all AMR determinant × source combinations, assigning zero
    where a determinant is not detected.
8.  Orders determinants primarily by the number of One Health source
    categories in which they occur and then by total unique-isolate
    count.
9.  Produces publication-quality figures and summary tables.

## Visualisations

The script contains several versions of the AMR determinant × source
visualisation.

### Presence/absence heatmap

A tiled heatmap showing whether each AMR determinant is detected in each
One Health source. Detected cells are highlighted and labelled with the
number of unique isolates.

### Dot heatmap

A publication-style dot heatmap in which:

-   rows represent **AMR determinants**;
-   columns represent **Human, Animal, and Environment**;
-   dot presence indicates detection;
-   dot size represents the **number of unique isolates** carrying the
    determinant;
-   dot colour represents the **AMR determinant**;
-   source labels are colour-coded;
-   all AMR determinants are retained.

The script also contains compact and zero-gap versions intended to
improve readability when many resistance determinants are present.

## Input data

The analysis expects a merged CSV file in the working directory. The
script uses the following filename variants in different sections:

``` text
Merged_AMR_MLST_Mobsuitclean.csv
Merged_AMR_MLST_Mobsuiteclean.csv
```

For reproducibility, use one consistent filename and update `file_path`
in the script accordingly.

### Required columns

At minimum, the dataset must contain:

``` text
AMLST_Isolation source or host
AMR_Element symbol
Isolate ID
```

Source classification is currently based on the following values:

  Original source/host              One Health category
  --------------------------------- ---------------------
  Human                             Human
  Beef cattle                       Animal
  Chicken                           Animal
  Environment / spelling variants   Environment

Any source not included in these rules is assigned `NA` and excluded
from the source-specific analysis.

## R requirements

The script uses the following R packages:

``` r
library(tidyverse)
library(readr)
library(openxlsx)
library(grid)
```

Install missing packages with:

``` r
install.packages(c("tidyverse", "readr", "openxlsx"))
```

`grid` is distributed with R.

## Running the analysis

Place the R script and merged CSV dataset in the same working directory,
confirm that `file_path` matches the dataset filename, and run the
script in R or RStudio.

Example:

``` r
file_path <- "Merged_AMR_MLST_Mobsuiteclean.csv"
```

The script automatically creates the output directory:

``` text
03_Figures/
```

## Outputs

Depending on the plotting version executed, the workflow produces
publication-quality figures and analysis tables such as:

``` text
03_Figures/
├── Figure_AMR_Determinant_Source_Presence_Absence.pdf
├── Figure_AMR_Determinant_Source_Presence_Absence.png
├── Figure_AMR_Determinant_Source_DotHeatmap.pdf
├── Figure_AMR_Determinant_Source_DotHeatmap.png
├── Figure_AMR_Determinant_Source_DotHeatmap_Final.pdf
├── Figure_AMR_Determinant_Source_DotHeatmap_Final.png
├── Figure_AMR_Determinant_Source_DotHeatmap_Compact.pdf
├── Figure_AMR_Determinant_Source_DotHeatmap_Compact.png
├── Table_AMR_Determinant_Source_Presence_Absence.csv
├── Table_AMR_Determinant_Source_Presence_Absence.xlsx
├── Table_AMR_Determinant_Source_DotHeatmap.csv
├── Table_AMR_Determinant_Source_DotHeatmap.xlsx
└── Table_AMR_Determinant_Source_Unique_Isolates.xlsx
```

PNG figures are saved at **600 dpi**, while PDF outputs provide
publication-quality vector graphics.

## Interpretation

The figures are designed to show the **distribution and overlap of AMR
determinants across One Health reservoirs**. Determinants detected in
more than one source category can be readily identified, while dot size
provides information on how frequently each determinant occurs among
unique isolates.

These visualisations describe **co-occurrence and cross-sector
distribution**. They should not, by themselves, be interpreted as
evidence of transmission direction, horizontal gene transfer, or
physical linkage of an AMR determinant to a specific plasmid.

## Reproducibility notes

-   Counts are based on **unique isolates**, not raw rows.
-   Duplicate isolate--source--determinant combinations are removed
    before counting.
-   Missing source, isolate, or AMR determinant values are excluded.
-   The source order is fixed as **Human → Animal → Environment**.
-   Figure height is adjusted automatically according to the number of
    AMR determinants.
-   Some sections highlight `blaTEM-1` visually for emphasis.
-   The script contains multiple alternative figure versions; users may
    retain only the preferred final plotting block for a streamlined
    repository.

## Suggested repository structure

``` text
repository/
├── README.md
├── AMR_source_analysis.R
├── data/
│   └── Merged_AMR_MLST_Mobsuiteclean.csv
└── 03_Figures/
```

If the dataset contains sensitive or unpublished information, consider
excluding it from the public repository using `.gitignore` and providing
only the R script, data dictionary, and/or a de-identified example
dataset.

## Scientific context

This workflow supports a **One Health AMR genomic surveillance**
framework by comparing resistance determinants detected in isolates from
human, animal, and environmental sources. It is suitable for exploring
cross-sector patterns in genomic AMR data and generating figures and
summary tables for reports, presentations, manuscripts, and
supplementary materials.

## Citation

If this repository accompanies a publication, report, thesis, or
conference presentation, add the corresponding citation here.

``` text
Author(s). Year. Title. Journal/Repository. DOI/URL.
```

## License

Add the license selected for the repository (for example, MIT for code).
If no license is provided, reuse rights are not automatically granted by
GitHub.
