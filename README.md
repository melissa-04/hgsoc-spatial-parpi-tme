# HGSOC Spatial Transcriptomics — Tumour Microenvironment Re-analysis

A learning project. I am re-analysing public Visium data from high-grade serous
ovarian carcinoma (HGSOC) to practise a full spatial transcriptomics workflow,
and to ask one applied question: **which cell–cell signals present in intact
tissue are absent from epithelial-only organoid culture?**

Nothing here is a novel finding. The aim is a careful, well-documented analysis.

---

## Background

PARP inhibitors (PARPi) improve outcomes in HGSOC, but 40–70% of patients
develop resistance. Targeted sequencing of DNA-repair genes explains only a
minority of cases (Burdett et al., *Sci Rep* 2023), which points towards
transcriptional and microenvironmental contributions instead.

Suzuki et al. (*npj Precis Oncol* 2025) profiled 8 HGSOC tumours with Visium and
reported increased midkine (MDK) signalling from CAFs to malignant spots in the
PARPi-resistant group. This project re-analyses that dataset with an independent
pipeline.

---

## Aims

1. Rebuild the analysis with different tools and check whether the MDK finding
   reproduces (positive control for the pipeline).
2. Apply pseudobulk statistics and test sensitivity to covariates
   (tissue site, slide batch, prior treatment).
3. Define spatial regions (tumour core / interface / stroma) from neighbourhood
   composition, and ask where signalling is concentrated.
4. List ligand–receptor pairs whose ligand is stromal or immune in origin —
   i.e. axes that cannot occur in epithelial-only organoid monoculture.

---

## Data

Data are **not** in this repository. All datasets are public.

| Accession | Platform | n | Role | Source |
|---|---|---|---|---|
| [GSE288483](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE288483) | Visium FFPE | 8 sections / 8 patients | Primary dataset (PARPi response known) | GEO |
| [GSE211956](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE211956) | Visium + scRNA-seq | 8 sections + 5 scRNA samples | scRNA reference; independent Visium cohort | GEO |

Run `notebooks/01_download_data.ipynb` to fetch them.

---

## Notebooks

Run in order.

| Notebook | Purpose |
|---|---|
| `00_setup.ipynb` | Environment, folders, version logging |
| `01_download_data.ipynb` | Download from GEO |
| `02_load_visium.ipynb` | Build AnnData objects |
| `03_qc_visium.ipynb` | Quality control and filtering |
| `04_scrna_reference.ipynb` | Annotate scRNA reference |
| `05_deconvolution.ipynb` | Spot-level cell type composition |
| `06_spatial_domains.ipynb` | Neighbourhood-based region labels |
| `07_differential_expression.ipynb` | Pseudobulk DE |
| `08_cell_communication.ipynb` | Ligand–receptor analysis |
| `09_synthesis.ipynb` | Summary and organoid-relevant output |

---

## Environment

Google Colab. Package versions in `environment/requirements.txt`.
Per-session logs in `environment/session_logs/`.

---

## Known limitations

- n = 8 patients. Hypothesis-generating only.
- Samples were taken before PARPi, so they reflect the pre-treatment state
  rather than an acquired resistance mechanism.
- Tissue site, prior treatment and slide batch are unbalanced between groups
  and cannot be fully separated with 8 samples.
- The Visium data are FFPE probe-based (17,943 genes) while the scRNA reference
  is fresh-tissue polyA (33,538 genes); deconvolution uses their intersection.

---

## References

- Suzuki H. *et al.* Exploring drug resistance via intercellular crosstalk using
  spatial transcriptomics in high-grade serous ovarian carcinoma.
  *npj Precis Oncol* 2025. doi:10.1038/s41698-025-01122-1
- Burdett N.L. *et al.* Small-scale mutations are infrequent as mechanisms of
  resistance in post-PARP inhibitor tumour samples in high grade serous ovarian
  cancer. *Sci Rep* 2023. doi:10.1038/s41598-023-48153-x

---

## Author

[Melisa Ağrı] — [Bogaziçi university]
Supervised by Emine Kazanç
