# Project 03 — BMP Image Approximation with SVD

Approximates an RGB bitmap by truncated singular value decomposition (SVD) on each color channel. Assignment materials: [project manual/LinearAlgebra_PA3.pdf](project%20manual/LinearAlgebra_PA3.pdf).

## Problem

BMP files store pixel values without the kind of transform coding used in formats like JPEG. Treating each color channel as a matrix, SVD provides an ordered expansion in singular values. Keeping only the first \(k\) terms yields a low-rank approximation that can look similar to the original while requiring far less storage if only the truncated factors are kept.

## Method

For an image array `img` of shape \((H, W, 3)\):

1. Split channels: \(R, G, B\)
2. Compute full SVD of each channel: \(M = U \Sigma V^\top\) via `numpy.linalg.svd`
3. Keep the first \(k\) singular values and corresponding columns/rows of \(U\) and \(V^\top\)
4. Reconstruct \(\tilde{M} = \sum_{i=1}^{k} \sigma_i\, u_i v_i^\top\)
5. Stack channels, cast to `uint8`, and display with Matplotlib

### Storage idea (from the assignment)

Compression is achieved by **storing** the truncated factors \(U_{:,1:k}\), \(\sigma_{1:k}\), and \(V^\top_{1:k,:}\) per channel, then reconstructing at display time—not by writing another full-resolution BMP of the same dimensions.

Example numbers from the assignment (different resolution than the sample file here): for a \(1024 \times 1024\) RGB image, full storage is \(1024 \times 1024 \times 3\) entries; with \(k = 100\), truncated factors need on the order of \(3 \times (1024\cdot 100 + 100 + 100\cdot 1024)\) entries—substantially fewer.

## Implemented vs. partial

| Capability | Status |
|------------|--------|
| Load BMP, split RGB, SVD, reconstruct, display | **Implemented** in `Compress_BMP_Files.py` and the notebook |
| Rank parameter \(k\) | **Hard-coded** (`k = 150` in `.py`; notebook cell uses `k = 10`) |
| Compare multiple \(k\) (assignment: 10, 50, 100, 200) | **Partially** — notebook/PPTX support exploration; script runs one \(k\) |
| Save truncated \(U,\Sigma,V^\top\) to disk as the compressed representation | **Not implemented** (comment in code notes the idea only) |
| Extra sample images | Present under `project manual/images.zip` |

## Sample data

| File | Details |
|------|---------|
| `1.bmp` | 1920×1280, 24-bit RGB (~7 MB uncompressed BMP) |

## Technology

- Python 3
- NumPy — `linalg.svd`, array ops
- Matplotlib — `matplotlib.image.imread`, `imshow`

## Usage

```bash
cd project_03
python3 Compress_BMP_Files.py
```

Requires `1.bmp` in the current working directory. Displays the reconstructed image.

Notebook (useful for trying different \(k\)):

```bash
jupyter notebook Compress_BMP_Files.ipynb
```

Dependencies: `numpy`, `matplotlib` (see [root README](../README.md)).

### Changing rank \(k\)

In `Compress_BMP_Files.py`:

```python
k = 150  # try 10, 50, 100, 200 as in the assignment
```

Smaller \(k\) → stronger compression / more blur; larger \(k\) → closer to the original.

## Design notes

- SVD uses `full_matrices=True`; only the first \(k\) components are used afterward.
- Reconstruction accumulates rank-1 updates in a Python loop rather than a single matrix product \(U_k \operatorname{diag}(\sigma_k) V_k^\top\) (equivalent, slower for large \(k\)).
- Reconstructed values are cast with `astype(np.uint8)` without explicit clipping; out-of-range floats from reconstruction may wrap when converted.

## Limitations

- No file writer for the truncated SVD factors (the actual compressed payload).
- One sample path and one \(k\) in the main script.
- Full SVD of a 1920×1280 channel is memory- and CPU-heavy; fine for coursework, not tuned for large batches.
- No automated quality metrics (e.g. PSNR) or tests.

## Files

| File | Description |
|------|-------------|
| `Compress_BMP_Files.py` | Scripted pipeline (`k = 150`) |
| `Compress_BMP_Files.ipynb` | Interactive version |
| `1.bmp` | Sample image |
| `compressed bmp files.pptx` | Presentation / results slides |
| `project manual/LinearAlgebra_PA3.pdf` | Assignment statement |
| `project manual/images.zip` | Additional BMP samples from the course |

## Related

- [Repository overview](../README.md)
- [Project 01 — LU factorization](../project_01/README.md)
- [Project 02 — least-squares denoising](../project_02/README.md)
