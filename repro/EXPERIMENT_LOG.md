# HEAL Reproduction Experiment Log

## 2026-09-05 — Stage 1 1-Epoch Rehearsal

### Purpose
End-to-end verification of the HEAL Stage 1 training and evaluation pipeline on OPV2V before the full baseline reproduction.

### Environment
- GPU: NVIDIA GeForce RTX 5070 Laptop GPU
- VRAM: 7.96 GB
- PyTorch: 2.7.1+cu128
- spconv: 2.3.8
- Branch: `repro/rtx5070`

### Configuration
- Base config: `m1_pyramid.yaml`
- Adapted config: `m1_pyramid_rtx5070_b1_1ep.yaml`
- Physical batch size: 1
- Epochs: 1
- Other major training settings unchanged

### Dataset
- OPV2V train: 6374 samples
- OPV2V validate: 1980 samples
- OPV2V test: 2170 samples

### Results
- Validation loss: 0.840499
- AP@0.3: 0.86
- AP@0.5: 0.83
- AP@0.7: 0.53

### Output
`opencood/logs/Pyramid_m1_base_rtx5070_b1_1ep_2026_09_05_21_10_06`

### Status
- End-to-end pipeline: PASS
- Final baseline reproduction: NOT YET

### Hardware Constraint
Official Stage 1 config uses batch size 4.

Observed training-step GPU memory:
- batch=1, 5 agents: 5.09 GB allocated / 5.82 GB reserved
- batch=2, 10 agents: 10.14 GB allocated / 11.66 GB reserved
- batch=4, 8 agents: 9.55 GB allocated / 10.78 GB reserved

Therefore the current 8 GB laptop GPU cannot safely run the official batch-size configuration.

## 2026-09-07 — Stage 1 30-Epoch Hardware-Adapted Run

### Purpose
Complete HEAL Stage 1 training and evaluation on OPV2V using the laptop-compatible batch-size configuration.

### Configuration
- Base config: `m1_pyramid.yaml`
- Adapted config: `m1_pyramid_rtx5070_b1.yaml`
- Physical batch size: 1
- Epochs: 30
- Other major training settings unchanged

### Results
- AP@0.3: 0.95
- AP@0.5: 0.94
- AP@0.7: 0.90

### Output
`opencood/logs/Pyramid_m1_base_rtx5070_b1_2026_09_06_16_18_16`

### Status
- Full 30-epoch hardware-adapted training: PASS
- End-to-end train / validation / checkpoint / test inference pipeline: PASS
- Strict official baseline reproduction: NO

### Note
The official HEAL Stage 1 configuration uses physical batch size 4. This run uses physical batch size 1 because of the 7.96 GB VRAM constraint on the RTX 5070 Laptop GPU. Therefore, the reported AP values should be treated as results of the hardware-adapted reproduction rather than as the strict official batch-size baseline.
