# Training dataset (fixed split)

This folder contains the train, val, and test sets used in the repository's training runs - they are organized by class.
---

## Folder structure

- `training/dataset/train/<class>/*.png`
- `training/dataset/val/<class>/*.png`
- `training/dataset/test/<class>/*.png`

## Classes

- `Biliary_Leaks`
- `Lithiasis`
- `Normal`
- `Stricture`

## Image counts (current snapshot)

- Train: Biliary_Leaks: 110, Lithiasis: 505, Normal: 197, Stricture: 255
- Val:   Biliary_Leaks: 24,  Lithiasis: 98,  Normal: 59,  Stricture: 53
- Test:  Biliary_Leaks: 17,  Lithiasis: 123, Normal: 43,  Stricture: 84

## Filenames and usage

Filenames follow a pattern that includes the crop size and an image identifier (e.g., `329_329_image3587.png` or `1504_1504_image18855_section_1.png`).


## Notes

- These splits are fixed and included in this repository to ensure reproducibility of the experiments.
- When the dataset DOI is available, a link will be added here
- If you need class counts, stratification details or the code used to generate the split, check `split_dataset.ipynb`.