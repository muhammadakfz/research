# Cleanup and inspection notes — 2026-11-15

The supplied `~/akhyar/research/` path was absent. The actual repository is `~/Documents/research/`; all cleanup changes are confined to `notebooks/ANSW/`. Other repository directories contained only placeholders, apart from filesystem metadata.

## Preservation and changes

Before changing anything, all 114 existing files (96,276,537 bytes) were copied into `backup/before_cleanup_2026-11-15.zip`. Every archived member was verified against its SHA-256 in `backup/before_cleanup_manifest.json`. Extract that ZIP into an **empty separate directory** to recover the original layout without overwriting current files.

The complete `Disentangling Exchange Frustration and Dzyaloshinskii–Moriya Interactions through Finite-Temperature Magnon Spectra_New.ipynb` remains at its original path and is byte-for-byte unchanged. `original/Original_MC_cells_0-18.ipynb` contains its first 19 cells, including their metadata and outputs. No cells were rewritten or added. The cell-18 boundary matches the author's description, ending with `df.to_csv("simulation_results.csv", index=False)`. The author explicitly confirmed the local `_New.ipynb` as the original source; the differently named `_New(1).ipynb` was not present. Authorship is recorded according to that confirmation and boundary, not inferred from style or Git history.

The following files moved into `archive/`, keeping their filenames:

| Notebook | Role and treatment |
|---|---|
| `Disentangling Exchange Frustration and Dzyaloshinskii–Moriya Interactions through Finite-Temperature Magnon Spectra.ipynb` | Earlier two-direction DMI MC; CPU measurements; unchanged bytes. |
| `Disentangling Exchange Frustration and Dzyaloshinskii–Moriya Interactions through Finite-Temperature Magnon Spectra copy.ipynb` | Earlier variant with GPU measurement code and a saved interrupted run; unchanged bytes. It is not an exact duplicate. |
| `LSWT_3D_Full_Lattice_Clean.ipynb` | Optional 3D extension with validation. Only its output folder changed to `../lswt_3d_results`, preserving the existing destination when opened from `archive/`. |
| `MC_Targeted_Temperatures_DMIx_Frustration_Clean.ipynb` | Empty, zero-byte placeholder; unchanged. |
| `ASD_2D_MultiTemperature_RTX4080_Clean.ipynb` | Empty, zero-byte placeholder; unchanged. |
| `lswt.ipynb` | Empty, zero-byte placeholder; unchanged. No populated standalone 1D LSWT notebook was found. |

`Download ASD_2D_RTX4080_DMIx_Frustration_Clean.ipynb` was renamed to `ASD_2D_RTX4080_DMIx_Frustration_Clean.ipynb`.

Only seven literal replacements were made across four existing notebooks:

- ASD and spectral analysis: one Linux-specific root path each became `Path.cwd()`.
- DMI-x calibration: two Linux-specific input paths became relative paths; the two model colors in its LSWT cut became `#C00032` and `#0055CC`.
- Archived 3D LSWT: the one output-directory correction described above.

All functions, loops, numerical formulas, kernels, parameters, cell order, metadata, and saved notebook outputs were otherwise retained. No import cleanup was necessary. The 2D LSWT notebook was not edited. **All 103 non-notebook files, including every dataset, result, and figure, remain at the same paths with identical bytes.** No figures or simulations were regenerated, and no Git commit, push, or history change was made.

Historical notebooks are preserved for reading. The extracted original MC writes its relative outputs in its own working directory. If revisiting the full mixed-development `_New.ipynb`, keep the `ANSW` working directory to retain its existing relative input paths. Use an isolated copy for reruns because original save calls intentionally remain unchanged.

## Dependencies and missing sources

| Notebook or stage | Existing inputs | Existing outputs / status |
|---|---|---|
| Original `_New.ipynb`, cells 0–18 | Random initial spins; no external input | Root `progress_{model}.npz`, `spins_{model}_latest.npy`, `simulation_results.csv`; original two-direction DMI and x-only frustration. |
| Original `_New.ipynb`, later AI-assisted cells | Root spin snapshots; in-memory MC arrays | Root susceptibility figures, `paper_figures/`, embedded structure/chirality outputs. These retain the original DMI model. |
| DMI-x calibration | No external input for MC; cells 16–19 use `production_dmi_x/{progress_dmi.npz,spins_dmi_latest.npy}` and root `spins_frustration_latest.npy` | `calibration_dmi_x/`, `production_dmi_x/`; embedded static structure factor, chirality, LSWT cut. |
| 2D LSWT | None; constructs its own ideal spiral | `lswt_2d_results/`; analytic dispersion, finite-difference and time-domain validation. |
| 2D ASD | `production_dmi_x/spins_dmi_latest.npy`, root `spins_frustration_latest.npy`; associated progress files check `T=0.1` | `asd_2d_results/asd_dynamical_structure_factor_T0p1.npz` and figures. |
| Spectral analysis | The above `T=0.1` spectrum, including its parameter and energy arrays | `asd_spectral_analysis/`: figures, `asd_peak_table.csv`, `asd_analysis_summary.npz`. |
| Targeted-temperature MC / multi-temperature ASD | Producer notebooks are zero bytes | Saved `mc_temperature_snapshots/` and `asd_finite_temperature/` data and figures exist; generation cannot be reproduced from those files. |
| 3D LSWT | None | Root `lswt_3d_results/`; optional, outside the main finite-temperature 2D study. |
| Older LSWT validation | No populated producer was found | `lswt_validation/lswt_validation_data.npz` and figures retained; line-grid validation data are auxiliary, not a separate production study. |

