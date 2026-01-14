# Training / Experiments

This folder contains the notebooks and artifacts for training and evaluating ERCP image classification models using the split defined in the repository.

---

## Notebooks

- `split_dataset.ipynb` - script/notebook used to create and validate the split (`train` / `val` / `test`) - note: fixed splits are included under `training/dataset/`.
- `RESNET.ipynb` - training and evaluation for a ResNet-based model.
- `DENSENET.ipynb` - DenseNet-based training and evaluation.
- `EFICIENTNET.ipynb` - EfficientNet-based experiments.
- `DEITIII.ipynb` - DeiT experiments.
- `MOBILENET.ipynb` - MobileNet-based experiments.
- `models/` - Trained model checkpoints used for experiments (and summary visuals).

Each notebook contains data loading, augmentations, training loop, validation and test evaluation, and plotting (confusion matrix, learning curves).

## Requirements

Install the training environment with:

```bash
python -m venv .venv
.\.venv\Scripts\activate   # Windows PowerShell
pip install -r requirements.txt
```

(`training/requirements.txt` contains the pinned packages used for experiments.)

## Models & Results

Trained checkpoints and summary visuals are under `training/models/` (checkpoints have the `.pth` extension; summary images include confusion matrices `.png`). See `training/models/README.md` for details on how to load them.

## Reproducibility notes

- Fixed splits are included in `training/dataset/` (no need to re-run `split_dataset.ipynb` unless you want a different split).
- Check each notebook for hyperparameters and random-seed usage.

## TODO / Placeholders

- Add citation/DOI for the dataset and the article when available.

## Contact

Open an issue for questions or requests.