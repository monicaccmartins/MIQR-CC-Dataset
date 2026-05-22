# Feature Specification: ERCP Training Pipeline Baseline

**Feature Branch**: `[001-ercp-training-pipeline]`

**Created**: 2026-05-21

**Status**: Draft

**Input**: User description: "Implement a group-assignment baseline pipeline using the assignment PDF and existing notebook style, with pluggable model training, configurable preprocessing and augmentation, class-imbalance training modes, existing evaluation approach, and Grad-CAM visualization."

## Clarifications

### Session 2026-05-21

- Q: If validation split is missing, what split behavior should be enforced? → A: Strict folder protocol using only existing split folders and no derived validation split.
- Q: What is the minimum model interchangeability scope for now? → A: Boilerplate mock model only for now, real model implementations later.
- Q: What Grad-CAM deliverable depth is required now? → A: Full runnable Grad-CAM pipeline with saved visual outputs.
- Q: Should the baseline include two model profiles now? → A: No, keep only one mock model profile in this phase.
- Q: How should future preprocessing extensibility be handled? → A: Provide a strategy registration mechanism so new preprocessing pipelines can be plugged in easily.

### Session 2026-05-22

- Q: Which notebook scope should the new requirements target? → A: Update only `training/BASELINE_PIPELINE.ipynb` as the source-of-truth baseline.
- Q: How should modular image transforms be configured? → A: Use ordered lists of named preprocessing and augmentation steps with per-step enable flags and per-step augmentation probability.
- Q: Which notebook should be the canonical source for the evaluation section and what evaluation coverage is required? → A: Use `training/DENSENET.ipynb` as the source and evaluate on all samples in the validation set.
- Q: How detailed should the new explanatory text be in the baseline notebook? → A: Add section title markdown cells before major phases and concise transition comments in code cells.
- Q: For the new training API contract, what structure should be mandatory? → A: Introduce a `Trainer` with `train(...)` and `evaluate(...)` methods.
- Q: Which `train(...)` input contract should be required? → A: Require explicit inputs for model, optimizer, iterations or epochs, save directory, model name, and full pipeline/run configurations.
- Q: Which `evaluate(...)` contract should be mandatory? → A: `evaluate(trained_model, eval_loader, run_config)` returns a metrics dictionary with per-class and aggregate metrics.

## User Scenarios & Testing *(mandatory)*

### User Story 1 - Train a baseline model with interchangeable architecture (Priority: P1)

As a student, I want to run one notebook-based baseline training workflow on the provided dataset and swap the classification model without rewriting the training pipeline.

**Why this priority**: A reusable baseline training workflow is the core deliverable for the group assignment and unlocks all other experimentation.

**Independent Test**: Can be fully tested by selecting one supported model, running end-to-end training using the dataset split found in the directory, and producing model outputs and evaluation artifacts.

**Acceptance Scenarios**:

1. **Given** the dataset directory split is available, **When** the user selects a supported model and starts training, **Then** the workflow trains and evaluates without structural notebook changes.
2. **Given** one model has completed, **When** the user switches to another supported model, **Then** the same pipeline runs with unchanged data, preprocessing, augmentation, and evaluation interfaces.

---

### User Story 2 - Configure preprocessing and independent augmentation (Priority: P2)

As a student, I want preprocessing and augmentation to be configured independently so I can compare image conditioning strategies without coupling them to augmentation choices.

**Why this priority**: The assignment emphasizes preprocessing quality and robust augmentation; independence is required for controlled experiments.

**Independent Test**: Can be fully tested by running two experiments where preprocessing remains constant and augmentation changes, and two experiments where augmentation remains constant and preprocessing changes.

**Acceptance Scenarios**:

