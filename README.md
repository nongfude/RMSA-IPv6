# RMSA-IPv6: Resilient Multi-homing Architecture for LEO Mega-Constellation Networks

This repository contains the simulation code and experimental data supporting the paper:

**"Resilient Multi-homing Architecture for LEO Mega-Constellation Networks Based on IPv6"**

*Submitted to IEEE Transactions on Aerospace and Electronic Systems*

---

## Repository Structure

```
RMSA-IPv6-Code/
├── src/                          # Source code
│   ├── constellation/            # Orbital propagation (Skyfield/SGP4)
│   ├── network/                  # Network simulation (NetworkX)
│   ├── perception/               # Isolation Forest anomaly detection
│   ├── decision/                 # STAR Q-learning routing
│   ├── execution/                # MH-SKAP protocol simulation
│   └── attacks/                  # Attack scenario implementations
├── results/                      # Experimental results
│   ├── scalability/              # Multi-scale experiments (66-1584 satellites)
│   ├── attack_scenarios/         # Attack resilience experiments
│   └── convergence/              # Q-learning convergence data
├── config/                       # Configuration files
│   ├── constellation_66.yaml
│   ├── constellation_264.yaml
│   ├── constellation_1056.yaml
│   └── constellation_1584.yaml
├── scripts/                      # Analysis and plotting scripts
│   ├── plot_scalability.py
│   ├── plot_convergence.py
│   ├── plot_attack_resilience.py
│   └── extract_statistics.py
└── requirements.txt              # Python dependencies
```

---

## Environment Setup

### System Requirements

- **Python**: 3.8 or higher
- **RAM**: Minimum 16 GB (32 GB recommended for 1584-satellite simulations)
- **CPU**: Multi-core processor (Intel i7 / AMD Ryzen or equivalent)
- **OS**: Linux, macOS, or Windows with WSL

### Dependencies

Install Python dependencies:

```bash
pip install -r requirements.txt
```

**Key Libraries:**
- `skyfield` (2.0+): SGP4 orbital propagation
- `networkx` (2.8+): Graph-based routing
- `scikit-learn` (1.2+): Isolation Forest anomaly detection
- `numpy` (1.23+), `scipy` (1.10+): Numerical computation
- `matplotlib` (3.6+), `seaborn` (0.12+): Visualization
- `pyyaml` (6.0+): Configuration parsing

### C++ Cryptographic Benchmarks (Optional)

Cryptographic latencies in Table II were measured using C++/OpenSSL 3.0:

```bash
cd crypto_benchmarks/
mkdir build && cd build
cmake ..
make
./benchmark_ecdsa
./benchmark_aes_gcm
./benchmark_shamir
```

Hardware tested:
- **User terminal**: ARMv8 Cortex-A72 @ 1.8 GHz
- **Satellite**: ARM Cortex-A53 @ 1.2 GHz

---

## Running Simulations

### Quick Start: Small-Scale Validation (66 satellites)

```bash
python src/main.py --config config/constellation_66.yaml --seeds 10 --duration 300 --output results/test_run/
```

Expected runtime: ~15 minutes on standard laptop

### Scalability Experiments

```bash
# 66 satellites (10 seeds, 300s duration)
python src/main.py --config config/constellation_66.yaml --seeds 10 --duration 300

# 264 satellites (10 seeds, 300s duration)
python src/main.py --config config/constellation_264.yaml --seeds 10 --duration 300

# 1056 satellites (10 seeds, 300s duration)
python src/main.py --config config/constellation_1056.yaml --seeds 10 --duration 300

# 1584 satellites (5 seeds, 60s duration - computational constraint)
python src/main.py --config config/constellation_1584.yaml --seeds 5 --duration 60
```

### Attack Scenario Experiments

```bash
# RF jamming attack (20% user links)
python src/main.py --config config/constellation_66.yaml --attack rf_jamming --intensity 0.2

# Routing manipulation (5% compromised satellites)
python src/main.py --config config/constellation_66.yaml --attack routing_manipulation --intensity 0.05

# Ground station DDoS (5 stations at 10 Gbps)
python src/main.py --config config/constellation_66.yaml --attack gs_ddos --targets 5 --rate 10000

# Coordinated multi-vector attack
python src/main.py --config config/constellation_66.yaml --attack coordinated
```

