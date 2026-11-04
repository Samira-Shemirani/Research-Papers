# Source Code: Fixed Molecular Graph Features for Multi-Type DDI Prediction

This directory contains the three **molecular preprocessing notebooks** accompanying
*From Fingerprints to Molecular Graphs: Exploring Fixed Graph-Convolutional Features for Multi-Type Drug–Drug Interaction Prediction*.

The notebooks produce **fixed, randomly initialized graph-convolutional descriptors** for drugs, for downstream use as chemical features in SumGNN. The molecular encoders are **not trained** in these notebooks. These notebooks are **not a complete end-to-end reproduction package** for the reported DDI results.

## Experiments

| Directory | Notebook | Molecular encoder | Output |
| --- | --- | --- | --- |
| `simple_gcn/` | `Preprocess1.ipynb` | 78 → 256 → 1024, mean pooling | 1024-D, standardized |
| `drug_gcn/` | `Preprocess2.ipynb` | 78 → 256 → 512 → 1024, max pooling, dense projection | 1024-D, standardized |
| `drug_gcn_pca/` | `Preprocess3.ipynb` | 78 → 256 → 512 → 1024, max pooling, dense projection to 256, PCA | 200-D |

## Using these notebooks

1. Create a Python environment with dependencies used in the notebooks: PyTorch, PyTorch Geometric, RDKit, NumPy, pandas, scikit-learn, h5py, networkx, pubchempy, tqdm, and joblib (for PCA). Exact package versions have not been established from the archived notebook files.
2. Obtain the **licensed / authorized** DrugBank-derived molecular input separately. The notebooks expect a file named `molecular_features.csv`. Place a local copy in the respective experiment directory; do not commit data that you are not permitted to redistribute.
3. Start Jupyter **with the current working directory set to the chosen experiment directory**, and run its notebook in order. Each notebook's original machine-specific `folder` path has been changed to use the current working directory.
4. Generated `.pt`, `.npy`, `.pkl` and `PCA.txt` files are local artifacts. The GCN features are generated offline and subsequently used in a separate SumGNN implementation. **These files are not required for code browsing** and are excluded here.

### Publication changes made to the original notebooks

- Cleared executed cell outputs and execution counts, for privacy and a readable code history.
- Replaced the absolute personal Windows folder path with the current working directory.
- Did **not** change the model architectures, data-processing operations, or training logic.
- The notebooks have **not been rerun or validated** after these presentation-only changes.

## Missing pieces for full reproduction

The archived notebooks do not provide all inputs, dependencies, exact random seeds, modified SumGNN training/evaluation source, selected classifier checkpoints, exact run commands, or an auditable mapping from each recorded result to an output artifact. These must be supplied and verified separately to support a complete reproducibility claim. The PCA/scaler steps are fit on the available molecular collection; this is **not** a strict unseen-drug protocol.

## Data and code provenance

This work builds on the public SumGNN method (Yu et al., Bioinformatics, 2021) and draws inspiration for molecular preprocessing from XGDP (Wang et al., Scientific Reports, 2025). Credit the original publications and check licenses of any reused third-party code. Do **not** mirror original XGDP expression datasets or DrugBank data without reviewing their licenses. The original `XGDP(1).zip` is intentionally excluded because its cancer-cell-line data are not part of these DDI experiments.

## Reported single-run paper results (not independently reproduced from this package)

| Descriptor | Macro-F1 (%) | Accuracy (%) | 100 × Cohen's κ |
| --- | ---: | ---: | ---: |
| Local Morgan baseline | 82.85 | 91.70 | 90.13 |
| Simple GCN | 83.32 | 91.66 | 90.09 |
| Drug GCN | 85.13 | 91.78 | 90.22 |
| Drug GCN + PCA | 84.59 | 91.97 | 90.46 |

These are single-run values as stated in the supplied manuscript. This repository does not establish statistical significance or complete run-level provenance.
