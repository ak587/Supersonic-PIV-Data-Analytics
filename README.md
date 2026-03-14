# Supersonic Jet Flow Analysis using PIV Data

## Project Overview
This project performs a comprehensive statistical analysis of supersonic jet flow using **Particle Image Velocimetry (PIV)** data. By processing 1,000 temporal samples, the analysis extracts mean velocity fields, turbulence intensities (RMS), and identifies critical flow structures such as **sonic curves** and **shock wave locations**.

The project specifically explores the statistical convergence of flow properties by comparing results derived from 100 samples versus 1,000 samples, highlighting the importance of sample size in turbulent flow characterization.

## Key Features
*   **Large-scale Data Processing:** Automated ingestion and reshaping of 1,000 CSV-based PIV datasets.
*   **Statistical Analysis:** Calculation of Mean and Root Mean Square (RMS) velocities to characterise turbulence.
*   **Fluid Dynamics Characterisation:** 
    *   Calculated exit conditions for a choked converging nozzle ($M_e = 1.0$).
    *   Normalized results using the sonic speed ($U_{sonic} = 315.35$ m/s).
    *   Identified the **Sonic Curve** and the **Shock Location** within the jet shear layer.
* **Advanced Visualisation:** Generated multi-panel contour plots and line profiles using Matplotlib to visualise flow physics.

## Tech Stack
*   **Language:** Python
*   **Libraries:** 
    *   `NumPy`: High-performance matrix operations and statistical calculations.
    *   `Pandas`: Efficient CSV data handling.
    *   `Matplotlib`: Scientific data visualisation (Contourf, Subplots, Multi-axis plotting).

## Analysis & Results

### 1. Velocity Statistics & Convergence
The analysis compares the mean $U$ component and RMS fluctuations across different sample sizes. 
*   **Insight:** While the mean flow stabilises relatively quickly, the RMS ($U_{rms}$) requires more samples (N=1000) to resolve fine-scale turbulent structures in the shear layer.

![Velocity Statistics](velocity_statistics.png)

### 2. Turbulence Profiling (Centerline vs. Shear Layer)
By plotting $U_{rms}$ along the x-axis for different radial positions ($y/d$):
*   **Centerline ($y/d=0$):** Shows fluctuations driven by the potential core and stand-off shocks.
*   **Shear Layer ($y/d=0.5$):** Exhibits higher fluctuation peaks due to intense turbulence and shock-shear layer interactions.

![RMS Profiles](u_mean_profiles.png)

### 3. Shock & Sonic Curve Identification
The project includes a custom algorithm to locate the **Sonic Curve** (where $U/U_{sonic} \approx 1$) and identifies the **Shock Location** by detecting the maximum gradient in the RMS fluctuation profile.

![Sonic Curve and Shock](sonic_curve_and_shock_location.png)

## Project Structure
```text
├── DATA/                       # Raw PIV sample files (CSV)
├── PIV_data_analysis.py        # Main processing and visualisation script
├── velocity_statistics.png     # Output: Mean and RMS contours
├── u_mean_profiles.png         # Output: Centerline vs Shear layer comparison
├── sonic_curve_shock.png       # Output: Physics-based feature extraction
```

## Theoretical Background
The project utilises isentropic flow relations to determine nozzle exit conditions:
*   **Exit Temperature ($T_e$):** Derived from stagnation temperature ($T_0 = 297K$).
*   **Sonic Speed ($U_{sonic}$):** Calculated as $\sqrt{\gamma R T_e}$.
*   **Flow State:** Identified as an **under-expanded jet** based on the calculated exit pressure of $1.796$ atm.

---

### How to Run
1. Ensure Python 3.x is installed.
2. Install dependencies: `pip install numpy pandas matplotlib`
3. Place PIV data in the `/DATA` folder.
4. Run the script: `python PIV_data_analysis.py`
