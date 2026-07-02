# FP16 Quantization Experiment

## Branch

```text
fp16_quantization
```

## Goal

Investigate whether changing selected floating-point values in the Gaussian
model from FP32 to FP16 can reduce memory or rendering cost while preserving
acceptable reconstruction quality.

## Modified area

The reconstructed implementation is contained primarily in:

```text
scene/gaussian_model.py
```

To inspect the exact branch changes:

```bash
git diff main..fp16_quantization -- scene/gaussian_model.py
```

## Historical reconstruction note

The original modified workstation copy was not available when this repository
was organized. The implementation was reconstructed from:

- thesis documentation;
- surviving experiment results;
- the author's recollection that float value types were changed in
  `gaussian_model.py`.

For this reason, this branch should be described as a reconstructed
implementation until it has been rerun and compared against the thesis results.

## Recommended validation

Confirm the dtype of each Gaussian parameter after:

1. point-cloud initialization;
2. checkpoint restoration;
3. PLY loading;
4. densification;
5. pruning;
6. model saving and reloading.

A useful diagnostic is:

```python
print("xyz:", gaussians._xyz.dtype)
print("features_dc:", gaussians._features_dc.dtype)
print("features_rest:", gaussians._features_rest.dtype)
print("opacity:", gaussians._opacity.dtype)
print("scaling:", gaussians._scaling.dtype)
print("rotation:", gaussians._rotation.dtype)
```

## Suggested experiment record

For each scene, report:

| Field | Value |
|---|---|
| Dataset | |
| Source commit | |
| GPU | |
| CUDA version | |
| PyTorch version | |
| Iterations | |
| Resolution | |
| Baseline model size | |
| FP16 model size | |
| Baseline render time | |
| FP16 render time | |
| Baseline PSNR / SSIM / LPIPS | |
| FP16 PSNR / SSIM / LPIPS | |

## Known risks

- CUDA kernels may expect FP32 inputs.
- Implicit casts can reduce or eliminate memory savings.
- Optimizer state may remain FP32 or create additional buffers.
- PLY serialization may store values as FP32 even when runtime tensors are FP16.
- Large or complex scenes may show different numerical behavior.
