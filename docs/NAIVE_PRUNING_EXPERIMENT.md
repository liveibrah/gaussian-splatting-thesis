# Preliminary Naive-Pruning Experiment

## Status

Unsuccessful preliminary experiment. Not recommended for production or
reproducibility use.

## Objective

Remove low-opacity Gaussians from an already trained model by creating an
opacity mask and directly filtering Gaussian parameter tensors.

Conceptually, the experiment followed this pattern:

```python
prune_mask = gaussians.get_opacity.squeeze() < threshold
keep_mask = ~prune_mask
```

The same mask was then applied directly to internal tensors such as:

```text
_xyz
_features_dc
_features_rest
_opacity
_scaling
_rotation
```

## Why it failed

Direct tensor filtering bypassed internal state that must remain aligned with
the Gaussian parameters, including:

- optimizer statistics;
- gradient accumulators;
- maximum image-space radii;
- densification state;
- temporary buffers;
- model bookkeeping.

The resulting model showed a large apparent reduction in size but severe visual
and metric degradation.

## Source-code availability

The exact original naive-pruning function and its function name are no longer
available. This document records the method conceptually based on the thesis
discussion and surviving experiment results.

The operational `opacity-pruning` branch does not include this implementation.
It instead reuses the upstream optimizer-aware pruning pipeline.

## Research lesson

Structural compression in 3DGS is not equivalent to deleting rows from a set of
independent tensors. All per-Gaussian state must be compacted consistently.
