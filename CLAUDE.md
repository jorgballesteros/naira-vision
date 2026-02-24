# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

**NAIRA Vision** is a Python-based course repository for multispectral image analysis applied to precision agriculture. It covers processing RGB and multispectral images to assess crop health via vegetation indices, segmentation, and basic ML classification. Part of the NAIRA project (AI-driven circular transformation of the agri-food sector).

## Environment Setup

```bash
# Core dependencies
pip install numpy opencv-python matplotlib rasterio scikit-learn pandas

# Optional
pip install geopandas scikit-image
```

Python 3.10+ required. Use `venv` or `conda` for isolation.

## Repository Architecture

The codebase follows a pipeline architecture: raw images → preprocessing → index calculation → visualization/segmentation → ML classification.

```
src/
├── preprocessing/   # Radiometric correction, normalization, noise reduction, band alignment, TIFF/GeoTIFF handling
├── indices/         # Vegetation index calculations: NDVI, GNDVI, SAVI, EVI
├── visualization/   # Vigor maps, false color maps, histogram analysis
├── segmentation/    # Thresholding, K-means clustering, anomaly detection
└── ml/              # Supervised classification models
notebooks/           # Guided practice notebooks per module
data/
├── raw/             # Original multispectral images (RGB + NIR bands)
└── processed/       # Preprocessed datasets
docs/                # Theoretical material organized by module
```

## Key Domain Concepts

- Images have separate spectral bands: Red, Green, Blue, NIR (near-infrared)
- Vegetation indices use band math: e.g., NDVI = (NIR - Red) / (NIR + Red)
- GeoTIFF is the primary raster format; use `rasterio` for reading/writing
- `opencv-python` (cv2) for image processing; `numpy` for band arithmetic
- Pixel values typically float32 in range [0,1] after normalization

## Running Notebooks

```bash
jupyter notebook notebooks/
```

## Linting

```bash
ruff check src/
```
