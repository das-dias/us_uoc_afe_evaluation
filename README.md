# Ultrasound-on-Chip Analog Frontend (UoC-AFE) Monte Carlo Analysis

> Simulation-driven evaluation framework for image-quality-constrained ultrasound-on-chip (UoC) analog frontend design and compressive multiplexing architectures.

---

## Overview

This repository contains the Jupyter notebooks, simulation workflows using [PyMUST](https://github.com/creatis-ULTIM/PyMUST), and analysis scripts used in the research study:

**“Image-Quality-Constrained Miniaturization of High Temporal-Resolution Diagnostic Ultrasound Integrated Analog Frontends”**

The work investigates how analog frontend (AFE) non-idealities impact ultrasound imaging quality in highly miniaturized wearable ultrasound-on-chip systems. The repository focuses on two complementary research directions:

1. **Image-quality-constrained AFE miniaturization**

   * Determining how much AFE specifications can be relaxed without compromising diagnostic image quality.
   * Evaluating the sensitivity of B-mode and Color-Doppler imaging to analog impairments.

2. **Compressive multiplexing for channel/data reduction**

   * Studying analog-mixed-signal charge-sharing compressive multiplexing (CS-CMUX) architectures.
   * Assessing how switched-capacitor non-idealities affect reconstructed RF data and final image quality.

The simulation framework is based on **Monte Carlo analysis** using **PyMUST**, enabling systematic exploration of statistical non-idealities across multiple ultrasound imaging scenarios.

---

# Research Objectives

The repository supports the following research goals:

* Evaluate the effect of:

  * Analog noise
  * Harmonic distortion
  * Gain mismatch
  * Time-gain compensation (TGC) quantization
  * Charge injection
  * Clock feed-through
  * Clock jitter
  * Capacitor mismatch

* Quantify image degradation using:

  * SSIM (Structural Similarity Index)
  * gCNR (generalized Contrast-to-Noise Ratio)
  * MSE (Mean Squared Error)

* Explore image-quality-driven specification relaxation for:

  * Low-noise amplifiers (LNAs)
  * TGC stages
  * Switched-capacitor compressive multiplexers

* Evaluate tradeoffs between:

  * Imaging fidelity
  * Power consumption
  * Data throughput
  * Channel count reduction
  * Mixed-signal hardware complexity

---

# Repository Structure

```text
.
├── afe_charge_sharing_cmux_monte_carlo_eval.ipynb
├── afe_evaluation_monte_carlo.ipynb
├── cmux_eval_vs_fs.csv
├── cmux_evaluation_monte_carlo.ipynb
├── differential_afe_charge_sharing_cmux_monte_carlo_eval.ipynb
├── hello.py
├── mc_results_afe_eval_color_doppler.csv
├── mc_results_afe_eval.csv
├── mc_results_cmux_afe_eval.csv
├── mc_results_cmux_afe_fs_search.csv
├── mc_results_cmux_afe_lambda_search.csv
├── mc_results_cs_cmux_nonideal_color_doppler.csv
├── mc_results_cs_cmux_nonideal.csv
├── pyproject.toml
└── README.md
```

> The exact repository organization may evolve as the study progresses.

---

# Simulation Framework

## Ultrasound Imaging Pipeline

The implemented simulation flow follows:

1. **Medium Definition**

   * Scatterer distribution
   * Reflectivity configuration
   * Anechoic and hyperechoic targets

2. **Transducer Modeling**

   * Fully-addressed linear array
   * Divergent wave transmission
   * Configurable center frequency and bandwidth

3. **RF Data Acquisition**

   * PyMUST-based acoustic simulation
   * Multi transmit-receive cycle acquisition

4. **AFE Modeling**

   * Ideal frontend
   * Noisy frontend
   * Nonlinear frontend
   * CS-CMUX frontend

5. **Signal Processing**

   * TGC
   * IQ demodulation
   * Beamforming
   * Delay-and-sum reconstruction
   * Log compression

6. **Image Quality Evaluation**

   * SSIM
   * gCNR
   * MSE
   * Visual analysis

---

# Imaging Modalities

## B-Mode Imaging

The B-mode evaluation setup includes:

* Divergent-wave imaging
* Anechoic and hyperechoic inclusions
* Variable scatterer diameters
* Compound imaging
* Envelope detection via Hilbert transform
* 40 dB log compression

### Evaluated Metrics

* Structural Similarity Index (SSIM)
* generalized Contrast-to-Noise Ratio (gCNR)
* Mean Squared Error (MSE)

---

## Color Doppler Imaging

The Doppler framework evaluates:

* Correlation-based axial velocity estimation
* Rotating disk phantom
* Multi-angle divergent-wave acquisition
* IQ-domain velocity reconstruction

### Focus of Analysis

* Velocity map degradation
* Temporal coherence sensitivity
* Jitter susceptibility
* Compression artifact robustness

---

# Analog Frontend (AFE) Non-Idealities

The repository models several analog impairments relevant to miniaturized ultrasound systems.

## Noise

Input-referred white noise is injected into the AFE model and evaluated through output SNR degradation.

The implemented model follows:

y(t)=A_vx(t)\pm\sigma_n(t),\quad \mathrm{SNR}=\frac{P_y}{P_n}

---

## Harmonic Distortion

Second- and third-order nonlinearities are modeled using polynomial distortion terms.

y(t)=A_vx(t)+A_{v,2}x^2(t)+A_{v,3}x^3(t)+O(\epsilon)

---

## Gain Error

AFE gain mismatch is modeled statistically using Gaussian perturbations.

A'*v=10^{(G+Z_G)/20},\quad Z_G=\Delta G*{dB}\cdot\mathcal{N}(1,0)

---

# Charge-Sharing Compressive Multiplexing (CS-CMUX)

This repository also contains block-level modeling and Monte Carlo evaluation of a switched-capacitor charge-sharing compressive multiplexer architecture for ultrasound channel compression.

## Modeled Non-Idealities

### Charge Injection

\epsilon_i[n]=\frac{\Delta V}{V_{DD}}=\left(1-x[n]-\frac{V_{th}}{V_{DD}}\right)\frac{C_{ox}}{2C_H}

### Clock Feed-through

\epsilon_{ck}=\frac{\Delta V_{CK}}{V_{DD}}=\left(\frac{V_{CK}}{V_{DD}}\right)\frac{C_{ov}}{C_{ov}+C_H}

### Clock Jitter

\epsilon_j[n]\approx \Delta T_{jit}\left(\frac{d}{dt}x(t)\right)[n]\leq 2A\pi f_0\Delta T_{jit}

### Capacitor Mismatch

\sigma_C\left(\frac{\Delta C}{C_H}\right)\approx \frac{K_C}{\sqrt{WL}}

---

# Monte Carlo Methodology

Each parameter sweep is evaluated using repeated Monte Carlo runs with Gaussian-distributed perturbations.

Typical workflow:

* Select non-ideality
* Sweep degradation parameter
* Perform repeated randomized simulations
* Reconstruct ultrasound images
* Evaluate image metrics
* Aggregate statistical results

The framework supports:

* Statistical sensitivity analysis
* Design-space exploration
* Hardware-aware imaging evaluation
* Cross-domain analog/imaging co-design

---

# Dependencies

Core dependencies include:

* Python 3.10+
* NumPy
* SciPy
* Matplotlib
* scikit-image
* Jupyter
* PyMUST

Install using:

```bash
[uv] pip install -r requirements.txt
```

or:

```bash
conda env create -f environment.yml
```

Example workflow:

```text
1. Generate RF channel data using PyMUST
2. Apply AFE impairment model
3. Reconstruct B-mode or Doppler image
4. Compute SSIM/gCNR/MSE
5. Aggregate Monte Carlo statistics
```

---

# Key Research Contributions

This work introduces:

* An image-quality-driven methodology for AFE specification definition
* Monte Carlo evaluation of ultrasound analog frontend impairments
* A framework for assessing imaging sensitivity to mixed-signal hardware non-idealities
* A block-level CS-CMUX architecture for compressive ultrasound acquisition
* Quantitative analysis of channel compression versus imaging fidelity tradeoffs

---

# Intended Audience

This repository may be useful for researchers and engineers working in:

* Ultrasound imaging
* Ultrasound-on-chip systems
* Analog/mixed-signal IC design
* Biomedical ASICs
* Compressive sensing
* Switched-capacitor circuits
* Medical imaging reconstruction
* Hardware-aware computational imaging

---

# Citation

If you use this repository or build upon this work, please cite the associated journal publication once available. For now, this repository can be directly cited through:

```bibtex
@misc{us_afe_min_img_eval_workflow,
  title = {{Image-Quality-Constrained Miniaturization of High Temporal-Resolution Diagnostic Ultrasound Integrated Analog Frontends - Simulation Setups and Workflow}},
  author  = {Dias, Diogo},
  note = {[Online]. Available: \url{https://github.com/das-dias/us_uoc_afe_evaluation}},
  year    = {2026}
}
```

---

# Status

> This repository is currently under active development and accompanies an ongoing journal submission.

The notebooks, APIs, and directory structure may change as the work evolves.

---

# License

Specify the intended license here.

Example:

```text
MIT License
```

---

# Acknowledgements

* PyMUST / MUST ultrasound simulation toolbox
* Open-source scientific Python ecosystem
* Researchers and collaborators contributing to ultrasound-on-chip miniaturization research
