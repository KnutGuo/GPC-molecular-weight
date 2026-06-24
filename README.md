# GPC-molecular-weight

A small Python (Jupyter) script that calculates the **number-average molecular weight (Mn)**, **weight-average molecular weight (Mw)**, and **dispersity (Đ = Mw / Mn)** of a single, already-deconvoluted gel-permeation-chromatography (GPC) peak.

This code accompanies the molecular-weight analysis described in the associated manuscript. Multimodal GPC traces are first deconvoluted into individual peaks externally (in Origin); this script takes one such peak and converts it to Mn, Mw, and Đ.

## Contents

| File | Description |
| --- | --- |
| `GPC-molecular-weight.ipynb` | The analysis notebook. |
| `sample_GPC_peak.csv` | Synthetic demo peak (**not real data**) so the notebook runs out of the box. |
| `README.md` | This file. |

## Requirements

- Python 3
- `pandas`, `numpy`, `matplotlib`
- Jupyter Notebook (e.g. via the Anaconda distribution)

Install the libraries with:

```
pip install pandas numpy matplotlib jupyter
```

## How to run

1. Open `GPC-molecular-weight.ipynb` in Jupyter Notebook.
2. Run **Block 1** (Shift + Enter). It loads the data and plots the raw peak. By default it uses the bundled `sample_GPC_peak.csv`.
3. Read the peak start and end times off the plot.
4. In **Block 2**, set `t_start` and `t_end` to those times, then run the cell. It prints `Mw`, `Mn`, and `PDI`, and shows a verification plot of the integration range.

To analyse **your own data**, edit three lines in Block 1 (`file_path`, and the column names `time_col` / `signal_col` if yours differ), and replace the calibration constants `a, b, c, d` in Block 2 with the values for your column and standards.

## Input format

A CSV file with two columns:

| Column | Meaning |
| --- | --- |
| `Time` | Retention (elution) time, in minutes. |
| `Signal` | Concentration-detector signal intensity (e.g. RI). |

Each file should contain **one deconvoluted peak**, with the baseline subtracted so that the signal returns to ~0 on both sides of the peak.

## Calibration and method

Retention time `t` is converted to molecular weight `M` via a third-order polynomial calibration established from narrow molar-mass standards:

```
log10(M) = a + b·t + c·t² + d·t³
```

The concentration-detector signal is proportional to the **mass** `w` of polymer eluting in each slice. The averages are therefore computed as:

```
Mw = Σ(w·M) / Σ(w)
Mn = Σ(w)   / Σ(w / M)
Đ  = Mw / Mn
```

> **Note:** The constants `a, b, c, d` in the notebook are specific to one particular column and one set of standards. They are included only as a worked example and must be replaced with your own calibration.

## How to cite

If you use this code, please cite the archived release (DOI to be added once the repository is archived on Zenodo):

```
DOI: 10.5281/zenodo.20825313
```

## License

Released under the MIT License — see the `LICENSE` file.
