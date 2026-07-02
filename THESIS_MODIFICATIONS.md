# Thesis Modifications

## Overview

This repository is a research fork of the official
[GraphDeco/Inria 3D Gaussian Splatting implementation](https://github.com/graphdeco-inria/gaussian-splatting).

The fork documents two independent efficiency experiments developed for an MSc
thesis:

1. FP16 Gaussian-parameter quantization.
2. Increased opacity-based Gaussian pruning.

Both branches originate from the same upstream baseline.

```text
main
├── fp16_quantization
└── opacity-pruning
```

The experiments were not combined in the thesis.

## Upstream attribution

The original 3D Gaussian Splatting implementation was created by Bernhard
Kerbl, Georgios Kopanas, Thomas Leimkühler, and George Drettakis.

All original source files remain subject to the upstream repository's license,
copyright notices, and citation requirements. This fork does not claim
authorship of the original 3DGS implementation.

## Branch summary

| Branch | Main file changed | Research purpose | Status |
|---|---|---|---|
| `main` | Documentation only | Preserve the common upstream baseline | Baseline |
| `fp16_quantization` | `scene/gaussian_model.py` | Evaluate reduced-precision storage/processing | Reconstructed |
| `opacity-pruning` | `train.py` | Evaluate stronger opacity-based pruning | Reconstructed |

## FP16 quantization

**Branch:** `fp16_quantization`

### Objective

Reduce the precision of selected floating-point values in
`scene/gaussian_model.py` from FP32 to FP16 and evaluate the effect on:

- model representation;
- memory consumption;
- training time;
- rendering time;
- reconstruction quality.

### Implementation status

The original workstation version was unavailable when the repository was
organized. The branch was reconstructed from thesis notes and the author's
recollection that the quantization experiment was implemented by changing
floating-point value types in `scene/gaussian_model.py`.

The branch diff is the authoritative record of the reconstructed implementation:

```bash
git diff main..fp16_quantization -- scene/gaussian_model.py
```

### Important limitation

The reconstructed branch may not be byte-for-byte identical to the original
workstation implementation. It should be fully rerun and validated before being
described as an exact reproduction.

See [docs/FP16_QUANTIZATION.md](docs/FP16_QUANTIZATION.md).

## Opacity pruning

**Branch:** `opacity-pruning`

### Objective

Increase the opacity threshold used by the native 3DGS densification-and-pruning
pipeline.

### Baseline

The upstream training loop calls:

```python
gaussians.densify_and_prune(
    opt.densify_grad_threshold,
    0.005,
    scene.cameras_extent,
    size_threshold,
    radii
)
```

### Reconstructed aggressive-pruning change

The experimental branch changes the opacity threshold to `0.03`:

```python
gaussians.densify_and_prune(
    opt.densify_grad_threshold,
    0.03,
    scene.cameras_extent,
    size_threshold,
    radii
)
```

The implementation reuses the upstream optimizer-aware pruning functions instead
of directly deleting tensor rows.

A conservative threshold of `0.015` was also evaluated during the thesis, but
the current branch records the aggressive `0.03` configuration.

See [docs/OPACITY_PRUNING.md](docs/OPACITY_PRUNING.md).

## Preliminary naive-pruning attempt

Before using the native pruning pipeline, a preliminary experiment attempted to
remove low-opacity Gaussians by directly filtering internal parameter tensors.

That approach produced a large apparent reduction in model size but caused
severe reconstruction degradation. It bypassed state that must remain aligned
with the Gaussian tensors, including optimizer statistics and densification
buffers.

The failed implementation is documented for research transparency and is not
recommended for use.

See [docs/NAIVE_PRUNING_EXPERIMENT.md](docs/NAIVE_PRUNING_EXPERIMENT.md).

## Reproducibility status

| Experiment | Source status | Validation status |
|---|---|---|
| FP16 quantization | Reconstructed from thesis records and recollection | Full rerun recommended |
| Opacity threshold `0.03` | Reconstructed from the documented threshold change | Full rerun recommended |
| Naive manual pruning | Exact source unavailable; method documented conceptually | Known unsuccessful |
| Combined FP16 + pruning | Not performed in the thesis | Not available |

## Future improvements

Suggested contributions include:

- exposing the opacity threshold as a command-line argument;
- adding a configurable FP16 precision policy;
- adding dtype and model-loading tests;
- rerunning baseline and experimental branches on the same datasets;
- recording hardware, CUDA, PyTorch, and dependency versions;
- creating a separate branch for combined FP16 and pruning experiments.
