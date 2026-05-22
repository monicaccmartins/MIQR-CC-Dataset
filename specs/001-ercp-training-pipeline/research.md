# Research: ERCP Training Pipeline Baseline

## Decision 1: Model interchangeability in current phase

- Decision: Implement a strict model interface with a runnable mock model profile only.
- Rationale: Matches clarified scope to establish stable training pipeline contracts before integrating real architectures.
- Alternatives considered:
  - Immediate torchvision integration: rejected for this phase due to clarified deferment.
  - Single hardcoded model: rejected because it weakens interchangeability contract validation.

## Decision 2: Dataset partition protocol

- Decision: Use only split directories present in [`training/dataset`](training/dataset), with no derived validation split.
- Rationale: Prevents split leakage and keeps reproducibility aligned with assignment comparison expectations.
- Alternatives considered:
  - Stratified split from train: rejected by clarification.
  - Cross-validation: deferred to later experimentation phase.

## Decision 3: Border-preserving zoom strategy

- Decision: Detect content region and apply geometric zoom only inside that region, then composite back into unchanged border canvas.
- Rationale: Satisfies requirement that final black frame remains unchanged while content can be zoom-augmented.
- Alternatives considered:
  - Full-frame zoom: rejected due to border distortion.
  - Disable zoom: rejected because zoom is required augmentation.

## Decision 4: Preprocessing and augmentation independence

- Decision: Define independent configuration blocks and execution stages for preprocessing and augmentation.
- Rationale: Enables controlled ablation studies and direct requirement traceability.
- Alternatives considered:
  - Combined transform chain: rejected because it couples concerns and reduces experimental control.

## Decision 5: Imbalance mode control

- Decision: Use enum-like run configuration with `normal`, `weighted_loss`, and `uniform_sampling`.
- Rationale: Direct mapping to clarified requirement and enables clean experiment matrix.
- Alternatives considered:
  - Ad hoc boolean flags: rejected due to ambiguity and invalid combinations.

## Decision 6: Grad-CAM deliverable depth

- Decision: Provide fully runnable Grad-CAM generation with saved overlay artifacts.
- Rationale: Explicitly requested in clarification and needed for explainability deliverable.
- Alternatives considered:
  - Stub-only Grad-CAM: rejected by clarification.

## Decision 7: Trainer API and definition-before-usage notebook structure

- Decision: Require a `Trainer` abstraction with two primary methods (`train(...)` and `evaluate(...)`), define all reusable functions/classes first, and keep orchestration/usage only in final notebook cells.
- Rationale: Matches clarified requirements, improves readability, and makes the training/evaluation contract testable and reusable.
- Alternatives considered:
  - Keep monolithic `train_one_run(...)`: rejected because it hides interface boundaries and mixes concerns.
  - Global-variable-driven evaluation: rejected because it weakens reproducibility and makes API behavior implicit.

## Decision 8: Explicit train/evaluate contracts

- Decision: `Trainer.train(...)` accepts explicit runtime inputs (model, optimizer, iterations/epochs, save dir, model name, preprocessing config, augmentation config, run config) and returns the trained model; `Trainer.evaluate(...)` accepts trained model + eval loader + run config and returns structured metrics.
- Rationale: Enforces explicit dependency injection and prevents hidden coupling to notebook globals.
- Alternatives considered:
  - Single opaque config dictionary only: rejected for reduced discoverability and weaker static readability.
