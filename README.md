# Data and code: NAA sampling in Paphos and Classic Maya ceramics

Version: v1.0.2. This release includes the manuscript's fixed-budget, fixed-path-dependence and Maya fixed-seed analyses, together with the current Figures 1–3. The GitHub release is https://github.com/yangcd4010/naa-sampling-paphos-maya/releases/tag/v1.0.2. The Zenodo all-versions record is https://doi.org/10.5281/zenodo.23250701; use the version-specific DOI displayed for v1.0.2 when citing this release.

This dataset supports the retrospective evaluation of NAA acquisition in a fixed pool of 97 Paphos amphora fragments and 183 Classic Maya ceramics.

## Start here

`PRIMARY_RESULTS_k2.csv` is the directly readable table of primary two-member results. `SI2_reproducibility.zip` contains the necessary source inputs, reference definitions, acquisition paths, numerical results and executable analyses. Unzip it once. Its `FILE_INVENTORY.csv` lists the payload and SHA-256 digests.

Primary results are in `Revision_Analysis_v5/results/primary_budget_summary.csv`; use `excluded = none` and `k = 2`. Random means are exact expectations conditional on the same fifteen-object starts. Fabric rules average twenty shared orders. Archived random paths are retained for path-level comparisons. Directory labels referenced by Supporting Information 1 remain stable identifiers.

Primary CSV fields: `case` identifies the pool; `target` identifies the reference comparison; `rule` is the algorithm key; `mean_budget` includes the initial fifteen specimens; `CI_low` and `CI_high` are pointwise resampling intervals; `random_mean` is the matched-start exact random expectation; `oracle_mean` is the membership-informed lower bound; `oracle_excess` is mean budget minus that bound; `orders` is the number of fabric orders averaged. `Proportional` is the proportional-medoid implementation. Maya `main` denotes four groups and `terminal` twelve categories.

## Contents and evidence map

| Location | Purpose |
|---|---|
| `Analysis/data` | One consolidated archive of all 9,400 main and sensitivity acquisition configurations, with path metadata and per-path hashes |
| `Analysis/inputs`, `Analysis/selector`, `Analysis/source_measurements` | Starts, reference labels, Paphos source measurements and transformed inputs, selection implementations, external-Maya adapter |
| `Revision_Analysis_v5` | Exact random references, paired differences, regret, cluster sensitivities, pXRF diagnostics and original figure calculations |
| `Revision_Analysis_v4` | Oracle bounds and selected-set score diagnostics; Figure S2 and S49–S51 |
| `Revision_Presentation_v6` | Legacy Paphos presentation entry point; current figures use Figure_Presentation |
| `Figure_Presentation` | Current Figures 1–3, descriptive-name mapping, and the unchanged Figure S4 continuation; reads existing results without changing analyses |
| `Revision_Diagnostics/results` | Category attainment, last-member bottlenecks and screening correspondence for S47 |
| `Submission_Tables` | Table exporter and verified object/metadata crosswalks |
| `Supporting_results` | Original group-coverage and path comparisons, response-panel results, member distributions, source overlap and object-level evidence supporting earlier SI tables |
| `Targeted_Analyses` | Fixed-budget evidence snapshots, all-set and required-member constraint dependence, Maya fixed-seed paths and contrasts |
| `Supporting_analyses` | The 18-object Kourion coverage and feedback experiments, Paphos/Maya full-data reconstruction checks, and frozen-control specification |

The reconstruction checks that failed their stated criteria remain included. Source concentration arrays for Maya are excluded as described below. The acquisition archive is a lossless consolidation of the reported paths; object IDs, order and budgets retain their scientific definitions.

## Reproduction

Use Python 3.12 and install the versions in `requirements.txt`. Run commands from the extracted root. Use a working copy when preserving supplied-file checksums, because analysis programs write calculated results to their result folders.

