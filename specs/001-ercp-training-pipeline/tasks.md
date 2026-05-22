# Tasks: ERCP Training Pipeline Baseline

**Input**: Design documents from [`specs/001-ercp-training-pipeline/`](specs/001-ercp-training-pipeline/)

**Prerequisites**: [`plan.md`](specs/001-ercp-training-pipeline/plan.md), [`spec.md`](specs/001-ercp-training-pipeline/spec.md), [`research.md`](specs/001-ercp-training-pipeline/research.md), [`data-model.md`](specs/001-ercp-training-pipeline/data-model.md), [`contracts/notebook-pipeline-contract.md`](specs/001-ercp-training-pipeline/contracts/notebook-pipeline-contract.md)

**Tests**: Include notebook smoke/integration validation tasks because user stories require independent testability.

**Organization**: Tasks are grouped by user story to enable independent implementation and validation.

## Phase 1: Setup (Shared Infrastructure)

**Purpose**: Prepare notebook and design artifacts for implementation.

- [x] T001 Confirm baseline target notebook path and scope in [`training/BASELINE_PIPELINE.ipynb`](training/BASELINE_PIPELINE.ipynb)
- [x] T002 [P] Align planning docs with trainer interface contract in [`specs/001-ercp-training-pipeline/contracts/notebook-pipeline-contract.md`](specs/001-ercp-training-pipeline/contracts/notebook-pipeline-contract.md)
- [x] T003 [P] Verify artifact output directory and naming conventions in [`training/models/`](training/models/)
- [x] T004 Verify strict split availability (`train`/`val`/`test`) in [`training/dataset/`](training/dataset/)

---

## Phase 2: Foundational (Blocking Prerequisites)

**Purpose**: Build core reusable pipeline definitions required by all user stories.

**⚠️ CRITICAL**: No user story implementation should be considered complete until this phase is done.

- [x] T005 Refactor notebook order so all definitions appear before runtime usage cells in [`training/BASELINE_PIPELINE.ipynb`](training/BASELINE_PIPELINE.ipynb)
- [x] T006 [P] Define reusable dataset/class mapping utilities in [`training/BASELINE_PIPELINE.ipynb`](training/BASELINE_PIPELINE.ipynb)
- [x] T007 [P] Define reusable preprocessing and augmentation registries in [`training/BASELINE_PIPELINE.ipynb`](training/BASELINE_PIPELINE.ipynb)
- [x] T008 Implement `Trainer` abstraction skeleton (`train(...)`, `evaluate(...)`) in [`training/BASELINE_PIPELINE.ipynb`](training/BASELINE_PIPELINE.ipynb)
- [x] T009 Implement shared artifact helper utilities (model save, metrics save, plot save) in [`training/BASELINE_PIPELINE.ipynb`](training/BASELINE_PIPELINE.ipynb)

**Checkpoint**: Foundation complete; user stories can be implemented/tested incrementally.

---

## Phase 3: User Story 1 - Train a baseline model with interchangeable architecture (Priority: P1) 🎯 MVP

**Goal**: Train via explicit trainer inputs and evaluate with validation-split coverage without editing notebook structure.

**Independent Test**: One execution path trains a mock model using explicit config inputs and returns structured evaluation metrics over all validation samples.

### Tests for User Story 1

- [x] T010 [P] [US1] Add smoke cell validating explicit `Trainer.train(...)` call contract in [`training/BASELINE_PIPELINE.ipynb`](training/BASELINE_PIPELINE.ipynb)
- [x] T011 [P] [US1] Add smoke cell validating `Trainer.evaluate(...)` structured return shape in [`training/BASELINE_PIPELINE.ipynb`](training/BASELINE_PIPELINE.ipynb)

### Implementation for User Story 1

- [x] T012 [US1] Implement explicit-parameter `Trainer.train(...)` flow (model, optimizer, epochs/iterations, save_dir, model_name, configs) in [`training/BASELINE_PIPELINE.ipynb`](training/BASELINE_PIPELINE.ipynb)
- [x] T013 [US1] Implement evaluation over all validation samples in `Trainer.evaluate(...)` in [`training/BASELINE_PIPELINE.ipynb`](training/BASELINE_PIPELINE.ipynb)
- [x] T014 [US1] Return structured metrics dictionary (per-class + aggregate) from `Trainer.evaluate(...)` in [`training/BASELINE_PIPELINE.ipynb`](training/BASELINE_PIPELINE.ipynb)
- [x] T015 [US1] Persist trained model with explicit `save_dir` and `model_name` handling in [`training/BASELINE_PIPELINE.ipynb`](training/BASELINE_PIPELINE.ipynb)

**Checkpoint**: US1 runs end-to-end independently.

---

## Phase 4: User Story 2 - Configure preprocessing and independent augmentation (Priority: P2)

**Goal**: Support ordered modular preprocessing/augmentation with step toggles and augmentation probabilities.

**Independent Test**: Two runs vary preprocessing only, and two runs vary augmentation only, with resulting behavior reflecting selected ordered steps.

### Tests for User Story 2

- [x] T016 [P] [US2] Add validation cells for ordered preprocessing step enable/disable behavior in [`training/BASELINE_PIPELINE.ipynb`](training/BASELINE_PIPELINE.ipynb)
- [x] T017 [P] [US2] Add validation cells for per-step augmentation probability behavior in [`training/BASELINE_PIPELINE.ipynb`](training/BASELINE_PIPELINE.ipynb)

