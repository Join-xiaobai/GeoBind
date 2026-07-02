# GeoBind

GeoBind is a structure-free, geometry-aware deep learning framework for drug-target modeling. It combines sequence-derived point-cloud descriptors with drug and protein sequence embeddings to support three prediction tasks:

- Drug-target affinity prediction (DTA)
- Drug-target interaction prediction (DTI)
- Mechanism-of-action prediction (MoA)

This repository contains the clean core implementation extracted from `Join-xiaobai/SeqSpaPoint` and renamed to GeoBind. Ablation experiments, case-study materials, and visualization outputs are intentionally removed.

## Repository Structure

```text
GeoBind/
|-- data/                         # Dataset placement folders and format notes
|   |-- dta/                       # Davis and KIBA-style DTA folders
|   |-- dti/                       # Hetionet and Yamanishi_08-style DTI folders
|   `-- moa/                       # Activation and inhibition MoA folders
|-- data_preprocessing/
|   |-- models/                    # Put ChemBERTa and ESM-2 model files here
|   `-- point_cloud_coordinate_construction.py
|-- dataset/
|   `-- interaction_dataset.py
|-- evalMetrics/
|   |-- classification_metrics.py
|   |-- eval_utils.py
|   `-- regression_metrics.py
|-- model/
|   `-- GeoBind.py                 # Core GeoBind model
|-- train/
|   |-- train_dta.py
|   |-- train_dti.py
|   `-- train_moa.py
|-- utils/
|   `-- common_utils.py
|-- main.py                        # Unified entry point
`-- requirements.txt
```

## Installation

Python 3.9 or later is recommended.

```bash
python -m venv .venv
. .venv/bin/activate  # Windows PowerShell: .venv\Scripts\Activate.ps1
pip install -r requirements.txt
```

Install the PyTorch build matching your CUDA environment from the official PyTorch index if GPU acceleration is required.

## External Model Files

GeoBind expects local embedding models under `data_preprocessing/models/`:

- `data_preprocessing/models/esm2_t33_650M_UR50D/`
- `data_preprocessing/models/chemberta/`

See `data_preprocessing/models/models_readme.md` for the expected files.

## Data Layout

Place raw datasets under `data/{task}/{dataset}/`.

Typical labels and ID columns:

- DTA: `drug_id`, `protein_id`, `affinity`
- DTI: `drug_id`, `protein_id`, `label`
- MoA: `DrugID`, `TargetID`, `label`

The release repository does not include large raw datasets or generated outputs. Generated preprocessing files and training results are written locally during runs.

## Usage

Edit `TASK_NAME` and dataset settings in `main.py`, then run:

```bash
python main.py
```

The script checks preprocessing outputs, generates missing sequence embeddings and point-cloud files, then starts cross-validation training for the selected task. Training outputs are written to `result/`.

## Core Model

The main model is implemented in `model/GeoBind.py`. It contains:

- Sequence projection branches for drug and target embeddings
- Sequence-derived point-cloud encoding with enhanced edge convolution
- Drug, target, and joint cross-attention over point-cloud features
- Multi-granularity feature fusion for downstream prediction

## Notes

- `result/` is intentionally ignored and should contain generated training outputs only.
- Large model checkpoints and pretrained model weights should be stored with Git LFS or downloaded separately.
- Removed from the original repository: `ablation_experiment/`, `case_study/`, and `visualize/`.
