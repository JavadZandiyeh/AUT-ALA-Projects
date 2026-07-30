# Applied Linear Algebra Projects (AUT)

Course project solutions for **Applied Linear Algebra** (جبر خطی کاربردی) at [Amirkabir University of Technology](https://aut.ac.ir/), taught by Dr. Chehrghani (academic year 1400 / 2021–2022).

Each project is a short Python implementation of a core linear-algebra technique applied to a concrete task: solving linear systems, denoising a time series, and approximating images with truncated SVD.

## Projects

| Project | Topic | Method | Entry point |
|---------|--------|--------|-------------|
| [project_01](project_01/) | Solve multiple \(Ax = b\) systems | LU factorization + forward/back substitution | `LUFactorization.py` |
| [project_02](project_02/) | Denoise Bitcoin price series | Regularized least squares | `main.py` |
| [project_03](project_03/) | Approximate / compress BMP images | Truncated SVD per RGB channel | `Compress_BMP_Files.py` |

## What this repository demonstrates

- Implementing LU factorization and triangular solves from first principles (not only calling a solver)
- Formulating a denoising problem as a stacked least-squares system with a first-difference penalty
- Using SVD truncation for low-rank image approximation and discussing storage savings
- Working with NumPy arrays and Matplotlib for numerical computation and visualization

## Technology stack

| Library | Used for |
|---------|----------|
| [NumPy](https://numpy.org/) | Matrices, SVD, linear algebra, `.npy` I/O |
| [Matplotlib](https://matplotlib.org/) | Plotting time series and displaying images |

Python 3 is assumed. There is no pinned `requirements.txt` in the repository; a typical install is:

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install numpy matplotlib
```

## Repository structure

```
AUT-ALA-Projects/
├── README.md                 # This file
├── project_01/               # LU factorization (Quera-style CLI)
│   ├── LUFactorization.py
│   ├── project_01.pdf        # Assignment statement
│   └── README.md
├── project_02/               # Bitcoin price denoising
│   ├── main.py
│   ├── btc_price.npy         # Input series (2500 samples)
│   ├── jupyter_p2/           # Exploratory notebook
│   ├── LinearAlgebra_PA2.pdf
│   ├── plots.pdf             # Saved plots
│   └── README.md
└── project_03/               # BMP compression via SVD
    ├── Compress_BMP_Files.py
    ├── Compress_BMP_Files.ipynb
    ├── 1.bmp                 # Sample 1920×1280 RGB image
    ├── project manual/       # Assignment PDF + extra images
    ├── compressed bmp files.pptx
    └── README.md
```

## Quick start

Run each project from its own directory so relative paths to data files resolve correctly:

```bash
# Project 1 — stdin sample (see project_01/README.md for format)
cd project_01
python3 LUFactorization.py < sample_input.txt

# Project 2 — denoise and plot
cd ../project_02
python3 main.py

# Project 3 — SVD reconstruction and display
cd ../project_03
python3 Compress_BMP_Files.py
```

Projects 2 and 3 open Matplotlib figure windows (`plt.show()` / `imshow`).

## Limitations (repository-wide)

- These are course assignments, not production libraries or packaged applications.
- There are no automated tests, CI, or dependency lockfile.
- Numerical choices (regularization parameter \(\lambda\), SVD rank \(k\), lack of pivoting in LU) follow the assignment assumptions or hard-coded values in the scripts; see each project README for details.

## License

No license file is present in this repository. Treat the code as the author’s coursework unless a license is added later.