1. **Given** preprocessing includes resizing, normalization, and CLAHE, **When** the user enables or disables each preprocessing strategy, **Then** the pipeline applies only the selected preprocessing components.
2. **Given** geometric and photometric augmentation options are available, **When** the user changes rotation, zoom, brightness, or blur settings, **Then** augmentation behavior changes without altering preprocessing configuration.
3. **Given** images contain a black border area, **When** zoom augmentation is applied, **Then** only the image content region is zoomed and the final black border area remains unchanged.

---

### User Story 3 - Compare imbalance strategies and inspect critical regions (Priority: P3)

As a student, I want to switch among class-imbalance training modes and generate Grad-CAM visualizations so I can compare class performance and interpret detected critical areas such as strictures or stones.

**Why this priority**: Assignment success depends on both performance comparison and explainability for medically meaningful findings.

**Independent Test**: Can be fully tested by running training with each imbalance mode, evaluating with the existing notebook approach, and generating Grad-CAM outputs for sample predictions.

**Acceptance Scenarios**:

1. **Given** class-imbalance mode is configurable, **When** the user selects normal mode, weighted-loss mode, or uniform-sampling mode, **Then** the training run follows the selected strategy without manual code edits.
2. **Given** a trained model and evaluation samples, **When** Grad-CAM is requested, **Then** a visual map highlights critical image regions used for the prediction.
3. **Given** target classes include Biliary_Leaks, Lithiasis, Stricture, and Normal, **When** evaluation is produced, **Then** results are reported for all configured classes.

---

### Edge Cases

- What happens when one of the required class folders is missing in a split directory?
- How does the workflow behave when a class has too few samples for stable uniform sampling?
- What happens if black-border detection fails on an image with irregular border thickness?
- How are corrupted or unreadable image files handled during loading?
- What happens when CLAHE or blur settings are outside valid bounds?

## Requirements *(mandatory)*

### Functional Requirements

- **FR-001**: System MUST provide a notebook-based training baseline that can execute end-to-end on the dataset located under `training/dataset`.
- **FR-002**: System MUST support interchangeable image-classification models within a common training pipeline interface.
- **FR-002a**: For the current baseline iteration, the model slot MUST be provided as a boilerplate mock profile demonstrating the interface contract, with real model implementations deferred.
- **FR-003**: System MUST provide preprocessing configuration that includes resizing, normalization, and CLAHE as selectable strategies.
- **FR-004**: System MUST allow adding additional preprocessing strategies in future without redesigning the workflow structure.
- **FR-004a**: System MUST provide a preprocessing strategy registration interface so new preprocessing pipelines can be defined and inserted into the workflow with minimal changes.
- **FR-005**: System MUST provide augmentation configuration independent from preprocessing configuration.
- **FR-006**: System MUST support at least rotation, zoom, brightness change, and blur augmentation controls.
- **FR-006a**: Preprocessing MUST be configured as an ordered list of named steps, each with explicit enable or disable control.
- **FR-006b**: Augmentation MUST be configured as an ordered list of named steps, each with explicit enable or disable control and a per-step application probability.
- **FR-007**: System MUST preserve the final black border area around images during zoom augmentation by applying zoom only to the image content region.
- **FR-008**: System MUST provide a training imbalance mode selector with exactly three modes: normal, weighted loss, and uniform sampling for low-probability classes.
- **FR-009**: System MUST use the existing directory split protocol found in the dataset structure for training and evaluation data partitioning.
- **FR-009a**: If a validation directory is not present in the existing split protocol, the baseline MUST NOT derive a new validation split.
- **FR-010**: System MUST use the same evaluation approach currently used in the existing notebook implementation as the default evaluation procedure.
- **FR-010a**: The evaluation implementation in `training/BASELINE_PIPELINE.ipynb` MUST be aligned to the approach used in `training/DENSENET.ipynb`.
- **FR-010b**: Evaluation MUST run across all available samples in the validation split for each run.
- **FR-011**: System MUST generate per-class evaluation outputs covering Biliary_Leaks, Lithiasis, Stricture, and Normal.
- **FR-012**: System MUST provide Grad-CAM visualization output for model predictions to show critical detected areas.
- **FR-012a**: Grad-CAM MUST be runnable end-to-end and save visualization overlays for selected predictions.
- **FR-013**: Users MUST be able to run repeated experiments by changing model choice and pipeline options without rewriting notebook core flow.
- **FR-014**: The implementation scope for this change set MUST target only `training/BASELINE_PIPELINE.ipynb` as the authoritative baseline notebook.
- **FR-015**: The notebook MUST include a markdown section-title cell before each major pipeline phase to describe what the following code block performs.
- **FR-016**: The notebook MUST include concise transition comments in code cells describing the purpose of the next code section.
- **FR-017**: The pipeline implementation MUST define a `Trainer` abstraction that exposes exactly two primary methods: `train(...)` and `evaluate(...)`.
- **FR-018**: `Trainer.train(...)` MUST accept explicit parameters for `model`, `optimizer`, `num_iterations` or `epochs`, `save_dir`, `model_name`, `preprocess_config`, `augmentation_config`, and `run_config`, and MUST return the trained model instance.
- **FR-019**: `Trainer.evaluate(...)` MUST accept `trained_model`, `eval_loader`, and `run_config`, and MUST return a structured metrics dictionary containing per-class metrics and aggregate metrics.

