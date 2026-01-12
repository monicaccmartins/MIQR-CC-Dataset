# MIQR-CC-Dataset - Pre-processing 🚀

MIQR-CC is a curated ERCP image dataset (19.018 raw and 19.317 processed images from 1.602 patients) created to support research in automated ERCP analysis and diagnosis. The dataset (and associated metadata) will be published separately - the metadata CSV is not included in this repository and a download link / DOI will be added here once available. This repository contains a Jupyter notebook with the preprocessing code, demonstrating representative images before and after processing (visual examples of the pipeline).

The dataset is intended to provide a benchmark for automatic ERCP analysis and diagnosis of biliary and pancreatic diseases.

## Project overview

This repository contains a Jupyter Notebook (`pre-processing.ipynb`) used to pre-process the MIQR-CC dataset. The notebook includes utilities to:
- detect vertical lines with Hough transforms,
- segment images by vertical boundaries,
- perform simple validity checks on image segments (width, darkness, contrast), and
- visualize representative examples.

## Contents

- `pre-processing.ipynb` - main notebook with detection / segmentation / plotting code
- `metadata.csv` (external) - dataset metadata is published separately and will be linked here when available; the notebook expects `image_type` and `raw_image_path` columns
- image files referenced by `raw_image_path` (not included in this repo).

## Requirements ✅

- Python 3.8+
- Packages:
  - `pandas`
  - `numpy`
  - `opencv-python`
  - `matplotlib`

See `requirements.txt` for a quick install.

## Setup 🔧

1. Create a virtual environment (recommended):

```bash
python -m venv .venv
.\.venv\Scripts\activate    # Windows PowerShell
```

2. Install dependencies:

```bash
pip install -r requirements.txt
```

## Usage 💡

1. Open `pre-processing.ipynb` in Jupyter or VS Code and run the cells.
2. The notebook reads `metadata.csv`, selects rows where `image_type == 'V'` (vertical), and processes the first 50 examples by default.
3. For each image it will:
   - read the image in grayscale,
   - detect vertical lines (`detect_vertical_lines`),
   - segment the image using those lines (`segment_image_by_vertical_lines`),
   - validate each segment (`is_image_valid`), and
   - visualize results (`plot_results`).

### Important notes

- The dataset metadata (`metadata.csv`) is hosted separately (not in this repository); a download link/DOI will appear here when available. Once downloaded, place `metadata.csv` in the repository root or set an environment variable `METADATA_PATH` pointing to its path so the notebook can find it.
- `metadata.csv` must contain `raw_image_path` with valid local paths to the image files or URLs.
- The notebook demonstrates before/after processing examples - it selects `image_type == 'V'` examples and displays original and segmented crops with validity indicators.
- If images fail to open, the notebook will print an error message for that file.
- You can adjust Hough/Canny thresholds and segmentation/validation parameters directly in the helper function calls.

## Functions summary 🔎

- `detect_vertical_lines(image, canny1=30, canny2=150, hough_threshold=120, min_line_length=70, max_line_gap=4, angle_threshold_deg=10)`
  - Returns Canny edges and a list of detected (vertical) lines.

- `segment_image_by_vertical_lines(image, lines, min_width=30)`
  - Uses detected vertical lines to cut image into column segments. Returns a list of image segments.

- `is_image_valid(image, min_width=212, black_percentile=90, contrast_threshold=10)`
  - Runs simple heuristics to decide whether a segment is valid (size, darkness, contrast).

- `plot_results(img, edges, lines, segments)`
  - Plots the original image, Canny edges, Hough overlay, and segmented crops with validity indicators.

## Contributing

If you'd like changes (different default parameters, different filtering logic, or an automated script), open an issue or submit a pull request.

## Contact

If you want this README adjusted (add code examples, badges or a quick-start script), tell me what to include and I will update it.

