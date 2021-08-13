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
├── README_FIRST.md    <- This file
├── data
│   ├── raw            <- Original data as provided.
│   ├── interim        <- Any interim transformations performed;
│   │                     Also contains HTML files from pandas_profiling.
│   ├── processed      <- Final, processed file(s) after cleansing
│   │   │                 and transformation; also contains train, test,
│   │   │                 validation split CSVs used by all models.
│   │   └── ...al_test <- Nested under DBLP_Scholar > active_learning,
│   │                     due to some early experiments, became the directory
│   │                     for all Active Learning models (TODO: fix).
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
followed by that for the de-duplication of Cora. We describe the main code
sections for the DBLP-Scholar notebook, although the majority will apply to the
other notebooks.

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

The same applies for **Blocking** as the blocked candidate files are available
in the directory. The code can be run to see the performance if required.

**Labelling** uses the existing blocked candidate files, and the reader can skip
 ahead directly to this step, without any of the previous data transformation or
 processing ones. Additionally, unless a different seed or proportion for
 sampling is desired, the cell for creating split CSV files under
 **Create train, validation, test data** should *not* be run. The main block
 under this section involves using `dm.data.process` to perform attribute
 embeddings as described in the report. This *must* be run for the models to be
 trained, or if test files are required for predicting on loaded models.

**Predict on unlabelled data** is admittedly messy code, but was used to explore
 to distribution visualisations (to validate the use of 0.5 as a threshold), and
 to calculate F-beta and AUC scores.

**ML models (with Magellan)** has some annoyances due to the repeated steps of
generating ''internal'' features and imputation for train, validation, and test
data sets.

**Active learning** contains a number of helper functions, not all of which have
 been used. **Back-up torchtext object list before mutations** is the most
 important cell to run here, and must only be done so for each run-time since
 my implementation involves mutating the objects (this is a definite TODO: fix).
 There are two sections for *entropy* and *random* training runs: the *entropy*
 block must be re-used for the sub-strategies based on *biased* and *balanced*
 selection as described in the report. This is done by setting the `strategy`
 variable. **Evaluation and plotting** use run-time dataframes, but some changes
 could (TODO: fix) be made to import the persisted metrics .csv files.

**Initialise weights with Transfer Learning (from DBLP-ACM)** implements
Deep Transfer Active Learning.

Finally, a large amount of old ''boilerplate'' code can be found in the
appropriately named section. Some of it is has been superceded, and there are a
few useful items in there. Personally, this could be safely deleted now that we
are at the end of the project.