### Key Entities *(include if feature involves data)*

- **Dataset Split**: A predefined folder-based partition of images used for training and evaluation.
- **Class Label**: One of four medical outcome categories: Biliary_Leaks, Lithiasis, Stricture, Normal.
- **Model Profile**: A selectable model option that conforms to the common training interface.
- **Preprocessing Profile**: A set of selected preprocessing operations and parameters applied before augmentation.
- **Preprocessing Profile**: An ordered list of named preprocessing operations with per-step enable flags and parameters, applied before augmentation.
- **Augmentation Profile**: An ordered list of independent augmentation operations with per-step enable flags, per-step application probabilities, and parameters.
- **Imbalance Mode**: A training strategy enum with values normal, weighted_loss, and uniform_sampling.
- **Evaluation Result**: Aggregated outputs from the notebook-aligned evaluation process.
- **Grad-CAM Artifact**: Visualization output indicating image regions that contributed most to predicted class decisions.

## Success Criteria *(mandatory)*

### Measurable Outcomes

- **SC-001**: Users can execute a complete training and evaluation run from the notebook on the provided dataset split without modifying core notebook structure.
- **SC-002**: Users can execute the baseline with one mock model profile while preserving a stable model interface contract that enables future model swaps without rewriting pipeline flow.
- **SC-003**: Users can independently vary preprocessing and augmentation settings and observe configuration changes reflected in run metadata for 100% of test runs.
- **SC-004**: For all runs that enable zoom augmentation, resulting images preserve a consistent black border area while zoom affects only the content region.
- **SC-005**: Users can run all three imbalance modes and obtain comparable per-class evaluation outputs for all four classes.
- **SC-006**: Grad-CAM visualizations are produced for selected predictions and are interpretable by users as localized critical regions.

## Assumptions

- The assignment scope for this feature is limited to baseline pipeline setup and does not include final model selection for best performance.
- The folder-based dataset split already present in the project is the authoritative partition for training and evaluation.
- If no validation folder exists in the provided split, training and evaluation proceed without creating a derived validation set.
- The existing notebook evaluation process is considered acceptable as the baseline evaluation standard.
- The class taxonomy for this feature is fixed to Biliary_Leaks, Lithiasis, Stricture, and Normal.
- Users executing the notebook have access to a compatible environment capable of running training and visualization workflows.
- Real production model integrations are intentionally deferred after the mock-model baseline interface is accepted.
- The baseline notebook for this feature iteration is `training/BASELINE_PIPELINE.ipynb`; other notebooks may remain unchanged in this iteration.
