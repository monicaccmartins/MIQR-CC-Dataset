# Notebook Pipeline Contract

## Purpose

Define the user-facing configuration contract for the ERCP baseline notebook pipeline.

## Run Configuration Contract

### Required fields

- `dataset_root`: path to dataset split root
- `classes`: ordered class list
- `model_profile`: model slot selection
- `preprocessing_profile`: preprocessing settings
- `augmentation_profile`: augmentation settings
- `imbalance_mode`: one of `normal`, `weighted_loss`, `uniform_sampling`
- `gradcam_enabled`: true or false

### Behavior guarantees

- Pipeline must run with strict folder split only.
- Pipeline must preserve black border area for zoom augmentation.
- Pipeline must output evaluation artifacts for all configured classes.
- Pipeline must save Grad-CAM overlays when enabled.

## Stage Interface Contract

## Trainer Interface Contract

### Required structure

- A `Trainer` abstraction is mandatory with two primary methods:
  - `train(...)`
  - `evaluate(...)`

### `train(...)` contract

- Required inputs:
  - `model`
  - `optimizer`
  - `num_iterations` or `epochs`
  - `save_dir`
  - `model_name`
  - `preprocess_config`
  - `augmentation_config`
  - `run_config`
- Return value:
  - trained model instance

### `evaluate(...)` contract

- Required inputs:
  - `trained_model`
  - `eval_loader`
  - `run_config`
- Return value:
  - structured metrics dictionary containing:
    - per-class metrics
    - aggregate metrics
    - confusion matrix reference
    - classification report reference

### Stage 1: Load

- Input: dataset root and class list
- Output: split-indexed sample collections

### Stage 2: Preprocess

- Input: raw image and preprocessing profile
- Output: preprocessed image

### Stage 3: Augment

- Input: preprocessed image and augmentation profile
- Output: augmented image with preserved border policy

### Stage 4: Train

- Input: stage outputs + explicit `train(...)` contract inputs
- Output: trained model state and run metadata

### Stage 5: Evaluate

- Input: trained state and explicit `evaluate(...)` contract inputs
- Output: structured per-class + aggregate metrics and evaluation artifacts

### Stage 6: Explain

- Input: trained state and selected images
- Output: Grad-CAM overlay artifacts
