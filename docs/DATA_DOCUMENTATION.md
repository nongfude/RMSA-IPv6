# RMSA-IPv6 Experimental Data Documentation

## Overview

This document describes the experimental datasets supporting the paper "Resilient Multi-homing Architecture for LEO Mega-Constellation Networks Based on IPv6" submitted to IEEE Transactions on Aerospace and Electronic Systems.

All results are generated using controlled random seeds with 95% confidence intervals for statistical rigor.

---

## Dataset Organization

```
results/
├── scalability/                  # Multi-scale constellation experiments
│   ├── scale_66_n10_d300s.json
│   ├── scale_264_n10_d300s.json
│   ├── scale_1056_n10_d300s.json
│   └── scale_1584_n5_d60s.json
├── attack_scenarios/             # Attack resilience experiments
│   ├── normal_operation_scale66_n10.json
│   ├── rf_jamming_scale66_n10.json
│   ├── routing_manip_scale66_n10.json
│   ├── gs_ddos_scale66_n10.json
│   └── coordinated_attack_scale66_n10.json
├── convergence/                  # Q-learning convergence analysis
│   ├── star_qlearning_convergence.json
│   └── baseline_qlearning_convergence.json
└── raw_traces/                   # Per-seed detailed traces (optional)
    ├── trace_66sat_seed0.json
    ├── trace_66sat_seed1.json
    └── ...
```

---

## Data Format Specification

### Scalability Results Format

Each `scale_*_n*_d*.json` file contains:

```json
{
  "metadata": {
    "constellation_size": 66,
    "num_seeds": 10,
    "duration_seconds": 300,
    "timestamp": "2026-09-30T10:00:00Z",
    "config_file": "constellation_66.yaml",
    "methods_compared": ["SPR", "SMH", "MPTCP", "RMSA"]
  },
  "results": {
    "RMSA": {
      "availability": {
        "per_seed": [77.5, 78.1, 77.2, ...],
        "mean": 77.67,
        "std": 0.41,
        "ci_95": 0.80,
        "ci_lower": 76.87,
        "ci_upper": 78.47
      },
      "latency_mean": {
        "per_seed": [96.5, 97.1, 96.3, ...],
        "mean": 96.77,
        "std": 0.18,
        "ci_95": 0.36
      },
      "latency_95th": { ... },
      "throughput": { ... },
      "handoff_latency": { ... },
      "cpu_utilization": { ... },
      "protocol_overhead": { ... }
    },
    "SPR": { ... },
    "SMH": { ... },
    "MPTCP": { ... }
  }
}
```

**Fields:**
- `per_seed`: Raw measurements from each random seed
- `mean`: Sample mean across seeds
- `std`: Sample standard deviation
- `ci_95`: 95% confidence interval half-width (±1.96σ/√n)
- `ci_lower`, `ci_upper`: Confidence interval bounds

### Attack Scenario Results Format

Each attack scenario file includes:

```json
{
  "metadata": {
    "attack_type": "rf_jamming",
    "constellation_size": 66,
    "num_seeds": 10,
    "duration_seconds": 300,
    "attack_parameters": {
      "intensity": 0.2,
      "start_time": 30,
      "affected_users_pct": 20
    }
  },
  "baseline": {
    "RMSA": {"availability": 78.30, "ci_95": 0.75},
    "SPR": {"availability": 79.70, "ci_95": 0.68},
    ...
  },
  "under_attack": {
    "RMSA": {
      "availability": {"mean": 73.77, "ci_95": 0.78},
      "degradation_pp": -4.53,
      "detection_time_ms": {"mean": 95.1, "std": 18.9},
      "recovery_time_ms": {"mean": 10.4, "std": 0.9}
    },
    ...
  }
}
```

### Convergence Data Format

Q-learning convergence files contain episode-by-episode metrics:

```json
{
  "metadata": {
    "algorithm": "STAR",
    "episodes": 25000,
    "learning_rate": 0.01,
    "discount_factor": 0.95
  },
  "convergence_data": [
    {"episode": 0, "avg_reward": -150.3, "epsilon": 1.0},
    {"episode": 100, "avg_reward": -120.5, "epsilon": 0.95},
    ...
    {"episode": 18000, "avg_reward": 85.2, "epsilon": 0.1}
  ],
  "convergence_episode": 18000,
  "final_avg_reward": 85.2
}
```

---

## Paper Table/Figure Mapping

### Table III: Performance Under Normal Conditions (No Attacks)
**Source**: `results/scalability/scale_66_n10_d300s.json`
- 66 satellites, 10 seeds, 300s duration
- Metrics: Availability, Latency (mean & 95th), Throughput, Handoff Latency, CPU, Overhead
- Methods: SPR, SMH, MPTCP, RMSA

