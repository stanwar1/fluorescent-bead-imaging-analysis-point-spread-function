# Fluorescent-Bead-Imaging-Analysis-Point-Spread-Function
This repository contains analysis pipelines for quantitative characterization of fluorescent beads imaged using widefield microscopy (Nikon Ti2, 100× oil, NA 1.49).
The workflows include:

- Point Spread Function (PSF) estimation from TIFF images
- Outlier removal using IQR filtering
- Statistical testing (Mann–Whitney U with Holm correction)
- Publication-ready figure generation

---

## Repository Structure
bead-imaging-analysis/
│
├── code/
│ ├── 01_psf_extract_from_tiff.py
│ └── 02_psf_plot_and_stats.py
│
├── requirements.txt
├── LICENSE
└── README.md

---

## 1. PSF Extraction Pipeline

**Script:** `01_psf_extract_from_tiff.py`

### What it does

- Detects fluorescent beads using Laplacian-of-Gaussian (LoG) blob detection
- Rejects aggregates, overlapping beads, saturated beads, and low-SNR detections
- Fits accepted beads with a 2D Gaussian model
- Computes:
  - FWHM_x
  - FWHM_y
  - Optional bead-size corrected FWHM (quadrature subtraction)
- Generates quality control overlays
- Outputs per-image and combined CSV result files

### Key Parameters

- Pixel size (µm/pixel) must be specified
- FWHM calculated as:  
  `FWHM = 2.355 × σ`
- Optional bead size correction:
  Quadrature subtraction of bead-equivalent Gaussian width
- SNR filtering and ellipticity filtering applied
- Background estimated using local annulus

### Outputs

For each image:

- `<image>_psf_results.csv`
- `<image>_psf_rejected.csv`
- `<image>_psf_summary.txt`
- QC overlay images

Combined output:

- `ALL_psf_results.csv`
- `ALL_psf_summary.txt`

---

## 2. PSF Plotting and Statistical Analysis

**Script:** `02_psf_plot_and_stats.py`

### What it does

- Reads per-condition XLSX/CSV files
- Removes outliers using IQR rule (k = 1.5)
- Generates:
  - Scatter plots (FWHM_x vs FWHM_y)
  - Boxplots for FWHM_x and FWHM_y
  - Combined FWHM = (FWHM_x + FWHM_y)/2
- Performs statistical testing:
  - Mann–Whitney U (two-sided)
  - Holm-Bonferroni multiple comparison correction
- Annotates significant comparisons with asterisks

### Statistical Approach

- Non-parametric test (Mann–Whitney U)
- Multiple comparison correction using Holm method
- Significance thresholds:
  - * p < 0.05
  - ** p < 0.01
  - *** p < 0.001

---

## Installation

Install required packages:

Dependencies include:

- numpy
- pandas
- scipy
- matplotlib
- scikit-image
- tifffile
- openpyxl

---

## How to Run

### Step 1 – Extract PSF from TIFF images

Edit input path and pixel size inside:

01_psf_extract_from_tiff.py

### Step 2 – Generate Figures and Statistics

Edit the BASE_DIR and file mapping inside:

02_psf_plot_and_stats.py


---

## Reproducibility Notes

- Pixel size must correspond to the calibrated sample-plane pixel size.
- Outlier removal uses Tukey IQR rule (k = 1.5).
- All statistical tests are two-sided.
- Raw TIFF images are not included in this repository.

---

## Code Availability

The code used for PSF extraction and statistical analysis is publicly available in this repository. The version corresponding to the published manuscript is archived with a DOI (if applicable).

---

## License

This project is released under the MIT License.

---

## Author

Swati Tanwar  
Johns Hopkins University  
Department of Mechanical Engineering


