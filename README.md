# BIOC 557 – Bioinformatics in Proteomics

Welcome! In this session we will analyse a real mass spectrometry proteomics dataset in R, step by step: from a table of protein intensities to the proteins and biological processes that change between experimental groups.

The data come from a honeybee study of how the presence of a queen and of brood affects worker bees: 4 treatment groups (QRBR, QLBR, QRBL, QLBL) × 3 tissues (head, gut, abdomen) × 5 replicates.

**Please complete the three steps below before class.** Installing packages can take 10–20 minutes, and it is much easier to fix problems before the session than during it.

## 1. Download the materials

1. Click the green **Code** button at the top of this page → **Download ZIP**.
2. Unzip the file somewhere easy to find, e.g. `Documents/bioinformatics_proteomics/`.

You should now have this folder structure:

```
bioinformatics_proteomics/
├── proteomics_data_analysis.Rmd   <- the script we will work through
├── data/
│   ├── data.tsv                   <- protein intensities (proteins x samples)
│   ├── meta.tsv                   <- sample information
│   └── contaminants.tsv           <- list of contaminant proteins
└── figure/                        <- plots will be saved here
```

If there is no `figure/` folder, create an empty one with that name.

## 2. Set up your working directory

The script reads files from `data/` and saves plots to `figure/`, so R needs to be "looking" at the `bioinformatics_proteomics` folder.

1. Open RStudio and open `proteomics_data_analysis.Rmd` (**File → Open File...**).
2. Go to **Session → Set Working Directory → To Source File Location**.
3. Check in the Console:
   ```r
   getwd()                       # should end with /bioinformatics_proteomics
   file.exists("data/data.tsv")  # should print TRUE
   ```

When you click **Knit**, R automatically uses the folder the .Rmd file is saved in, so keep the .Rmd file in the same folder as `data/`.

## 3. Install the required packages

Copy and paste the following into the RStudio **Console** and press Enter. If R asks *"Update all/some/none?"*, type `n`.

```r
# packages from CRAN
install.packages(c("tidyverse", "missForest", "patchwork", "ggrepel",
                   "gprofiler2", "circlize", "BiocManager"))

# packages from Bioconductor
BiocManager::install(c("limma", "ComplexHeatmap", "preprocessCore"))
```

Then check that everything loads:

```r
pkgs <- c("tidyverse", "missForest", "patchwork", "ggrepel", "gprofiler2",
          "circlize", "limma", "ComplexHeatmap", "preprocessCore")
ok <- sapply(pkgs, requireNamespace, quietly = TRUE)
if (all(ok)) message("All packages installed - you are ready!") else
  message("Missing: ", paste(pkgs[!ok], collapse = ", "))
```

If you see **"All packages installed - you are ready!"**, you're all set. If some packages are missing, try installing them again one at a time and read the error message. If it still fails, email me before class with a screenshot of the error.

Note: the GO enrichment step (g:Profiler) needs an internet connection during class.

## What we will cover

| Step | Topic |
|---|---|
| 1 | Import data and quality control: contaminants, missing values, boxplots, PCA |
| 2 | Normalization |
| 3 | Missing value imputation |
| 4 | Differential expression analysis with limma: volcano plot, heatmap |
| 5 | GO term enrichment analysis with g:Profiler |

## Contact

Yuming Shi – ys25@student.ubc.ca