### Table IV: Scalability Across Constellation Sizes
**Sources**: 
- `results/scalability/scale_66_n10_d300s.json`
- `results/scalability/scale_264_n10_d300s.json`
- `results/scalability/scale_1056_n10_d300s.json`
- `results/scalability/scale_1584_n5_d60s.json`

**Note**: 1584-satellite configuration uses 5 seeds (computational constraint) with 60s duration.

### Table V: Measured Service Availability Under Attack Scenarios
**Sources**:
- `results/attack_scenarios/normal_operation_scale66_n10.json` (baseline row)
- `results/attack_scenarios/rf_jamming_scale66_n10.json`
- `results/attack_scenarios/routing_manip_scale66_n10.json`
- `results/attack_scenarios/gs_ddos_scale66_n10.json`
- `results/attack_scenarios/coordinated_attack_scale66_n10.json`

### Figure 5: Q-learning Convergence
**Sources**:
- `results/convergence/star_qlearning_convergence.json` (RMSA-IPv6 STAR)
- `results/convergence/baseline_qlearning_convergence.json` (Standard Q-learning)

### Figure 6: RF Jamming Path Switching Timeline
**Source**: `results/attack_scenarios/rf_jamming_scale66_n10.json` + detailed traces

### Figure 7: DDoS Availability Timeline
**Source**: `results/attack_scenarios/gs_ddos_scale66_n10.json` + detailed traces

### Figure 8: CPU Utilization Scalability
**Source**: All files in `results/scalability/`, CPU utilization field extracted

---

## Statistical Rigor

### Confidence Interval Computation

95% confidence intervals computed as:
```
CI_95 = mean ± 1.96 × (σ / √n)
```

Where:
- `mean`: Sample mean across n seeds
- `σ`: Sample standard deviation
- `n`: Number of random seeds (10 for most experiments, 5 for 1584-sat)

### Significance Testing

Two-tailed t-tests at α = 0.05 significance level used for method comparisons.

Test statistic:
```
t = (mean_A - mean_B) / sqrt(s²_A/n_A + s²_B/n_B)
```

Degrees of freedom approximated using Welch-Satterthwaite equation.

p-values reported where p < 0.05 indicates statistically significant difference.

---

## Reproducibility

### Verification Steps

1. **Check file integrity**:
```bash
python scripts/verify_data_integrity.py --directory results/
```

2. **Reproduce table statistics**:
```bash
python scripts/extract_statistics.py --table 3 --verify
python scripts/extract_statistics.py --table 4 --verify
python scripts/extract_statistics.py --table 5 --verify
```

3. **Regenerate figures**:
```bash
python scripts/plot_all_figures.py --output figures/ --verify
```

Expected match: Paper values should match recomputed values within ±0.01 for percentages, ±0.1 for latencies.

### Random Seeds Used

Scalability experiments (66/264/1056 satellites): Seeds 0-9
Full-scale (1584 satellites): Seeds 0-4
Attack scenarios: Seeds 0-9

Seeds are fixed in configuration files for exact reproducibility.

---

## Data Availability Statement

**For Reviewers**: All data files are included in the submission supplementary materials archive `RMSA-IPv6-Data.zip`.

**Upon Acceptance**: Data will be deposited in IEEE DataPort with DOI assignment and made publicly available under CC BY 4.0 license.

**Contact**: For data access questions during review, contact the corresponding author through the IEEE submission system.

---

## File Size Summary

| Directory | Files | Total Size | Description |
|-----------|-------|------------|-------------|
| `scalability/` | 4 | ~15 MB | Multi-scale experiments |
| `attack_scenarios/` | 5 | ~20 MB | Attack resilience data |
| `convergence/` | 2 | ~8 MB | Q-learning convergence |
| `raw_traces/` (optional) | 100+ | ~500 MB | Per-seed detailed traces |
| **Total** | **111+** | **~543 MB** | Full dataset |

Core dataset (without raw traces): ~43 MB
Full dataset (with raw traces): ~543 MB

Both versions available upon request.

---

## Data Quality Assurance

### Validation Checks Performed

✓ All seeds produce valid results (no crashes/errors)
✓ Confidence intervals are symmetric and reasonable
✓ Statistical distributions are approximately normal (Shapiro-Wilk test)
✓ No outliers beyond 3σ (checked per metric per method)
✓ Results are consistent across multiple runs with same seeds
✓ Cross-validation: Independent re-runs match within 2% variance

### Known Limitations

- 1584-satellite experiments use 5 seeds (vs. 10) due to computational constraints
- 1584-satellite duration is 60s (vs. 300s) for tractability
- Rare events (e.g., simultaneous multi-satellite failures) may be undersampled
- Simulation abstracts physical layer details (RF propagation, antenna patterns)

---

## Version History

- **v1.0** (2026-09-30): Initial submission dataset
- Future versions will be tracked with DOI versioning upon publication

---

## License

Data released under **Creative Commons Attribution 4.0 International (CC BY 4.0)** upon acceptance.

Citation required when using this data (see main README.md).
