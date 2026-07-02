# Opacity-Pruning Experiment

## Branch

```text
opacity-pruning
```

## Goal

Evaluate stronger removal of low-opacity Gaussians while preserving the native
3DGS pruning pipeline.

## Modified file

```text
train.py
```

## Baseline behavior

The upstream training loop uses an opacity threshold of `0.005`:

```python
gaussians.densify_and_prune(
    opt.densify_grad_threshold,
    0.005,
    scene.cameras_extent,
    size_threshold,
    radii
)
```

## Aggressive-pruning branch

The branch changes the opacity threshold to `0.03`:

```python
gaussians.densify_and_prune(
    opt.densify_grad_threshold,
    0.03,
    scene.cameras_extent,
    size_threshold,
    radii
)
```

## Why the native pipeline is used

The upstream pruning functions compact the Gaussian parameters and maintain
associated optimization state. This is safer than directly slicing internal
tensors.

The native workflow handles structures such as:

- Gaussian positions;
- spherical-harmonic features;
- opacity;
- scale;
- rotation;
- optimizer moving averages;
- gradient accumulators;
- image-space radii;
- densification buffers.

## Thesis thresholds

| Configuration | Opacity threshold |
|---|---:|
| Upstream baseline | `0.005` |
| Conservative experiment | `0.015` |
| Aggressive experiment | `0.03` |

The current branch records the aggressive configuration.

## Recommended future improvement

Replace the hardcoded value with a command-line parameter, for example:

```bash
python train.py   -s /path/to/dataset   -m output/prune_003   --opacity_pruning_threshold 0.03
```

The default should remain `0.005` so that upstream behavior is preserved unless
the user explicitly selects a stronger threshold.

## Suggested validation

Report:

- number of Gaussians before and after training;
- model size;
- PSNR;
- SSIM;
- LPIPS;
- training time;
- rendering time;
- visible artifacts;
- exact threshold and commit.