### Implementation for User Story 2

- [x] T018 [US2] Implement ordered preprocessing execution pipeline from config steps in [`training/BASELINE_PIPELINE.ipynb`](training/BASELINE_PIPELINE.ipynb)
- [x] T019 [US2] Implement ordered augmentation execution pipeline with `enabled` and `prob` per step in [`training/BASELINE_PIPELINE.ipynb`](training/BASELINE_PIPELINE.ipynb)
- [x] T020 [US2] Implement/verify border-preserving zoom augmentation in modular step form in [`training/BASELINE_PIPELINE.ipynb`](training/BASELINE_PIPELINE.ipynb)
- [x] T021 [US2] Add concise transition comments and phase markdown around preprocessing/augmentation sections in [`training/BASELINE_PIPELINE.ipynb`](training/BASELINE_PIPELINE.ipynb)

**Checkpoint**: US2 is independently configurable and testable.

---

## Phase 5: User Story 3 - Compare imbalance strategies and inspect critical regions (Priority: P3)

**Goal**: Execute all imbalance modes and produce Grad-CAM artifacts with per-class reporting.

**Independent Test**: Runs for all three imbalance modes produce comparable per-class metrics and save Grad-CAM artifacts.

### Tests for User Story 3

- [x] T022 [P] [US3] Add validation cells verifying mode switch behavior (`normal`, `weighted_loss`, `uniform_sampling`) in [`training/BASELINE_PIPELINE.ipynb`](training/BASELINE_PIPELINE.ipynb)
- [x] T023 [P] [US3] Add Grad-CAM smoke validation cell saving sample artifact in [`training/BASELINE_PIPELINE.ipynb`](training/BASELINE_PIPELINE.ipynb)

### Implementation for User Story 3

- [x] T024 [US3] Implement weighted-loss branch wiring in `Trainer.train(...)` in [`training/BASELINE_PIPELINE.ipynb`](training/BASELINE_PIPELINE.ipynb)
- [x] T025 [US3] Implement uniform-sampling branch wiring in `Trainer.train(...)` in [`training/BASELINE_PIPELINE.ipynb`](training/BASELINE_PIPELINE.ipynb)
- [x] T026 [US3] Ensure evaluation output includes per-class metrics for all four target classes in [`training/BASELINE_PIPELINE.ipynb`](training/BASELINE_PIPELINE.ipynb)
- [x] T027 [US3] Save Grad-CAM overlays and metadata artifact references in [`training/models/`](training/models/)

**Checkpoint**: US3 independently functional and verifiable.

---

## Phase 6: Polish & Cross-Cutting Concerns

**Purpose**: Final consistency and execution-readiness checks across stories.

- [x] T028 [P] Update user-facing execution steps for trainer API in [`specs/001-ercp-training-pipeline/quickstart.md`](specs/001-ercp-training-pipeline/quickstart.md)
- [x] T029 [P] Verify data-model and contract terminology alignment (`Trainer`, metrics dictionary fields) in [`specs/001-ercp-training-pipeline/data-model.md`](specs/001-ercp-training-pipeline/data-model.md)
- [x] T030 Run final notebook smoke flow and capture output checklist notes in [`specs/001-ercp-training-pipeline/quickstart.md`](specs/001-ercp-training-pipeline/quickstart.md)

---

## Dependencies & Execution Order

### Phase Dependencies

- **Phase 1 (Setup)**: No dependencies.
- **Phase 2 (Foundational)**: Depends on Phase 1; blocks all user stories.
- **Phase 3 (US1)**: Depends on Phase 2.
- **Phase 4 (US2)**: Depends on Phase 2 and can proceed after US1 scaffold is stable.
- **Phase 5 (US3)**: Depends on Phase 2 and US1 train/evaluate flow.
- **Phase 6 (Polish)**: Depends on completion of target user stories.

### User Story Dependencies

- **US1 (P1)**: First deliverable; MVP scope.
- **US2 (P2)**: Independent configuration layer, integrates into US1 pipeline.
- **US3 (P3)**: Uses US1 train/evaluate core plus imbalance/Grad-CAM extensions.

### Parallel Opportunities

- Setup tasks marked `[P]` can run in parallel.
- Foundational tasks [`T006`](specs/001-ercp-training-pipeline/tasks.md) and [`T007`](specs/001-ercp-training-pipeline/tasks.md) can run in parallel after [`T005`](specs/001-ercp-training-pipeline/tasks.md).
- In each user story, validation tasks marked `[P]` can run in parallel.
- Polish document updates marked `[P]` can run in parallel.

---

## Parallel Example: User Story 1

```bash
Task: T010 Add smoke cell validating explicit Trainer.train(...) call contract in training/BASELINE_PIPELINE.ipynb
Task: T011 Add smoke cell validating Trainer.evaluate(...) structured return shape in training/BASELINE_PIPELINE.ipynb
```

---

## Implementation Strategy

### MVP First (US1)

1. Complete Phase 1 + Phase 2.
2. Complete Phase 3 (US1).
3. Validate US1 independently before expanding scope.

### Incremental Delivery

1. Deliver US1 trainer-based baseline.
2. Add US2 modular preprocessing/augmentation controls.
3. Add US3 imbalance + Grad-CAM extensions.
4. Finish with Phase 6 polish.

### Parallel Team Strategy

1. Team completes Setup + Foundational together.
2. Then parallelize by story:
   - Developer A: US1
   - Developer B: US2
   - Developer C: US3