```text
python Analysis/evaluate_paths.py
python Revision_Analysis_v5/analyse_v5.py
python Revision_Analysis_v5/regret_sensitivities_v5.py
python Revision_Analysis_v4/analyse_revision_v4.py --oracle-only
python Submission_Tables/export_tables.py
python Revision_Presentation_v6/make_fig1_v6.py
python Revision_Analysis_v5/make_figures_v5.py
python Figure_Presentation/make_figure2.py
python Figure_Presentation/readability_figures.py --output Figure_Presentation/figures
python Revision_Analysis_v4/plot_geometry_v4.py
```

Run readability_figures.py last to generate the current Figures 1–3. make_figure2.py retains the previous presentation and generates the unchanged Figure S4 continuation. The last command above for geometry retains the SI figures. The first command reads the single path archive and verifies primary and sensitivity attainment endpoints. The following commands use supplied memberships, endpoints and shared bootstrap arrays. No provider workbook is needed for these commands.

For the primary Paphos selectors:

```text
python Analysis/reproduce_selectors.py --case Paphos --workers 1
```

Add `--stop 1001` for one archived start. The full range is 1001–1100. For Maya, first obtain the source workbook described below, then run:

```text
python Analysis/prepare_maya_inputs.py --source-xlsx PATH_TO_mmc2.xlsx
python Analysis/reproduce_selectors.py --case Maya --workers 1
python Revision_Analysis_v5/diagnose_pxrf_v5.py --maya-input Analysis/selector/maya_inputs.npz
python Revision_Analysis_v4/analyse_revision_v4.py --geometry-input Analysis/selector/maya_inputs.npz
python Supporting_analyses/maya_reconstruction/reconstruction_gate.py
```

The adapter verifies the workbook digest. Keep the reconstructed Maya matrix private; do not add it to a redistributed archive. Supplied acquisition paths and derived summaries allow endpoint evaluation and figure generation without that matrix. Complete Maya selection and feature reconstruction need the external source.

For the earlier Kourion evidence and reconstruction results:

```text
python Supporting_analyses/kourion_coverage/score_and_report.py
python Supporting_analyses/kourion_coverage/check_label_sensitivity.py
python Supporting_analyses/kourion_feedback/evaluate_feedback.py
python Supporting_analyses/paphos_reconstruction/run_judgment_gate.py
```

The corresponding pure selection and replay scripts remain beside their inputs. Both successful and unsuccessful reported outcomes are retained; the script output directory is separate from the public release record.

## Targeted supplementary analyses

This release supplies the targeted supplementary analyses listed here alongside the primary analysis. Primary results and all 8,200 previously archived acquisition configurations are retained; 1,200 Maya fixed-seed paths are added to the same consolidated archive. These additions are result-informed exploratory checks. No new measured specimens, batch-feedback protocol or selection algorithm is introduced.

| Output in `Targeted_Analyses/results` | Interpretation |
|---|---|
| `fixed_budget_evidence_snapshot.csv` | All six Paphos targets at k=2 and total budgets 35, 55 and 75; completion probabilities/fractions, completed sets and pointwise intervals |
| `fixed_path_set_dependence_summary.csv` | All 20 Paphos target-set instances and all 16 Maya broad/fine categories; acquisition positions and waiting under a relaxed set constraint |
| `fixed_path_member_dependence_summary.csv` | All required objects in the nine two-member Paphos sets; candidates and original paths are retained |
| `maya_fixed_seed_budget_summary.csv` | All four MU/DWU arms, three seeds, two resolutions and k=2/3/5 |
| `maya_fixed_seed_contrasts.csv` | Paired updated-minus-frozen differences per seed and after averaging seeds within each initial set |
| `maya_fixed_seed_endpoints.csv` | All 7,200 attainment endpoints from 1,200 new paths |

The compressed `*_by_start` and `*_dependence` CSVs provide complete configuration-level records. Random in the snapshot table denotes the exact conditional completion reference; Archived random denotes actual saved paths. Mean completed sets for exact random sampling is the sum of marginal hypergeometric completion probabilities, not a transformation of whole-target completion. Fabric means and intervals use 100 starts crossed with 20 shared AMG orders. Additional waits are not additive effects and do not simulate unavailable specimens.

