# Mass Spectrometry Imaging (MSI) Preprocessing & Analysis in Python

A Python workflow for parsing `.imzML` Mass Spectrometry Imaging (MSI) datasets, performing spectral cleaning and baseline correction, aligning centroided peaks across pixels, and reconstructing 2D spatial ion distribution maps.

## Overview

Mass Spectrometry Imaging (MSI) produces high-dimensional hyperspectral data cubes where each spatial pixel $(x, y)$ contains a full mass spectrum ($m/z$ vs. intensity). This repository implements a step-by-step preprocessing and exploration pipeline in [ms_tutorial.ipynb](ms_tutorial.ipynb):

1. **Data Parsing & Coordinate Inspection:**
   - Loads `.imzML` / `.ibd` files using `pyimzML`.
   - Detects continuous vs. processed acquisition modes and computes Total Ion Current (TIC) spatial maps.
2. **Spatial Ion Distribution Mapping:**
   - Extracts 2D ion intensity heatmaps around target $m/z$ values within a $\pm 5\text{–}10\text{ ppm}$ mass-accuracy window.
3. **Spectral Smoothing & Baseline Correction:**
   - **Smoothing:** Savitzky–Golay filtering (`scipy.signal.savgol_filter`).
   - **Baseline Removal:** Asymmetric Least Squares (`pybaselines.Baseline.asls`) and 1D morphological grey-opening / Top-Hat subtraction (`scipy.ndimage.grey_opening`).
4. **Peak Picking & Consensus $m/z$ Alignment:**
   - Converts profile spectra to centroid peaks (`scipy.signal.find_peaks`).
   - Builds a `0.005 Da` consensus $m/z$ grid across all pixels and aligns peaks within $\pm 5\text{ ppm}$ using `np.searchsorted`.
   - Exports the aligned `(pixels × consensus m/z features)` matrix as a memory-efficient `scipy.sparse.csr_matrix` for downstream machine learning and spatial clustering.

## Dependencies

```bash
pip install numpy matplotlib scipy pyimzml pybaselines scikit-image
```

## Usage

1. Place your `.imzML` and corresponding `.ibd` file in the working directory (default filename in notebook: `example.imzML`).
2. Open and run `ms_tutorial.ipynb` in Jupyter Notebook or JupyterLab.

## Author

**Artur Safrastyan**  
- GitHub: [@artursafrastyan-uni](https://github.com/artursafrastyan-uni)  
- OpenReview: [Artur Safrastyan](https://openreview.net/profile?id=~Artur_Safrastyan1)