---

## Experimental Results

All experimental results from the paper are available in `results/` directory:

### Scalability Results (Table IV)

```
results/scalability/scale_66_n10_d300s.json
results/scalability/scale_264_n10_d300s.json
results/scalability/scale_1056_n10_d300s.json
results/scalability/scale_1584_n5_d60s.json
```

Each JSON file contains:
- Per-seed measurements (availability, latency, throughput, CPU utilization)
- Mean ± 95% confidence intervals
- Timestamp and configuration metadata

### Attack Resilience Results (Table V)

```
results/attack_scenarios/rf_jamming_scale66_n10.json
results/attack_scenarios/routing_manip_scale66_n10.json
results/attack_scenarios/gs_ddos_scale66_n10.json
results/attack_scenarios/coordinated_scale66_n10.json
```

### Convergence Analysis (Figure 5)

```
results/convergence/star_qlearning_convergence.json
results/convergence/baseline_qlearning_convergence.json
```

---

## Reproducing Paper Results

### Table III: Performance Under Normal Conditions

```bash
python scripts/extract_statistics.py --input results/scalability/scale_66_n10_d300s.json --table 3
```

### Table IV: Scalability Across Constellation Sizes

```bash
python scripts/extract_statistics.py --table 4 \
    --files results/scalability/scale_*_n*.json
```

### Table V: Attack Resilience

```bash
python scripts/extract_statistics.py --table 5 \
    --files results/attack_scenarios/*_scale66_n10.json
```

### Figures

```bash
# Figure 5: Q-learning convergence
python scripts/plot_convergence.py --output figures/fig5_qlearning_convergence.png

# Figure 6: RF jamming path switching
python scripts/plot_attack_timeline.py --attack rf_jamming --output figures/fig6_jamming_path_switching.jpg

# Figure 7: DDoS availability timeline
python scripts/plot_attack_timeline.py --attack gs_ddos --output figures/fig7_ddos_availability_timeline.png

# Figure 8: Scalability CPU utilization
python scripts/plot_scalability.py --metric cpu --output figures/fig8_scalability_cpu.png
```

---

## Configuration Parameters

Key simulation parameters (aligned with Section IV-A):

### Constellation Parameters
- **Altitude**: 780 km
- **Inclination**: 86.4° (Iridium-like)
- **Walker Delta Pattern**: Various configurations (66/6/1 to 1584/72/1)
- **ISL Range**: 2,800 km max
- **ISL Capacity**: 10 Gbps (free-space optical @ 1,550 nm)
- **User Link Capacity**: 50-250 Mbps (Ka-band)
- **Minimum Elevation**: 25°

### RMSA-IPv6 Architecture Parameters
- **Multi-homing degree**: K = 3
- **Isolation Forest trees (deployment)**: N_t = 8
- **Isolation Forest subsampling**: ψ = 256
- **Anomaly threshold**: θ_anomaly = 0.65
- **Q-learning rate**: α = 0.01
- **Q-learning discount**: γ = 0.95
- **Feature dimension**: 50 (Table I)
- **Key refresh interval**: 300 seconds

### Statistical Parameters
- **Random seeds**: 10 (Small/Medium/Large), 5 (Full-scale)
- **Simulation duration**: 300s (Small/Medium/Large), 60s (Full-scale)
- **Confidence interval**: 95% (1.96σ/√n)
- **Significance level**: α = 0.05 (two-tailed t-test)

---

## Validation and Testing

Run unit tests:

```bash
pytest tests/
```

Run integration tests:

```bash
python tests/integration_test_66sat.py
```

Expected outputs match paper results within experimental variance (±5% typical).

---

## Citation

If you use this code or data, please cite:

```bibtex
@article{rmsa-ipv6-2026,
  title={Resilient Multi-homing Architecture for {LEO} Mega-Constellation Networks Based on {IPv6}},
  author={[Anonymous for Review]},
  journal={IEEE Transactions on Aerospace and Electronic Systems},
  year={2026},
  note={Under Review}
}
```

---

## License

This code is released under the MIT License for research purposes.

---

## Contact

For questions or issues, please open a GitHub issue or contact the corresponding author.

**Anonymous Submission** - Contact information will be provided upon acceptance.
