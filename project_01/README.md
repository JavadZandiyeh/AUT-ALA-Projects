# Project 01 — LU Factorization

Implements LU factorization of a square matrix \(A\) and solves multiple linear systems \(Ax = b\) via forward and backward substitution. Written for a Quera programming assignment ([assignment PDF](project_01.pdf)).

## Problem

Gaussian elimination on each right-hand side costs \(O(n^3)\). When \(A\) is fixed and many vectors \(b\) must be solved, computing \(A = LU\) once (\(O(n^3)\)) and then solving \(Ly = b\) and \(Ux = y\) for each \(b\) (\(O(n^2)\) each) is cheaper.

Assumptions from the assignment:

- \(A\) is \(n \times n\) and has an LU factorization (no pivoting required)
- Each system has a unique solution

## Implemented functionality

| Function | Role |
|----------|------|
| `LU(A)` | Computes lower-triangular \(L\) (unit diagonal) and upper-triangular \(U\) |
| `solving_Ly_equal_b(L, b)` | Forward substitution for \(Ly = b\) |
| `solving_Ux_equal_y(U, y)` | Backward substitution for \(Ux = y\) |

The script:

1. Reads \(n\), \(m\), matrix \(A\), and \(m\) right-hand sides from standard input
2. Factorizes \(A\) once
3. For each \(b\), solves for \(x\) and prints components rounded to **two decimal places**

## Technology

- Python 3
- NumPy (`numpy.array`, `numpy.eye`, `numpy.dot`)

## Input / output format

**Input**

```
n m
<a row 1: n integers>
...
<a row n: n integers>
<b_1: n integers>
...
<b_m: n integers>
```

**Output**

\(m\) lines; each line is the solution \(x\) for the corresponding \(b\), space-separated, rounded to 2 decimals.

### Sample (from the assignment)

Input:

```
3 5
5 6 2
4 5 2
2 4 8
18 7 2
4 5 8
15 7 6
11 9 5
13 12 12
```

Expected output (values as in the assignment statement):

```
75.0 -64.0 13.5
-14.0 13.0 -2.0
53.0 -45.0 10.0
0.5 1.5 -0.25
-10.0 11.0 -1.5
```

Intermediate \(L\) and \(U\) shown in the PDF are for explanation only and must **not** be printed.

## Usage

```bash
cd project_01
python3 LUFactorization.py
# then type (or pipe) the input described above
```

Example with a file:

```bash
python3 LUFactorization.py < sample_input.txt
```

Dependencies: `numpy` (see [root README](../README.md) for install).

## Design notes

- Elimination updates \(U\) in place and fills multipliers into \(L\); \(L\) starts as the identity.
- Print formatting uses `round(..., 2)`, consistent with the assignment’s two-decimal requirement (`np.around` is referenced in the PDF).
- Global `n` from the input is used when building \(L\); the matrix is assumed square of order \(n\).

## Limitations

- **No partial pivoting** — fails or is unstable if a pivot is zero or very small (allowed by the assignment’s assumptions).
- Integer input only (as specified); computation uses `float64`.
- Not packaged as a reusable module; CLI script only.
- No unit tests in the repository.

## Files

| File | Description |
|------|-------------|
| `LUFactorization.py` | Full solution |
| `project_01.pdf` | Assignment statement (Persian, from Quera) |

## Related

- [Repository overview](../README.md)
- [Project 02 — least-squares denoising](../project_02/README.md)
- [Project 03 — SVD image approximation](../project_03/README.md)