All required inputs of the populated notebooks exist under the documented working directories. `production_frustration/spins_frustration_latest.npy` is absent, but it is only an optional ASD fallback: the first-choice root frustration file exists. It was not created or substituted.

The three empty placeholders were already empty when first added in the available Git history. No missing producer code was invented. No separate populated statistical-analysis or convergence notebook was found. The existing spectral notebook includes peak finding and asymmetry; numerical diagnostics also appear inside the LSWT and ASD notebooks.

The three thermodynamic CSVs are distinct and were all retained: root `simulation_results.csv` covers `T=3 → 0.1`; `OUTPUT_1M/simulation_results.csv` covers `3 → 0.2`; `output/simulation_results.csv` covers `0.2 → 3`. The last file's producer is not established. These should not be treated as duplicate copies or combined automatically.

## Scientific points to discuss — no implementations changed

1. **Different Hamiltonians and wavevectors.** In `_New.ipynb`, DMI is on both x and y bonds, with `D = sqrt(2)*tan(q0)`. Saved output reports a peak at `(-pi/4, -pi/4)`, whose magnitude is `sqrt(2)*pi/4`. The final calibration/LSWT/ASD use x-bond DMI only, with `D = J*tan(q0)`; the calibration's saved production peak is `(-pi/4, 0)` at `T=0.1`. Both use ferromagnetic nearest-neighbor exchange in x and y; frustration adds `+J2 S(x,y)·S(x+2,y)`. Preserve the implemented signs. Root DMI files and historical thermodynamic figures must not be substituted for `production_dmi_x/` or relabeled DMI-x.
2. **Matching is nominal, not guaranteed at every temperature.** The ideal LSWT reference uses `q0=pi/4`, and the saved calibration comparison matches the dominant low-temperature peaks. This does not establish exact matching at all temperatures. The frustrated ordering wavevector can vary with temperature. Targeted MC files do not store measured ordering wavevectors; their snapshots remain available for later analysis. No new wavevector calculation was run in this cleanup.
3. **Temperature and dynamics.** The populated ASD notebook evolves thermal MC initial configurations with deterministic, undamped Landau–Lifshitz dynamics, normalized RK4 stages, and no thermostat. Its `T` label describes the initial ensemble. Do not describe it as a thermostatted finite-temperature trajectory. The missing multi-temperature source prevents independent confirmation that it followed exactly the same procedure.
4. **Peak widths and asymmetry remain preliminary.** Saved ASD spectra use four trajectories per model, 512 samples, `dt=0.025`, and sampling every five steps. Their angular-frequency spacing is about `0.09817 J`. The analysis subtracts the time mean, uses a Hann window, then smooths peak profiles with Gaussian `sigma=0.8` bins before estimating widths. These are apparent widths, affected by observation time, windowing, smoothing, and sampling; they do not establish intrinsic lifetimes. Integrated positive-frequency intensity asymmetry is not by itself a demonstration of nonreciprocal dispersion. The populated analysis has no uncertainty estimates for these quantities.
5. **Observable and random-seed interpretation.** `Kai` is the variance of the magnitude of total magnetization divided by `N*T`, not a component-resolved susceptibility or a spiral structure-factor order parameter. GPU seeds are recorded in code, but `random_spin_field()` uses unseeded host NumPy randomness; `batch[0]` inherits that field. Those GPU seeds alone do not reproduce the entire initial state. Neither convention was changed.

## Lightweight verification

Notebook JSON and Python syntax were checked without executing notebook code. All populated notebooks parsed and their Python cells compiled; `!nvidia-smi` was recognized as an IPython shell command. The existing ASD plotting labels produce two invalid-escape warnings under the available Python 3.14; they were left unchanged. Empty `.ipynb` files are preserved placeholders and cannot be opened as valid notebooks.

NPY headers, NPZ member names/shapes and small metadata arrays, and CSV headers/ranges were inspected using the Python standard library. The main spectra all have shape `(256, 64, 64)` with the expected `J=1`, `D≈1`, `J2≈0.353553`, `B_z=0` metadata. Low-temperature spin files have shape `(32, 64, 64, 3)`. Targeted MC files instead have shape `(4, 32, 64, 64, 3)`: do not pass them directly to code expecting the four-dimensional low-temperature format.

Targeted MC metadata records `100,000` thermalization sweeps, `160,000` measurement sweeps, four saved snapshots, and spacing `40,000`; this records what was saved, without verifying the absent implementation. Saved ASD energy arrays show maximum recorded relative drifts below `6.2e-5` across the four temperatures. This is a read-only diagnostic, not a new integration or convergence test.

The current interpreter lacks NumPy, Numba, SciPy, Matplotlib, pandas, and nbformat. Runtime imports, notebook schema validation with nbformat, CUDA compilation, and numerical execution were not verified. No dependency installation was attempted. Checksums verify file preservation, not scientific validity.

Final checks passed: all 114 original files are accounted for, with 110 byte-identical and only the four notebooks listed above edited. All 103 non-notebook files retain their original paths and checksums. The extracted 19 cells equal the original cell objects exactly, including outputs and metadata. Nine populated notebooks parse; 143 Python cells compile, with five additional `!nvidia-smi` shell cells recognized separately. The seven distinct required input files exist, expected spectrum keys/shapes and low-temperature progress metadata agree with their consumers, and the archived 3D output path resolves to the existing results directory. No required data path remains broken under the documented working directories.
