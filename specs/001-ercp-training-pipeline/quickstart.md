# Quickstart: ERCP Baseline Notebook Workflow

## 1. Prepare workspace

1. Ensure dataset exists under [`training/dataset`](training/dataset).
2. Open [`training/BASELINE_PIPELINE.ipynb`](training/BASELINE_PIPELINE.ipynb) as the authoritative baseline notebook.

## 2. Configure run

1. Set class list exactly to Biliary_Leaks, Lithiasis, Stricture, Normal.
2. Set model profile to the current mock boilerplate profile.
3. Configure preprocessing options for resize, normalize, CLAHE.
4. Configure augmentation options independently for rotation, zoom, brightness, blur.
5. Enable border-preserving zoom policy.
6. Select imbalance mode enum.

## 3. Execute pipeline

All definitions (configs, transforms, dataset/model/trainer helpers) should be declared before the final orchestration cells.

1. Load split folders as provided by directory protocol.
2. Build or pass explicit runtime configs (preprocess, augmentation, run config).
3. Instantiate `Trainer` and call `train(model, optimizer, num_iterations|epochs, save_dir, model_name, preprocess_config, augmentation_config, run_config)`.
4. Evaluate with `evaluate(trained_model, eval_loader, run_config)` over all validation samples.
5. Generate Grad-CAM artifacts and save overlays.

## 4. Validate outputs

1. Confirm per-class outputs exist for all four classes.
2. Confirm black frame consistency under zoomed samples.
3. Confirm Grad-CAM overlay files are saved.

## 5. Reproducibility and run metadata

1. Record run configuration (imbalance mode, preprocessing strategy, augmentation settings) for each execution.
2. Keep artifact naming consistent via explicit `model_name` and `save_dir` passed to trainer.
3. Persist per-class metric snapshots for Biliary_Leaks, Lithiasis, Stricture, and Normal.

## 6. Final artifact checklist

- Notebook exists and runs: [`training/BASELINE_PIPELINE.ipynb`](training/BASELINE_PIPELINE.ipynb)
- Baseline model weights saved: [`training/models/baseline_mock_model.pth`](training/models/baseline_mock_model.pth)
- Grad-CAM sample saved: [`training/models/gradcam_sample.npy`](training/models/gradcam_sample.npy)
- Dataset split respected from [`training/dataset`](training/dataset)
- Border-preserving zoom enabled in augmentation config
