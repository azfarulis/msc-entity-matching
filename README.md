# MSc Project 2: "Neural record linkage and de-duplication"
Entity matching and record de-duplication project with
Amazon Development Centre Scotland

# Objectives
1. Implement blocking
2. Train and compare DL and ML matching models
3. Implement transfer learning in low resource scenarios
4. Implement active learning in low resource scenarios
5. Try a combination of transfer and active learning in low resource scenarios
6. Perform model checks

# Data
Matching publication records from different sources

- DBLP-Scholar: 2,616 and 64,263 records; 5,347 true pairs
- Cora: 1,879 records; 64,578 true pairs
- (Extra) DBLP-ACM: 2,616 and 2,295 records; 2,225 true pairs

# Explanation of directory structure

The root directory is named `MSc-Amazon-Record-Linkage`.

```
├── README.md          <- This file
├── data
│   ├── raw            <- Original data as provided.
│   ├── interim        <- Any interim transformations performed;
│   │                     Also contains HTML files from pandas_profiling.
│   ├── processed      <- Final, processed file(s) after cleansing
│   │                     and transformation; also contains train, test,
│   │                     validation split CSVs used by all models.
│   │   └── ...al_test <- Nested under DBLP_Scholar > active_learning,
│   │                     due to some early experiments, became the directory
│   │                     for all Active Learning models (TODO: future fix).
│   └── cached_data    <- Unused. Intention was to store downloaded fastText
│                         files, which are now stored elsewhere.
│
├── eda                <- Unused. EDA files are found under data > processed >
│                         R EDA, or in named data folders under data > interim.
│
├── models             <- Contains most (but not all) trained models.
│                         DBLP_Scholar > active_learning contains metrics from
│                         Active and Transfer Learning as CSVs.
│                         DBLP_Scholar > imbalance_exploration contains metrics
│                         from all class imbalance models, and data proportions.
│
├── notebooks          <- Contains all .ipynb notebooks from Google Colab,
│                         and a single .Rmd notebook for text mining EDA.
│
├── report             <- Unused since report was written online on Overleaf.
│   └── plots          <- Generated graphics and figures used in report.
```

# Explanation for running code

Due to some challenges with Kaggle, all notebooks were developed on Google
Colab, with a Colab Pro subscription for increased RAM footprint and increased
limitations on running time.

The notebook for record linkage of DBLP-Scholar is the most feature-complete,
followed by that for the de-duplication of Cora.

**Warning**: unfortunately, none of these notebooks can be run end-to-end. They
require step-by-step runs with appropriate changes since some of the
explorations require changing parameter values manually (e.g. Active Learning
  with different entropy selection strategies are run from the same code block
  by setting the `strategy` parameter first; this will overwrite the previous
  runs).

Due to older versions of some libraries (pandas, pandas_profiling), these must
be manually updated. As such, the after the initial imports
(section: **Import libraries**) and installations, the run-time must be
restarted. Versions can be checked with the code in **Verify package versions**.

Generally, all cells under **Data loading** and
**Set seeds for reproducibility** can be run. Profiling in
**Exploratory data analysis** has been commented out as the output HTML files
are available as described in the directory explanation.

All **Data transformations** and **Data pre-processing** can be run, except for
**Replicating joint table for DBLP and Scholar matches** which was an initial
experiment that is no longer required.

Unless persisted data files need to be created, I advise against running any
cells that write to directory. All necessary files (.pkl, .csv,
  .pklmetadata) can be found in the directories and imported directly using or
  re-purposing existing code.
