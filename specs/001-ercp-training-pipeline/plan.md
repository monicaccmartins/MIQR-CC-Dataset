# Implementation Plan: ERCP Training Pipeline Baseline

**Branch**: `[001-setup-pipeline]` | **Date**: 2026-05-22 | **Spec**: [`spec.md`](specs/001-ercp-training-pipeline/spec.md)

**Input**: Feature specification from [`specs/001-ercp-training-pipeline/spec.md`](specs/001-ercp-training-pipeline/spec.md)

## Summary

Deliver a single authoritative baseline notebook at [`training/BASELINE_PIPELINE.ipynb`](training/BASELINE_PIPELINE.ipynb) with clear sectioned flow, modular ordered preprocessing/augmentation, and a mandatory trainer abstraction exposing [`Trainer.train(...)`](specs/001-ercp-training-pipeline/spec.md) and [`Trainer.evaluate(...)`](specs/001-ercp-training-pipeline/spec.md). Training must accept explicit runtime/config parameters (including model name, save path, optimizer, and run settings), and evaluation must follow DenseNet-style logic over all validation samples while returning structured metrics.

## Technical Context

**Language/Version**: Python 3.10+ (notebook-first workflow)

**Primary Dependencies**: PyTorch, Torchvision, NumPy, OpenCV, PIL, scikit-learn, Matplotlib, Seaborn

**Storage**: Local filesystem artifacts under [`training/models`](training/models)

**Testing**: Notebook smoke-run + metrics/artifact validation against split protocol

**Target Platform**: Linux workstation with optional CUDA

**Project Type**: Notebook-based ML training pipeline

**Performance Goals**: Complete baseline smoke training/evaluation run without structural notebook edits; evaluate all samples in validation split

**Constraints**: Strict folder protocol (`train`/`val`/`test`) with no derived validation split; preserve black border during zoom; single baseline notebook scope

**Scale/Scope**: Four-class classification (Biliary_Leaks, Lithiasis, Stricture, Normal) on assignment dataset splits

## Constitution Check

*GATE: Must pass before Phase 0 research. Re-check after Phase 1 design.*

Current constitution file at [`constitution.md`](.specify/memory/constitution.md) is a placeholder template (no enforceable principles populated). Gate status: **PASS (No active constitutional constraints to violate)**.

Post-design re-check: **PASS** (no active gates defined).

## Project Structure

### Documentation (this feature)

```text
specs/001-ercp-training-pipeline/
├── plan.md
├── research.md
├── data-model.md
├── quickstart.md
├── contracts/
│   └── notebook-pipeline-contract.md
└── tasks.md
```

### Source Code (repository root)
```text
training/
├── BASELINE_PIPELINE.ipynb
├── DENSENET.ipynb
├── MOBILENET.ipynb
├── dataset/
└── models/

specs/001-ercp-training-pipeline/
├── spec.md
├── plan.md
├── research.md
├── data-model.md
├── quickstart.md
└── contracts/
    └── notebook-pipeline-contract.md
```

**Structure Decision**: Keep a single notebook-centric implementation in [`training/BASELINE_PIPELINE.ipynb`](training/BASELINE_PIPELINE.ipynb) with supporting design artifacts under [`specs/001-ercp-training-pipeline`](specs/001-ercp-training-pipeline).

## Complexity Tracking

No constitutional violations identified.
