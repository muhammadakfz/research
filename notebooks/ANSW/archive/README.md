# Finite-temperature magnon spectra

**Disentangling Exchange Frustration and Dzyaloshinskii–Moriya Interactions through Finite-Temperature Magnon Spectra**

This undergraduate poster project compares magnetic spirals produced by exchange frustration and DMI. The main study uses a **2D square lattice with periodic boundaries**. The later comparison has DMI on x bonds only (DMI-x); frustration couples `(x+2, y)`. The reference wavevector is `q0 = pi/4`. Finite-temperature wavevectors, especially for frustration, are not necessarily perfectly matched.

## Original work and extensions

[Original_MC_cells_0-18.ipynb](original/Original_MC_cells_0-18.ipynb) preserves cells 0–18, ending with the CSV save, without changing their code, parameters, plotting style, or saved outputs. The author confirmed the local [_New.ipynb](<Disentangling Exchange Frustration and Dzyaloshinskii–Moriya Interactions through Finite-Temperature Magnon Spectra_New.ipynb>) as the source. That full notebook remains completely unchanged in its original location.

Following the author's stated boundary, cells 0–18 are identified as the original implementation; subsequent cells and the separate calibration, LSWT, ASD, and analysis notebooks are AI-assisted extensions. The original implementation has **DMI along both x and y**, and is distinct from DMI-x.

## Main 2D workflow

Open these notebooks in order. Cell numbers below are zero-based positions.

| Step | Notebook | Purpose |
|---|---|---|
| 1. Monte Carlo → magnetic structure | [DMI-x calibration](DMI_x_Matched_Spiral_Calibration_Clean.ipynb) | Adaptive Gaussian Metropolis MC, DMI-x production, static structure factor, chirality. Cells 16–19 inspect saved production data; the frustration input comes from the original MC work. |
| 2. LSWT | [2D LSWT](LSWT_2D_Full_Lattice_Clean.ipynb) | Dispersion around the ideal spiral; includes numerical checks. It does not require MC snapshots. |
| 3. ASD | [2D ASD](ASD_2D_RTX4080_DMIx_Frustration_Clean.ipynb) | CUDA spin dynamics from saved MC configurations at `T=0.1`; dynamical structure factor. |
| 4. Spectral analysis | [Spectral analysis](ASD_Spectral_Analysis_Clean.ipynb) | Loads the saved `T=0.1` spectrum; LSWT overlays, apparent peak widths, integrated intensity asymmetry. |

For the poster, start with the saved magnetic-structure/chirality outputs in the calibration notebook and these existing figures:

- [2D dispersion](lswt_2d_results/lswt_2d_cuts.pdf).
- [Temperature-dependent spectra](asd_finite_temperature/finiteT_asd_qy0_comparison.pdf) and [peak profiles](asd_finite_temperature/finiteT_asd_peak_profiles.pdf), covering `T=0.1, 0.3, 0.5, 1.0`.
- [Low-temperature LSWT overlay](asd_spectral_analysis/asd_lswt_labframe_overlay.pdf), [peak profiles](asd_spectral_analysis/asd_peak_profiles.pdf), and [intensity asymmetry](asd_spectral_analysis/asd_integrated_intensity_asymmetry.pdf).

These are saved study results, not proof of a definitive mechanism diagnostic. **The targeted-temperature MC and multi-temperature ASD notebooks are empty files**, preserved in `archive/`. Their results exist, but their generating code is missing locally. The populated analysis notebook only covers `T=0.1`.

## Data, history, and running

- `production_dmi_x/`: DMI-x MC progress and final spins at `T=0.1`; pair with `progress_frustration.npz` and `spins_frustration_latest.npy` at this directory's root for ASD.
- `mc_temperature_snapshots/`: saved MC configurations and checks at `T=0.3, 0.5, 1.0`.
- `asd_2d_results/`, `asd_finite_temperature/`, `asd_spectral_analysis/`: spectra, analysis tables, and figures. `calibration_dmi_x/` holds the shorter MC calibration.
- `archive/`: older notebook variants, empty placeholders, and optional 3D LSWT. `lswt_validation/` and `lswt_3d_results/` are auxiliary results. `output/`, `OUTPUT_1M/`, `paper_figures/`, the root CSV/DMI snapshots, and root susceptibility figures remain as historical or auxiliary work; do not relabel them DMI-x.
- `backup/`: a verified ZIP of all 114 files before cleanup, with checksums. [Inspection notes](INSPECTION_NOTES.md) list moves, dependencies, scientific cautions, and verification limits.

Start Jupyter with its working directory set to **this `ANSW` directory** for the main notebooks. The optional 3D notebook expects `archive/` as its working directory. Read saved outputs first. MC production is expensive (`500,000` thermalization and `1,000,000` measurement sweeps per temperature); calibration also runs MC. `RUN_PRODUCTION` is still `True`. ASD runs GPU trajectories, and simulation/plot cells save to existing filenames. Use a separate project copy for intentional reruns.

Requirements: Jupyter, Python compatible with NumPy/Numba, **NumPy 2+** (`np.trapezoid`), SciPy, Matplotlib, pandas, and Numba with `numba.cuda`. MC and CUDA ASD require a compatible NVIDIA GPU and driver. Saved runs report Python 3.10 and an RTX 4080; exact package versions were not recorded. No packages were installed or upgraded during cleanup.