Evaluate fixed-budget, dependence and fixed-seed contrasts directly from the supplied paths, without provider matrices or execution checkpoints:

```text
python Targeted_Analyses/fixed_path_diagnostics.py
python Targeted_Analyses/summarize_fixed_seeds.py
```

To rerun Maya fixed seeds, obtain the source workbook and create `Analysis/selector/maya_inputs.npz` with the existing adapter. Keep that local matrix outside any redistributed package. Then run:

```text
python Targeted_Analyses/maya_fixed_seeds.py --matrix Analysis/selector/maya_inputs.npz --smoke --output local_fixed_seed_checkpoints
python Targeted_Analyses/maya_fixed_seeds.py --matrix Analysis/selector/maya_inputs.npz --output local_fixed_seed_checkpoints --workers 8
python Targeted_Analyses/summarize_fixed_seeds.py --checkpoints local_fixed_seed_checkpoints
```

The smoke check reproduces the original scheduled-seed paths for starts 1001, 1050 and 1100. Fixed seeds are 202610041, 202610042 and 202610043, constant across budgets and matched arms. Default selector calls retain original scheduling. Tree queries are converted once to the same float32 representation used by scikit-learn, avoiding repeated validation without changing tree predictions. Summaries average the three settings within each start before resampling 100 starts; 300 start–seed combinations are not treated as independent archaeological samples. Checkpoint JSON files are local execution aids and are excluded from this archive.

## Current readable presentation

The current Figure 1 replaces the previous Figure 2. Its member schematics read the original Paphos object crosswalks; positions represent membership, not chemical distance. All five SICILY and eight CYP-B objects are retained, including grey non-target objects marked †. The heatmap shows all 60 primary completion values at 55 specimens and k=2 directly from `Targeted_Analyses/results/fixed_budget_evidence_snapshot.csv`. Nonzero/partial values are protected from being displayed as 0%/100%. Original attainment curves and pointwise intervals are unchanged. Results at 35/75 specimens remain in that scientific CSV and SI1 S43.

Current Figure 2 presents the previous Figure 1's Paphos paired differences. Figure 3 retains the Maya budgets and contrasts. `Figure_Presentation/readability_figures.py` reads the existing scientific summaries for all three figures; `readability_names.json` records cosmetic names without changing algorithm IDs. No analysis, path, input matrix or original source CSV is regenerated or duplicated. Independent submission figures are 180 mm wide, 600 dpi TIFF with a 7 pt font floor; vector PDF/SVG versions are supplied for reproducible presentation.

```text
python Figure_Presentation/readability_figures.py --output Figure_Presentation/figures
```

The v1.0.2 archive contains both the targeted supplementary analyses and the current presentation. QA JSON, manuscript revision records, proof images and local execution checkpoints are excluded from the upload package.

## Sources and rights

Paphos measurement workbooks: Hein, Dobosz and Kilikoglou (2021), Mendeley Data, version 1, https://doi.org/10.17632/ydgs3z4g76.1, CC BY 4.0. Reference assignments follow https://doi.org/10.1016/j.jas.2021.105436. Source metadata distinguish published assignments and later database labels.

Maya: LeMoine and Halperin (2021), https://doi.org/10.1016/j.jasrep.2021.102819, supplement mmc2. The original workbook and full raw/standardized concentration matrices are not redistributed. Obtain them under the provider's terms; the adapter and source identifiers are supplied. No publisher article PDFs or full-text extracts are included.

Author-created data, derived results and documentation are licensed under Creative Commons Attribution 4.0 International (CC BY 4.0): https://creativecommons.org/licenses/by/4.0/. Author-created software code is licensed under the MIT License. The full MIT terms and the scope of each licence are provided in `LICENSES.md` inside `SI2_reproducibility.zip`. These grants apply only to rights held by the package contributors. Third-party materials retain their original licences; neither grant overrides provider terms or covers the excluded Maya source workbook and concentration matrices.
