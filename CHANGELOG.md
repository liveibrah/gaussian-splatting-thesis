# Changelog

All notable thesis-specific changes to this fork are documented here.

The original GraphDeco/Inria project history is preserved in the Git history and
upstream repository.

## Unreleased

### Documentation

- Added a research-fork notice for the top of the original README.
- Documented branch structure and attribution.
- Added contribution and reproducibility guidance.
- Documented the unsuccessful naive-pruning experiment.

### `fp16_quantization`

- Reconstructed the FP16 quantization experiment in
  `scene/gaussian_model.py`.
- Recorded that the exact original workstation patch was unavailable.
- Marked the implementation for full reproducibility validation.

### `opacity-pruning`

- Increased the native opacity-pruning threshold from `0.005` to `0.03`.
- Preserved the upstream optimizer-aware pruning workflow.
- Documented the previously evaluated conservative threshold of `0.015`.

## Baseline

- Forked from the official GraphDeco/Inria Gaussian Splatting repository.
- Preserved the original license, authorship, citation, and documentation.
