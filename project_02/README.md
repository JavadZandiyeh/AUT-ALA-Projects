# Project 02 — Denoising Bitcoin Prices with Least Squares

Smooths a noisy Bitcoin price series by solving a **regularized least-squares** problem that penalizes large jumps between consecutive samples. Assignment materials: [LinearAlgebra_PA2.pdf](LinearAlgebra_PA2.pdf).

## Problem

The file `btc_price.npy` holds a price series with large fluctuations. Fitting \(Ix = y\) alone recovers the noisy data. The assignment adds a smoothness penalty so consecutive values of the unknown clean signal \(x\) stay close.

## Mathematical formulation

Given observations \(y \in \mathbb{R}^n\) and first-difference matrix \(D \in \mathbb{R}^{(n-1)\times n}\):

\[
D =
\begin{bmatrix}
1 & -1 & 0 & \cdots & 0 \\
0 & 1 & -1 & \cdots & 0 \\
\vdots & & \ddots & \ddots & \vdots \\
0 & \cdots & 0 & 1 & -1
\end{bmatrix}
\]

minimize

\[
\|Ix - y\|_2^2 + \lambda \|Dx\|_2^2
=
\left\|
\begin{bmatrix}
I \\
\sqrt{\lambda}\, D
\end{bmatrix}
x
-
\begin{bmatrix}
y \\
0
\end{bmatrix}
\right\|_2^2
\]

The stacked system is solved via the normal equations:

\[
\hat{x} = (A^\top A)^{-1} A^\top b,
\quad
A = \begin{bmatrix} I \\ \sqrt{\lambda}\, D \end{bmatrix},
\quad
b = \begin{bmatrix} y \\ 0 \end{bmatrix}.
\]

\(\lambda\) trades fidelity to \(y\) against smoothness. The assignment asks you to try several \(\lambda\) values and judge which curve looks both smooth and faithful to the original trend.

## Implemented functionality

In `main.py`:

1. Load `btc_price.npy`
2. Build identity \(I\) and difference matrix \(D\)
3. Form stacked \(A\) and \(b\) with **\(\lambda = 100\)**
4. Solve the normal equations with `numpy.linalg.inv`
5. Plot \(\hat{x}\) with Matplotlib

### Also present (not identical to `main.py`)

| Artifact | Notes |
|----------|--------|
| `jupyter_p2/Untitled.ipynb` | Step-by-step notebook; uses **\(\lambda = 10000\)** and also plots the raw series |
| `plots.pdf` | Saved plots from experimentation |

So: denoising for a fixed \(\lambda\) is **implemented** in the script; systematic comparison across many \(\lambda\) values is only partially reflected (notebook + PDF), not automated in code.

## Data

| Property | Value |
|----------|--------|
| File | `btc_price.npy` |
| Length | 2500 samples |
| Dtype | `float64` |
| Value range (approx.) | 14 643 – 64 511 |

(Duplicate copy under `jupyter_p2/btc_price.npy` for the notebook.)

## Technology

- Python 3
- NumPy — load data, build matrices, invert \(A^\top A\)
- Matplotlib — `plt.plot` / `plt.show`

## Usage

```bash
cd project_02
python3 main.py
```

Requires `btc_price.npy` in the current working directory. A figure window opens with the denoised series.

Notebook:

```bash
cd project_02/jupyter_p2
jupyter notebook Untitled.ipynb
```

Dependencies: `numpy`, `matplotlib` (see [root README](../README.md)).

## Changing \(\lambda\)

Edit the hard-coded value in `main.py`:

```python
L = 100  # λ; try e.g. 1, 100, 10000
```

Larger \(\lambda\) yields a smoother curve and can oversmooth structure in the price path.

## Design notes

- \(D\) is built row-by-row with `numpy.append` (clear but \(O(n^2)\) construction for \(n = 2500\)).
- The normal equations use an explicit inverse; for larger \(n\), a linear solver (`numpy.linalg.solve`) would be more stable, but this matches a direct course-level approach.

## Limitations

- Single hard-coded \(\lambda\) in `main.py` (assignment encourages manual experimentation).
- Explicit matrix inverse of \(A^\top A\) rather than a more stable solve.
- No command-line arguments, batch plots, or quantitative error metrics in the script.
- No automated tests.

## Files

| File | Description |
|------|-------------|
| `main.py` | Denoising script (\(\lambda = 100\)) |
| `btc_price.npy` | Input price series |
| `jupyter_p2/Untitled.ipynb` | Exploratory notebook (\(\lambda = 10000\)) |
| `LinearAlgebra_PA2.pdf` | Assignment statement |
| `plots.pdf` | Saved visualization results |

## Related

- [Repository overview](../README.md)
- [Project 01 — LU factorization](../project_01/README.md)
- [Project 03 — SVD image approximation](../project_03/README.md)
