# 🛰️ RMSA-IPv6 Anonymous Review Repository

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Paper: Under Review](https://img.shields.io/badge/Paper-Under%20Review-blue.svg)](https://ieeexplore.ieee.org)

**Anonymous Submission for IEEE Transactions on Aerospace and Electronic Systems**

---

## 📋 Repository Status

🔒 **Anonymous Submission**: Author information withheld for double-blind peer review  
📊 **Complete Data**: All experimental results from the paper included  
📖 **Full Documentation**: Comprehensive setup and reproduction guides  
🚧 **Source Code**: Placeholder structure provided; full implementation released upon acceptance  

---

## 📂 What's Included

### ✅ Documentation
- **README.md**: Complete project documentation with setup instructions
- **DATA_DOCUMENTATION.md**: Dataset specifications, formats, and statistical methods
- **COMPLETION_REPORT.md**: Summary of all fixes and improvements
- Configuration files for all experimental scales

### ✅ Experimental Results
- `results/scalability/`: Multi-scale experiments (66, 264, 1056, 1584 satellites)
- `results/attack_scenarios/`: Attack resilience data (RF jamming, routing manipulation, DDoS, coordinated)
- `results/convergence/`: Q-learning convergence analysis

### ✅ Configuration Files
- YAML configuration for all constellation scales (66 to 1584 satellites)
- Parameter settings matching paper specifications
- Attack scenario configurations

### ✅ Code Structure
- Directory layout showing module organization
- Placeholder scripts demonstrating architecture
- `requirements.txt` with all Python dependencies

---

## 🚀 Quick Start for Reviewers

### 1. Review Documentation
```bash
# Clone the repository
git clone https://github.com/YOUR_USERNAME/RMSA-IPv6-Anonymous-Review.git
cd RMSA-IPv6-Anonymous-Review

# Read main documentation
cat README.md
cat docs/DATA_DOCUMENTATION.md
```

### 2. Inspect Experimental Results
```bash
# List all result files
ls results/scalability/
ls results/attack_scenarios/

# View a sample result file
cat results/scalability/scale_66_n10_d300s.json | head -50
```

### 3. Check Configuration
```bash
# View constellation configurations
cat config/constellation_66.yaml
cat config/constellation_1584.yaml
```

### 4. Verify Dependencies
```bash
cat requirements.txt
```

### 5. Test Placeholder Code
```bash
# Install dependencies (optional)
pip install -r requirements.txt

# Run placeholder simulator
python src/main.py --config config/constellation_66.yaml --seeds 10 --duration 300
```

---

## 📊 Paper Mapping

| Paper Element | Repository Location | Description |
|--------------|---------------------|-------------|
| **Table III** | `results/scalability/scale_66_n10_d300s.json` | Performance under normal conditions (66 satellites) |
| **Table IV** | `results/scalability/scale_*_n*.json` | Scalability across constellation sizes (4 files) |
| **Table V** | `results/attack_scenarios/*_scale66_n10.json` | Attack resilience (5 scenarios) |
| **Figure 5** | `results/convergence/star_qlearning_convergence.json` | Q-learning convergence |
| **Figure 6** | `results/attack_scenarios/rf_jamming_scale66_n10.json` | RF jamming path switching |
| **Figure 7** | `results/attack_scenarios/gs_ddos_scale66_n10.json` | DDoS availability timeline |
| **Figure 8** | All files in `results/scalability/` | CPU utilization scalability |
| **Configuration** | `config/*.yaml` | All constellation parameters |

---

## 🔍 Key Features

### Three-Layer Architecture
1. **Perception Layer**: Lightweight Isolation Forest anomaly detection (8 trees, 24 KB memory)
2. **Decision Layer**: STAR algorithm with distributed Q-learning and game-theoretic routing
3. **Execution Layer**: MH-SKAP protocol for secure multi-satellite handoff (sub-50ms)

### Validation Scale
- **66 satellites**: Baseline validation (Iridium-like)
- **264 satellites**: Medium-scale validation
- **1,056 satellites**: Large-scale validation
- **1,584 satellites**: Starlink Group-4 equivalent

### Attack Scenarios
- RF Jamming (20% user links)
- Routing Manipulation (5% compromised satellites)
- Ground Station DDoS (5 stations at 10 Gbps)
- Coordinated Multi-Vector Attack

---

## 📈 Key Results

### Scalability
- CPU utilization: 22.42% (66 sats) → 33.74% (1584 sats)
- Sub-linear growth: 1.50× CPU for 24× satellites
- Availability stable: 77.67% → 77.66% across all scales

### Attack Resilience
- RF Jamming: -4.53pp degradation (vs -19.68pp for single-path)
- Routing Manipulation: -3.94pp degradation (vs -15.66pp)
- Ground Station DDoS: -3.60pp degradation (vs -11.85pp)
- Coordinated Attack: -10.17pp degradation (vs -35.46pp)

### Performance
- Handoff latency: 45.8±1.2ms (meets <50ms requirement)
- Detection time: 95.1±18.9ms (anomaly detection)
- Recovery time: 10.4±0.9ms (path switching)

---

## 🛠️ Repository Structure

```
RMSA-IPv6-Anonymous-Review/
├── README.md                     # This file (GitHub overview)
├── LICENSE                       # MIT License
├── requirements.txt              # Python dependencies
├── docs/
│   ├── DATA_DOCUMENTATION.md     # Dataset specifications
│   └── COMPLETION_REPORT.md      # Fixes and improvements summary
├── src/
│   ├── main.py                   # Simulation entry point (placeholder)
│   ├── constellation/            # Orbital propagation modules
│   ├── network/                  # Network simulation
│   ├── perception/               # Anomaly detection
│   ├── decision/                 # STAR Q-learning routing
│   ├── execution/                # MH-SKAP protocol
│   └── attacks/                  # Attack scenario implementations
├── config/
│   ├── constellation_66.yaml     # 66-satellite configuration
│   └── constellation_1584.yaml   # 1584-satellite configuration
├── results/
│   ├── scalability/              # Multi-scale experiments
│   ├── attack_scenarios/         # Attack resilience data
│   └── convergence/              # Q-learning convergence
└── scripts/                      # Analysis and plotting scripts
```

---

## 📝 Citation

If you use this code or data in your research, please cite:

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

## 📧 Contact

**For Reviewers**: Questions during review should be directed through the IEEE TAES review system.

**Upon Acceptance**: Full source code, author information, and contact details will be released.

---

## 🔐 License

This code is released under the [MIT License](LICENSE) for research purposes.

---

## ⚠️ Disclaimer

This is an **anonymous repository for peer review**. The full implementation source code will be released publicly upon paper acceptance. The current repository provides:
- Complete experimental data matching all paper results
- Full documentation for reproducibility
- Configuration files with all parameters
- Placeholder code structure showing architecture

**Last Updated**: September 2026  
**Repository Version**: v1.0-review
