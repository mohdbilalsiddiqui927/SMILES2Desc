# ⬡ SMILES2Desc

![Python](https://img.shields.io/badge/Python-3.10%2B-blue?style=flat-square&logo=python)
![Streamlit](https://img.shields.io/badge/Streamlit-1.32%2B-FF4B4B?style=flat-square&logo=streamlit)
![RDKit](https://img.shields.io/badge/RDKit-2023.9%2B-brightgreen?style=flat-square)
![License](https://img.shields.io/badge/License-MIT-yellow?style=flat-square)
![Status](https://img.shields.io/badge/Status-Publication%20Ready-00d4a0?style=flat-square)

> **Molecular descriptor generation from SMILES strings — RDKit · Mordred · PaDEL**

SMILES2Desc is a publication-ready Streamlit application for generating 2D molecular descriptors from SMILES input. It supports three descriptor engines, handles validation, deduplication, InChIKey generation, and exports structured results in CSV or multi-sheet Excel format.

---

## Features

| Feature | Details |
|---|---|
| **Three engines** | RDKit (209), Mordred (1613), PaDEL (2326) descriptors |
| **SMILES validation** | Invalid SMILES detected and reported separately |
| **Canonical SMILES** | Normalized via RDKit before processing |
| **InChIKey** | Generated per molecule for universal cross-referencing |
| **Deduplication** | Duplicate SMILES removed before processing |
| **Zero-variance filter** | Non-informative descriptor columns removed automatically |
| **2D structure preview** | Grid image of first 12 molecules |
| **Descriptor statistics** | Mean, std, min, max, missing% per descriptor |
| **Excel export** | 3 sheets: Descriptors · Metadata · Failed SMILES |
| **Reproducibility** | All library versions, OS, and timestamp saved in metadata |

---

## Installation

### Option A — pip

```bash
git clone https://github.com/YOUR_USERNAME/SMILES2Desc.git
cd SMILES2Desc
pip install -r requirements.txt
```

### Option B — conda (recommended)

```bash
git clone https://github.com/YOUR_USERNAME/SMILES2Desc.git
cd SMILES2Desc
conda env create -f environment.yml
conda activate smiles2desc
```

---

## Running the App

```bash
streamlit run app.py
```

The app opens at `http://localhost:8501` in your browser.

---

## Input Format

Upload a CSV file with at minimum a column named **`smiles`**. Any additional columns (compound ID, activity values, etc.) are preserved in the output.

```csv
smiles,compound_id,compound_name,activity
CCO,mol_001,Ethanol,0.12
CC(=O)O,mol_002,Acetic acid,0.45
c1ccccc1,mol_003,Benzene,0.08
```

A test file with 60 drug-like molecules is included: [`test_smiles.csv`](test_smiles.csv)

---

## PaDEL Setup

PaDEL requires **Java 8 or higher** installed and on your system PATH.

**Step 1** — Install Java if not already installed:
- Windows: https://www.java.com/en/download/
- Linux: `sudo apt install default-jre`
- macOS: `brew install openjdk`

**Step 2** — Download PaDEL-Descriptor:
- https://www.yapcwsoft.com/dd/padeldescriptor/
- Extract and note the paths to `PaDEL-Descriptor.jar` and `descriptors.xml`

**Step 3** — Set paths (choose one method):

*In the sidebar* (per session):
```
JAR path:  C:\padel\PaDEL-Descriptor.jar
XML path:  C:\padel\descriptors.xml
```

*As environment variables* (permanent):
```bash
# Windows PowerShell
[System.Environment]::SetEnvironmentVariable("PADEL_JAR_PATH", "C:\padel\PaDEL-Descriptor.jar", "User")
[System.Environment]::SetEnvironmentVariable("PADEL_XML_PATH", "C:\padel\descriptors.xml", "User")

# Linux / macOS
export PADEL_JAR_PATH=/path/to/PaDEL-Descriptor.jar
export PADEL_XML_PATH=/path/to/descriptors.xml
```

---

## Output

### CSV mode
| File | Contents |
|---|---|
| `SMILES2Desc_descriptors_<ts>.csv` | All descriptors + metadata columns |
| `SMILES2Desc_metadata_<ts>.csv` | Run metadata (versions, counts, settings) |
| `SMILES2Desc_failed_<ts>.csv` | Failed / skipped SMILES with reasons |

### Excel mode (recommended for publication)
| Sheet | Contents |
|---|---|
| `Descriptors` | Full descriptor table |
| `Metadata` | Library versions, timestamp, settings |
| `Failed SMILES` | Invalid or oversized molecules |

---

## Reproducibility

Every run captures the following in the Metadata output:

```
tool                  SMILES2Desc v1.0
descriptor_types_used RDKit, Mordred, PaDEL
rdkit_version         2023.9.5
mordred_version       1.2.0
padel_version         PaDEL-Descriptor 2.21
python_version        3.10.12
operating_system      Windows-11-...
timestamp_utc         2025-01-15T10:32:44.123456
input_molecules       60
successful_molecules  60
failed_molecules      0
total_descriptors     4148
```

---

## Repository Structure

```
SMILES2Desc/
├── app.py                  # Main Streamlit application
├── requirements.txt        # pip dependencies
├── environment.yml         # conda environment
├── test_smiles.csv         # 60-molecule test dataset
├── .gitignore              # Git ignore rules
├── LICENSE                 # MIT License
└── README.md               # This file
```

---

## Dependencies

| Package | Version | Purpose |
|---|---|---|
| `streamlit` | ≥1.32.0 | Web UI framework |
| `rdkit` | ≥2023.9.1 | RDKit descriptors, SMILES parsing, structure drawing |
| `mordred` | ≥1.2.0 | Mordred 2D descriptor calculator |
| `pandas` | ≥2.0.0 | Data handling |
| `openpyxl` | ≥3.1.2 | Excel export |
| `Pillow` | ≥10.0.0 | Image rendering |
| Java 8+ | external | Required for PaDEL only |

---

## Citation

If you use SMILES2Desc in your research, please cite the underlying descriptor libraries:

- **RDKit**: Landrum, G. et al. *RDKit: Open-source cheminformatics*. https://www.rdkit.org
- **Mordred**: Moriwaki, H. et al. *Mordred: a molecular descriptor calculator*. J Cheminform 10, 4 (2018). https://doi.org/10.1186/s13321-018-0258-y
- **PaDEL**: Yap, C.W. *PaDEL-descriptor: An open source software to calculate molecular descriptors and fingerprints*. J Comput Chem 32, 1466–1474 (2011). https://doi.org/10.1002/jcc.21707

---

## License

This project is licensed under the MIT License — see [`LICENSE`](LICENSE) for details.
