# Reproducibility Guide

## Record the source revision

Before running an experiment:

```bash
git branch --show-current
git rev-parse HEAD
git status --short
```

Save the branch name and commit SHA with the experiment results.

## Record the environment

At minimum, record:

```bash
python --version
nvidia-smi
nvcc --version
python -c "import torch; print(torch.__version__, torch.version.cuda)"
conda env export > environment-export.yml
```

## Record the command

Store the exact training command in a text or Markdown file:

```bash
python train.py   -s /path/to/dataset   -m output/experiment_name   --eval
```

## Recommended directory naming

```text
results/
└── <scene>/
    └── <branch>/
        └── <date-or-commit>/
            ├── command.txt
            ├── environment.txt
            ├── metrics.json
            ├── timing.txt
            └── notes.md
```

Do not commit large model files or complete render sequences to Git.

## Minimum comparison table

| Metric | Baseline | Experimental |
|---|---:|---:|
| Number of Gaussians | | |
| Model size | | |
| Peak GPU memory | | |
| Training time | | |
| Rendering time | | |
| PSNR | | |
| SSIM | | |
| LPIPS | | |

## Validation labels

Use one of these labels in experiment documentation:

- **Recovered** — exact original implementation was recovered.
- **Reconstructed and validated** — implementation was recreated and rerun successfully.
- **Reconstructed, validation pending** — implementation was recreated but not fully rerun.
- **Documented only** — method is described but source code is unavailable.
