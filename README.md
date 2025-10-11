## Overview

This project integrates quantum chemical calculations, molecular descriptor extraction, and machine learning modeling to investigate the mechanism of polyethylene (PE) formation in Cr/PNP-catalyzed ethylene oligomerization.

The workflow consists of three main modules:

1. Quantum Chemical Calculations (DFT and xTB)
2. Descriptor Extraction (electronic, steric, geometric, RDKit, and ACSF)
3. Machine Learning Modeling (classification and regression)

---

## 1. Quantum Chemical Calculations

All quantum chemical calculations were performed using Gaussian 09 and GFN2-xTB.

### Folder
Quantum Chemical Calculations/

### Subdirectories

#### (a) gaussian/
Contains Gaussian input/output files and SLURM submission scripts.

Files:
- run_g09e01.sh – execution script for Gaussian jobs  
- sub_g09e01.sh – SLURM submission script for Gaussian jobs  

Usage:
```bash
sbatch sub_g09e01.sh
```

This command submits the Gaussian job to the HPC cluster.

#### (b) xtb/
Contains xTB and CREST scripts for conformer generation and pre-optimization.

Files:
- run_xtb.sh – xTB execution script  
- sub_xtb.sh – SLURM submission script  

Usage:
```bash
sbatch sub_xtb.sh
```

Both Gaussian and xTB calculations produce optimized geometries and energy outputs used for descriptor generation.

---

## 2. Descriptor Extraction

Folder:
Descriptor Extraction/

This module automatically extracts multiple categories of molecular descriptors based on optimized structures.

### Descriptor Categories

| Category | Script | Description |
|-----------|--------|-------------|
| Electronic | calculate_electronic-parameter.py | NBO charge, HOMO/LUMO, bond energy |
| Steric | calculate_buried.py, calculate_coneAngle.py, calculate_sterimol.py | Buried volume, cone angle, Sterimol parameters |
| Geometric | calculate_geometric-parameter.py | Bond distances, angles, bite angles |
| RDKit | calculate_RDKIT_descriptor.py | Molecular fingerprints and topological descriptors |
| ACSF | calculate_ACSF.py | Atom-centered symmetry functions |

### Helper Scripts
- create-conformerANDfolder.py – generates folders and conformers  
- calculate_boltzmann_weights.py – computes Boltzmann-weighted averages  
- extract_R1_AND_H_index.py – extracts atomic indices for descriptor mapping  

### Export Results
After calculation, results can be converted into CSV format:
```bash
python buried-results_toCSV.py
python electronic-results_toCSV.py
python ACSF_results_toCSV.py
```

### Example Usage
Run any descriptor calculation:
```bash
python calculate_buried.py
```
All descriptor results are saved in the results/ subfolder.

---

## 3. Machine Learning Modeling

Folder:
Machine learning/

This module contains Jupyter notebooks for training and evaluating the ML models.

Files:
- PE-classification.ipynb – Classifies catalysts into low-PE (<1 wt%) and high-PE (≥1 wt%)  
- PE-regression.ipynb – Predicts quantitative PE content for low-PE systems  

### Algorithms Used
Classification: Decision Tree, Random Forest, Logistic Regression  
Regression: Linear Regression, Ridge, LASSO, SVR, XGBoost  

### Evaluation Metrics
Classification: Accuracy  
Regression: R², MAE  
Validation: 5-fold cross-validation  

### Example Usage
Open Jupyter notebooks and execute step-by-step:
```bash
jupyter notebook PE-classification.ipynb
```

Each notebook outputs:
- Feature importance (SHAP plots)  
- Predicted vs. experimental PE content  
- Model performance summaries  

---
