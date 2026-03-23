# Automated Experimental Aero-Data Pipeline (Supersonic PIV)

[![Python](https://img.shields.io/badge/Python-3.8+-blue.svg)](https://www.python.org/)
[![Pandas](https://img.shields.io/badge/Data%20I%2FO-Pandas-green.svg)](https://pandas.pydata.org/)
[![NumPy](https://img.shields.io/badge/Matrix%20Computing-NumPy-orange.svg)](https://numpy.org/)
[![Matplotlib](https://img.shields.io/badge/Visualization-Matplotlib-lightblue.svg)](https://matplotlib.org/)

## The Engineering Challenge
Experimental data from Particle Image Velocimetry (PIV) in high-speed aerodynamics is inherently noisy and fragmented across thousands of temporal snapshots. Manually inspecting these files to identify critical flow structures—like shock waves and sonic lines—is highly inefficient.

This repository contains a **custom Python data pipeline** designed to autonomously ingest, reshape, and statistically process 1,000 discrete PIV datasets of an under-expanded supersonic jet ($M = 1.0$). The pipeline programmatically extracts stable turbulence metrics and identifies complex aerodynamic features.

### Analytics Impact & Scale
* **Data Ingestion:** Automated the parsing and structural reshaping of 1,000 raw CSV files into 3D NumPy arrays ($N_x \times N_y \times t$).
* **Statistical Convergence:** Systematically compared results at $N=100$ vs $N=1000$ samples to quantify the exact sample size required to resolve fine-scale shear layer turbulence (RMS convergence).
* **Algorithmic Feature Extraction:** Replaced manual "eyeballing" with programmatic gradient-tracking to pinpoint exact shock wave locations.

---

## Core Analytics Modules

### 1. Batch Processing & Statistical Convergence
Experimental fluid dynamics requires rigorous statistical averaging. The pipeline efficiently computes:
* **Time-Averaged Fields:** Extracts the stable mean velocity components ($\bar{U}, \bar{V}$) from chaotic temporal data.
* **Turbulence Intensity (RMS):** Computes root-mean-square fluctuations ($U_{rms}, V_{rms}$) via vectorized array operations to map turbulent kinetic energy distribution.
* **Convergence Verification:** Proves that while mean flow stabilizes rapidly ($N=100$), resolving shear layer RMS structures mandates higher-order sampling ($N=1000$).

### 2. Physics-Based Feature Extraction
Instead of just plotting data, the code applies aerodynamic theory to find critical structures:
* **Algorithmic Shock Detection:** Utilizes discrete spatial differentiation (`np.argmax` on RMS profiles) to autonomously detect the maximum gradient, accurately pinpointing the physical location of the stand-off shock wave.
* **Sonic Curve Mapping:** Scans the 2D velocity field to isolate and plot the exact coordinates where the local flow matches the calculated sonic speed ($U/U_{sonic} \approx 1$).

### 3. Turbulence Profiling
Maps flow distortion by extracting precise 1D profiles across specific radial coordinates:
* **Centerline Tracking ($y/d = 0$):** Captures velocity fluctuations driven by the potential core and shock cells.
* **Shear Layer Tracking ($y/d = 0.5$):** Highlights the intense turbulence amplification caused by shock-shear layer interactions.

---

## Extracted Physics & Visualizations

### 1. Algorithmic Sonic Curve & Shock Location
The pipeline's automated detection of the $M=1$ boundary (red) and the primary shock location (dashed vertical line), overlaid on the RMS turbulence field.
> ![sonic_curve_and_shock_location](sonic_curve_and_shock_location.png)

### 2. Statistical Convergence (Mean vs. RMS)
Side-by-side comparison proving that 1,000 samples are required to properly resolve the turbulent shear layers compared to 100 samples.
> ![velocity_statistics](velocity_statistics.png)

### 3. Radial Turbulence Profiling
1D extraction showing the sharp divergence in turbulent kinetic energy between the jet centerline and the highly unstable shear layer.
> ![u_mean_profiles](u_mean_profiles.png)

---

## Tech Stack & Implementation Details
* **Data I/O:** `pandas.read_csv` for rapid ingestion of 1,000 sequential files.
* **Matrix Operations:** `numpy` broadcasting for instantaneous computation of $N$-dimensional variances and means without scalar loops.
* **Physics Normalization:** All spatial dimensions non-dimensionalized by nozzle diameter ($x/d, y/d$) and velocities normalized by local sonic speed ($U_{sonic} = 315.35$ m/s) to ensure industry-standard reporting.
