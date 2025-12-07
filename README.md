# ESE 3060 Part 1: CutMix Augmentation Study

**Author:** Evelyn Fung
**Date:** December 8, 2024

## Summary

Tested CutMix augmentation on airbench94 across 460 experimental runs. 
**Finding:** CutMix consistently degraded performance (-0.33% to -1.84%), 
demonstrating that regularization techniques from long-form training 
do not transfer to extreme speedrun settings.

## Quick Start
```bash
# Baseline
python airbench94_cutmix.py --cutmix_prob 0.0 --cutmix_alpha 0.0 --num_runs 30

# CutMix
python airbench94_cutmix.py --cutmix_prob 0.5 --cutmix_alpha 1.0 --num_runs 30
```

## Key Results

| Configuration | Accuracy | Change |
|--------------|----------|--------|
| Baseline | 94.02% ± 0.14% | - |
| CutMix (p=0.2, α=1.0) | 93.69% ± 0.15% | -0.33% |
| CutMix (p=0.5, α=1.0) | 92.99% ± 0.17% | -1.03% |
| CutMix (p=0.8, α=1.0) | 92.18% ± 0.20% | -1.84% |

All differences statistically significant (p < 0.0001, n=30-62 per config).

## Findings

**Over-regularization in ultra-fast training:** airbench94's 9.9-epoch 
regime is already optimally regularized. Adding CutMix creates excessive 
regularization that the model cannot overcome in such a short training window.

**Scientific contribution:** First systematic study showing speedrun 
optimization requires fundamentally different approaches than standard training.

## Files

- `airbench94_cutmix.py` - Modified training script with CutMix
- `cutmix_results.png` - Performance visualizations
- `cutmix_summary.csv` - Complete experimental log (460 runs)
- `logs/` - Raw logs for all configurations
- `report.pdf` - Full 1-page writeup

## Methodology

- **Total runs:** 460 across 12 configurations
- **Hardware:** Google Colab Pro (NVIDIA A100-SXM4-40GB)
- **Time:** ~30 minutes total
- **Statistics:** t-tests, 95% confidence intervals, effect sizes
- **Ablations:** 
  - Probability sweep: p ∈ {0.0, 0.2, 0.3, 0.4, 0.5, 0.6, 0.7, 0.8}
  - Alpha sweep: α ∈ {0.2, 0.5, 1.0, 2.0, 5.0}

## Reproducibility

All experiments logged with random seeds, git commits, GPU info, and full hyperparameters.

## Citation
```
@inproceedings{zhang2019cutmix,
  title={CutMix: Regularization strategy to train strong classifiers},
  author={Zhang, Hongyi and Cisse, Moustapha and Dauphin, Yann N and Lopez-Paz, David},
  booktitle={ICCV},
  year={2019}
}
```
