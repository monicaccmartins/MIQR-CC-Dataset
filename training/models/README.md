# Trained models and artifacts

This folder contains the trained model checkpoints and summary visuals used in the experiments.

## Files

- `deitiii.pth` (+ `deitiii.pth.png`, `deitiii.pth_cm.png`) — DeiT/ViT checkpoint and result visuals.
- `densenet.pth` (+ `densenet.pth.png`, `densenet.pth_cm.png`) — DenseNet checkpoint and visuals.
- `efficientnet_b7.pth` (+ `efficientnet_b7.pth.png`, `efficientnet_b7.pth_cm.png`) — EfficientNet checkpoint and visuals.
- `mobilenet_v2.pth` (+ `mobilenet_v2.pth.png`, `mobilenet_v2.pth_cm.png`) — MobileNet checkpoint and visuals.

## How to load

Example (PyTorch):

```python
import torch
from torchvision import models

# Load checkpoint (adjust model architecture to match)
model.load_state_dict(ckpt)
```
Check the relevant notebook (e.g., `training/DENSENET.ipynb`) for the exact model instantiation and loading logic used during evaluation.

## Notes

- Visual summaries (e.g., confusion matrices) are included as PNG files alongside checkpoints.