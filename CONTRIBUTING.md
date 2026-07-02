# Contributing

Thank you for your interest in this research fork.

## Scope

This repository is derived from the official GraphDeco/Inria 3D Gaussian
Splatting codebase. It focuses on thesis-specific experiments involving FP16
quantization and opacity-based pruning.

Questions or bugs that affect the unmodified upstream implementation should
first be checked against the official repository.

## Choose the correct target branch

| Contribution | Target branch |
|---|---|
| General project documentation | `main` |
| FP16, mixed precision, or dtype handling | `fp16_quantization` |
| Opacity pruning or pruning-threshold work | `opacity-pruning` |
| Combined FP16 and pruning research | A new dedicated experimental branch |

The existing experiment branches should remain independent unless a contribution
explicitly studies their interaction.

## Recommended workflow

1. Fork this repository.
2. Clone your fork recursively.
3. Update the branch relevant to your work.
4. Create a focused feature branch.
5. Implement and test one logical change.
6. Document the command, environment, dataset, and results.
7. Open a pull request against the appropriate branch.

Example:

```bash
git clone --recursive https://github.com/liveibrah/gaussian-splatting-thesis.git
cd gaussian-splatting-thesis

git switch fp16_quantization
git pull origin fp16_quantization
git switch -c feature/configurable-fp16-policy
```

## Pull-request description

A pull request should explain:

- the research problem;
- the target branch;
- files changed;
- expected effect;
- dataset or scene used;
- exact training/rendering command;
- GPU and operating system;
- CUDA and PyTorch versions;
- evaluation metrics;
- model size and runtime when relevant;
- known limitations.

## Validation expectations

Where applicable, test:

- model initialization;
- at least one training iteration;
- backward propagation;
- densification;
- pruning;
- checkpoint saving and loading;
- PLY saving and loading;
- rendering;
- metric evaluation.

Relevant metrics include:

- PSNR;
- SSIM;
- LPIPS;
- training time;
- rendering time;
- GPU memory;
- model size;
- number of Gaussians.

## Repository hygiene

Do not commit:

- datasets;
- `.bag` recordings;
- trained models;
- checkpoints;
- rendered frame sequences;
- COLMAP databases;
- generated point clouds;
- virtual environments;
- caches;
- API keys or credentials;
- machine-specific absolute paths.

Small result tables, configuration files, and a few illustrative images are
acceptable when they are needed to explain an experiment.

## Attribution and license

Preserve all existing copyright, license, and attribution headers from the
upstream project.

Contributors must not present the original GraphDeco/Inria implementation as
their own work. New contributions should clearly identify which code and
documentation were added or modified in this fork.
