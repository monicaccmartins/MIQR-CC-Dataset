# MIQR-CC Dataset

This repository contains the preprocessing code and training notebooks used for work with the MIQR-CC dataset. When the paper and the dataset are published, links/DOI and the bibliographic reference will be added here.

---

## Overview

Endoscopic Retrograde Cholangiopancreatography (ERCP) is a key procedure in the diagnosis
and treatment of biliary and pancreatic diseases. Artificial Inteligence has being pointed as one solution to automatize diagnosis. However, public ERCP datasets are scarce, which limits the use of such approach. Therefore, the MIQR-CC Dataset proposes filling this gap by providing a large and curated dataset. The collection is composed of 19.018 raw images and 19.317 processed from 1.602 patients. 5.519 cases are labeled, which provides a ready to use dataset. 

The utility and validity of the dataset is proven by a experiment. This collection aims to provide or contribute for a benchmark in automatic ERCP analysis and diagnosis of biliary and pancreatic diseases.

This repository contains:

- `preprocessing/` - Jupyter notebook and utilities that demonstrate the image preprocessing pipeline.
- `training/` - Notebooks used to create train/val/test splits and to train multiple image classification models.

## How to cite (placeholder)

*Paper:* TBD - will be added when available

*Dataset DOI / Download link:* TBD - will be added when available

Suggested citation template (details will be replaced when available):

> Author A, Author B, et al. "MIQR-CC: A curated ERCP image dataset for automated analysis." Journal / Repository, YEAR. DOI: <ADD_DOI_HERE>

## Quickstart

1. Read about preprocessing in `preprocessing/README.md` and inspect `preprocessing/preprocessing.ipynb`.
2. For training, read `training/README.md` and open the notebooks in `training/`.
3. When the dataset metadata / ZIP is available, place `metadata.csv` and image files in a local path and set the environment variable `METADATA_PATH` (or point notebooks directly to the files).

## License

The license for the dataset and code will be added when the dataset is published; a `LICENSE` file will be included and referenced here.
