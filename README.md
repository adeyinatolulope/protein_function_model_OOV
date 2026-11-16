# protein_function_model_OOV

Code and result tables for the manuscript *Vocabulary recurrence, rather than biological grounding, explains tokenization transfer in protein function prediction* (Tolulope S. Adeyina and Jonathon E. Mohl; manuscript in preparation).

## What this repository contains

The analysis compares four ways of turning a protein sequence into features for predicting six Gene Ontology (GO) terms:

- tryptic peptides (cleavage after K or R, not before P)
- overlapping 3-mers
- peptide "topics" (clusters of peptide embeddings)
- amino-acid composition

Two reference points are included: mean-pooled ESM-2 (650M) embeddings and homology transfer (best MMseqs2 hit). Every representation is scored on the same fixed test set. The notebook also measures how often each tokenization's vocabulary recurs in held-out proteins, and how much sequence identity remains between the test and training proteins.

The six GO terms are GO:0002933 (lipid hydroxylation), GO:0004608 (phosphatidylethanolamine N-methyltransferase activity), GO:0004653 (polypeptide N-acetylgalactosaminyltransferase activity), GO:0004713 (protein tyrosine kinase activity), GO:0015977 (carbon fixation) and GO:0043885 (anaerobic carbon-monoxide dehydrogenase activity). They were not chosen by a formal criterion, so the study should be read as a case study.

## Layout

```
notebooks/peptide_pipeline_v3.ipynb   the full pipeline, from download to final tables and figures
docs/peptide_pipeline_v3_executed.html  the Colab run behind the manuscript numbers, with outputs
results/tables/                        derived result tables (CSV)
requirements.txt                       software versions reported in the manuscript
CITATION.cff, .zenodo.json             citation metadata
LICENSE                                MIT
```

The executed notebook in `docs/` is a record of the run, with its printed outputs. Download it and open it in a browser; GitHub shows only its source.

## Running the notebook

**Google Colab (tested).** Open the notebook in Colab and run it top to bottom. Outputs and checkpoints are written to `MyDrive/Thesis/v3` on Google Drive. Every stage checkpoints, so a timed-out session resumes where it stopped. The all-versus-all search (section 3) and the ESM-2 embeddings (section 11, which needs a GPU) are the slowest stages.

**Elsewhere.**

```bash
pip install -r requirements.txt
# install MMseqs2 so that `mmseqs` is on your PATH
export TOKENIZATION_ROOT=/path/to/output/folder     # tables, figures and checkpoints
export TOKENIZATION_WORK=/path/to/fast/local/disk   # MMseqs2 temporary files
jupyter notebook notebooks/peptide_pipeline_v3.ipynb
```

If `mmseqs` is not found, the notebook downloads the latest Linux AVX2 build. The manuscript results used build `17b688d21dda57fc5f5b7286ecba7ec003d4717f`; install that build yourself if you want to match it exactly. All random seeds are fixed at 42.

## Data

Sequences are not redistributed. Section 1 of the notebook retrieves them from UniProtKB with plain `go:XXXXXXX` queries for the six terms, with no reviewed-status or free-text filter. The manuscript used UniProt release 2026_02. The notebook queries the current release, so a fresh download can differ slightly from the corpus used in the manuscript.

To identify the proteins used, section 16 of the notebook writes `results/tables/corpus_accessions_and_split.tsv`: accession, GO class, database, fragment flag and train/test assignment, with no sequences.

Section 4c of the notebook checks that the split reproduces the one reported in the manuscript (fold 3, 3,181 test proteins, 2,636 clusters) and prints a warning if it does not.

## Result tables

The notebook writes every table to `results/tables/`. Where a table appears in the manuscript:

| File | Manuscript |
|---|---|
| `T1_corpus_composition.csv` | Methods, corpus |
| `T2_component_structure.csv` | Table 1 |
| `T3a_component_folds.csv`, `T3b_cluster_folds.csv` | Methods, split construction |
| `T4_split_composition.csv` | Table 2 |
| `T5_identity_distribution.csv` | Results 3.4 |
| `T6_coverage_conditioned.csv` | Table 4 |
| `T7_accuracy_by_coverage.csv` | Results, coverage |
| `T8_min_count_sensitivity.csv` | Table 5 |
| `T9_identity_stratified.csv`, `T10_stratum_class_composition.csv`, `T11_within_class_stratified.csv` | Results 3.4 |
| `T12_esm2_per_class.csv` | ESM-2 per-class results |
| `T13_summary_all_arms.csv` | Table 3 |
| `T13_classifier_robustness.csv` | Table 6 and Table S1 |
| `T14_abstention_by_classifier.csv` | Table S2 |
| `S3_per_class_precision_recall.csv` | Table S3 |

## Citation

Please cite the manuscript and the archived version of this repository. A DOI for the archive will be added here once the first release is deposited.

## License

MIT. See `LICENSE`.
