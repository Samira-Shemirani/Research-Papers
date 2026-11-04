# From Fingerprints to Molecular Graphs

**Exploring Fixed Graph-Convolutional Features for Multi-Type Drug–Drug Interaction Prediction**

**Authors:** Samira Ahmadzadeh Shemirani and Eghbal Mansoori  
**Research area:** Drug–drug interaction prediction · Molecular representations · Graph neural networks · Biomedical knowledge graphs

## Overview

This research investigates whether fixed features extracted from molecular graphs can replace the **1,024-bit Morgan fingerprints** used in the chemical feature channel of **SumGNN** for multi-type drug–drug interaction (DDI) prediction.

Drug molecules are constructed from SMILES as graphs, with atoms represented as nodes and bonds as connections. Three graph-convolutional pipelines produce descriptors **offline**. The molecular encoders use their randomly initialized weights without training; their descriptors are then intended as fixed inputs to a separately trained SumGNN DDI classifier.

The goal is to evaluate this feature substitution in the reported DrugBank-based multiclass setting, **not** to propose an end-to-end trained molecular GNN.

## Code organization

| Folder | Notebook | Description |
| --- | --- | --- |
| [`code/simple_gcn/`](code/simple_gcn/) | [`Preprocess1.ipynb`](code/simple_gcn/Preprocess1.ipynb) | Two GCN layers and mean pooling; 1,024-dimensional descriptors |
| [`code/drug_gcn/`](code/drug_gcn/) | [`Preprocess2.ipynb`](code/drug_gcn/Preprocess2.ipynb) | Three GCN layers, max pooling and dense projections; 1,024-dimensional descriptors |
| [`code/drug_gcn_pca/`](code/drug_gcn_pca/) | [`Preprocess3.ipynb`](code/drug_gcn_pca/Preprocess3.ipynb) | Three GCN layers, max pooling, a 256-dimensional projection and PCA; 200-dimensional descriptors |

See [`code/README.md`](code/README.md) for additional notes on the notebooks, inputs and limitations.

## Molecular representation settings

| Configuration | Graph convolution widths | Graph pooling | Final descriptor |
| --- | --- | --- | --- |
| Morgan baseline | Not applicable | Not applicable | 1,024-bit fingerprint |
| Simple GCN | 78 → 256 → 1,024 | Mean | 1,024 standardized features |
| Drug GCN | 78 → 256 → 512 → 1,024 | Max | 1,024 standardized features |
| Drug GCN + PCA | 78 → 256 → 512 → 1,024 | Max | 200 PCA components |

The input atomic vectors have 78 features. The supplied graph-convolutional notebooks do not explicitly use bond type or stereochemistry as edge features.

## Reported results

The following are **single-run results reported in the accompanying manuscript**, compared against its *local* SumGNN–Morgan baseline. They have **not been independently reproduced from the public notebook package**.

| Molecular representation | Macro-F1 (%) | Accuracy (%) | 100 × Cohen's κ |
| --- | ---: | ---: | ---: |
| Local SumGNN + Morgan | 82.85 | 91.70 | 90.13 |
| Simple GCN | 83.32 | 91.66 | 90.09 |
| **Drug GCN** | **85.13** | 91.78 | 90.22 |
| Drug GCN + PCA | 84.59 | **91.97** | **90.46** |

The largest recorded macro-F1 difference from the local baseline was **+2.28 percentage points** for Drug GCN. Because the configurations also differ in network depth, pooling and/or dimensionality, and each was recorded only once, the table does not isolate a causal effect or establish statistical significance.

## Exploring the notebooks

1. Open one of the notebooks linked above, or clone this repository and navigate to the corresponding directory under `code/`.
2. Prepare an authorized DrugBank-derived molecular input file named `molecular_features.csv` in that notebook's working directory. **The data file is not distributed here.**
3. Set up a compatible Python environment. The notebooks use libraries including PyTorch, PyTorch Geometric, RDKit, NumPy, pandas and scikit-learn. Exact dependency versions for the recorded experiments have not been verified.
4. Run the notebook in order to generate local graph data and descriptor artifacts. Review the notebook's file paths and expected input columns before execution.

**Important:** The notebooks were cleaned for publication (saved outputs removed and an absolute local path replaced with the current working directory) but have **not been rerun and validated** after those edits.

## Reproducibility and limitations

- This directory currently publishes the **three offline molecular feature extraction notebooks**, not a complete end-to-end training and evaluation package for the reported SumGNN experiments.
- A complete reproduction would require the modified SumGNN classifier/training code, external knowledge-graph and DDI inputs, exact split files, dependency versions, run configurations, random seeds, checkpoints and a verified mapping from each reported result to its artifacts.
- Standardization and PCA were fitted to the available molecular collection in these notebooks. These results should **not** be interpreted as evaluation under a strict unseen-drug or cold-start protocol.
- The single-run comparisons do not establish statistical significance; the effects of architecture, pooling, scaling and output dimension have not been separately controlled.
- DrugBank-derived inputs and other third-party datasets are not mirrored here. Consult their access terms and redistribution permissions before obtaining or sharing them.

## Research foundations

- **SumGNN:** Yu et al. (2021), *SumGNN: Multi-typed Drug Interaction Prediction via Efficient Knowledge Graph Summarization*, *Bioinformatics*. [DOI](https://doi.org/10.1093/bioinformatics/btab207) · [Original source code](https://github.com/yueyu1030/SumGNN)
- **XGDP:** Wang et al. (2025), *Drug Discovery and Mechanism Prediction with Explainable Graph Neural Networks*, *Scientific Reports*. [DOI](https://doi.org/10.1038/s41598-024-83090-3)

The research builds on these published methods. Please credit the original authors when reusing their ideas or code, and review applicable licenses for third-party source and data.

## Manuscript

*From Fingerprints to Molecular Graphs: Exploring Fixed Graph-Convolutional Features for Multi-Type Drug–Drug Interaction Prediction* — accompanying research manuscript. Publication details and a formal citation will be added when available.
