# Data Model: ERCP Training Pipeline Baseline

## Entity: DatasetSplit

- Description: Folder-based partition of samples used directly by the pipeline.
- Fields:
  - `split_name`: train | val | test
  - `split_path`: relative dataset path
  - `class_folders`: list of class folder names
  - `sample_count`: total image files in split
- Validation rules:
  - `split_path` must exist for configured split
  - class folders must include expected labels when required by run mode

## Entity: ClassLabel

- Description: Classification label domain.
- Allowed values:
  - Biliary_Leaks
  - Lithiasis
  - Stricture
  - Normal
- Validation rules:
  - label names must match folder names used by split protocol

## Entity: ModelProfile

- Description: Pluggable model slot contract used by training loop.
- Fields:
  - `profile_name`
  - `profile_type` mock_only
  - `input_shape`
  - `num_classes`
- Validation rules:
  - must expose forward-compatible training interface

## Entity: PreprocessingProfile

- Description: Independent preprocessing configuration.
- Fields:
  - `resize_enabled`
  - `resize_target`
  - `normalize_enabled`
  - `normalize_stats`
  - `clahe_enabled`
  - `clahe_params`
- Validation rules:
  - invalid CLAHE params must fail fast with clear message

## Entity: AugmentationProfile

- Description: Independent augmentation configuration.
- Fields:
  - `rotation_range`
  - `zoom_range`
  - `brightness_range`
  - `blur_range`
  - `border_preserve_enabled`
- Validation rules:
  - zoom must preserve black border canvas
  - ranges must stay within safe bounds

## Entity: ImbalanceMode

- Description: Enum-style training control.
- Allowed values:
  - normal
  - weighted_loss
  - uniform_sampling
- Validation rules:
  - exactly one mode active per run

## Entity: EvaluationResult

- Description: Outputs from notebook-aligned evaluation.
- Fields:
  - `run_id`
  - `per_class_metrics`
  - `aggregate_metrics`
  - `confusion_matrix_artifact`
  - `classification_report`

## Entity: Trainer

- Description: Primary training/evaluation orchestration abstraction.
- Fields:
  - `save_dir`
  - `model_name`
  - `run_config`
- Methods:
  - `train(model, optimizer, num_iterations|epochs, save_dir, model_name, preprocess_config, augmentation_config, run_config) -> trained_model`
  - `evaluate(trained_model, eval_loader, run_config) -> EvaluationResult`
- Validation rules:
  - all required train inputs must be explicit (no hidden global dependency)
  - evaluate must return structured per-class and aggregate metrics

## Entity: GradCAMArtifact

- Description: Explainability visualization output.
- Fields:
  - `image_id`
  - `predicted_label`
  - `overlay_path`
  - `heatmap_strength_summary`
