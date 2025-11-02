# PhaseBO: Accelerated Exploration of Compositional Space with Bayesian Optimization

_A. Vasylenko, 3 Nov 2025_

**PhaseBO** couples Bayesian optimization (BO) with density functional theory (DFT)–based crystal structure prediction (CSP) to accelerate the discovery of stable inorganic compositions.  
CSP acts as a computational “experiment” assessing each candidate composition’s stability, while BO actively selects the next compositions to evaluate, guided by the evolving energy–composition landscape.  

This closed-loop strategy reduces the number of required DFT calculations by up to **50% compared to random or clustering-based exploration**, enabling faster and more efficient mapping of complex chemical phase fields.

This repository accompanies the publication:  
**A. Vasylenko *et al.***, “Inferring energy–composition relationships with Bayesian optimization enhances exploration of inorganic materials,” *J. Chem. Phys.* **160**, 054110 (2024).  

The framework can be extended to incorporate custom kernels and acquisition functions for domain-specific active learning in materials discovery.
---

## Overview

PhaseBO infers energy–composition relationships within a defined phase field and identifies new, promising compositions for further CSP or synthesis.  
It leverages the [GPyOpt](https://github.com/SheffieldML/GPyOpt) Bayesian optimization framework and extends it to the materials-discovery context by integrating:
- Composition-aware candidate generation and exclusion rules
- Interface to total-energy datasets from DFT
- Configurable exploration and exploitation strategies for active learning in composition space

---

## Functionality

Three operational modes are available:

| Mode | Description |
|------|--------------|
| `'path'` | Reconstructs a “would-be” Bayesian optimization path for previously computed compositions in a phase field. |
| `'suggest'` | Suggests new candidate compositions for DFT/CSP based on prior evaluations. |
| `'generate'` | Generates an editable list of candidate compositions for use in later `'suggest'` runs. |

---

## Requirements

- Python ≥ 3.9  

### Dependencies
`numpy`, `pandas`, `pymatgen`, `scikit-learn`, `GPyOpt`  
(Installed automatically via `pip install .`)

---

## Installation

```bash
pip install .
```
---

## Usage

1. Prepare a two-column `.csv` file containing compositions and their total energies.  
   Include at least reference compositions in the phase field.  
2. Modify `input_config.yaml` to define atom types, oxidation states, and input file names.  
3. Run:

   ```bash
   python -m phasebo
   ```

   By default, this uses `input_config.yaml`, or specify a custom configuration:

   ```bash
   python -m phasebo --config path/to/my_config.yaml
   ```

Example outputs are provided in the `example/` directory.

---

## Configuration Parameters

| Parameter | Description |
|------------|-------------|
| `mode` | (`'suggest'`) Operation mode (`'path'`, `'suggest'`, `'generate'`). |
| `inputfile` | (`LiSnSCl_700eV.csv`) CSV file with compositions and total energies. |
| `compositionfile` | Optional list of candidate formulas to restrict exploration. |
| `excludefile` | Optional list of compositions to exclude from convex-hull and candidate generation. |
| `ions` | (`{'Li':1, 'Sn':4, 'S':-2, 'Cl':-1}`) Dictionary of elements and oxidation states. |
| `seeds_type` | (`'random'`) Strategy for selecting initial seeds (`'segmented'` or `'random'`). |
| `disect` | (`4`) Number of phase-field segments for `'segmented'` seeding. |
| `N_atom` | (`24`) Maximum number of atoms per unit cell. |
| `max_iter` | (`10`) Number of BO iterations. |

---

## Example

A default run using `input_config.yaml` produces outputs in the `example/` folder.

---

## Citation

If you use this code, please cite:

> A. Vasylenko *et al.*, “Inferring energy–composition relationships with Bayesian optimization enhances exploration of inorganic materials,” *J. Chem. Phys.* **160**, 054110 (2024).

and also:

> The GPyOpt authors, *GPyOpt: A Bayesian Optimization framework in Python* (2016). [https://github.com/SheffieldML/GPyOpt](https://github.com/SheffieldML/GPyOpt)

---

## Project Structure

```
phasebo/
│
├── phasebo/                # Core package
│   ├── __init__.py
│   ├── main.py
│   ├── utils.py
│   └── ...
├── example/                # Example datasets and results
├── input_config.yaml
├── setup.py
└── README.md
```

---

## License

MIT License © 2025 Andrij Vasylenko  
Includes adapted components from GPyOpt (MIT License).
