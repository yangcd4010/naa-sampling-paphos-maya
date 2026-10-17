# NAA sampling in Paphos and Classic Maya ceramics: data and numerical analyses

Version: v1.0.6

This archive supports a retrospective evaluation of NAA acquisition in a fixed pool of 97 Paphos amphora fragments and 183 Classic Maya ceramics. It contains source inputs that may be redistributed, reference-member definitions, acquisition paths, numerical analysis code, result tables and finished figures. Measurements, reference memberships, acquisition paths and reported numerical results are unchanged from v1.0.4.

Repository release: https://github.com/yangcd4010/naa-sampling-paphos-maya/releases/tag/v1.0.6

Zenodo version history: https://doi.org/10.5281/zenodo.23250701. Select v1.0.6 and use its version-specific DOI when citing this release.

## Files and first steps

The repository contains three files:

| File | Contents |
|---|---|
| `PRIMARY_RESULTS_k2.csv` | Primary two-member results in a directly readable table |
| `README.md` | This guide |
| `SI2_reproducibility.zip` | Numerical code, data, results, finished figures and an internal copy of this guide |

Extract `SI2_reproducibility.zip` once and run commands from its extracted root. `FILE_INVENTORY.csv` lists the byte size and SHA-256 digest of every other file in the archive. `TABLE_ARCHIVE_MAP.csv` maps supplementary table identifiers to their locations. Original directory names and table identifiers are retained to match the manuscript and Supporting Information 1.

The code supplied here reproduces numerical analyses. Figure-rendering scripts are not included. Finished figures and the numerical data underlying them are retained.

## Primary results and definitions

The complete primary table is `Revision_Analysis_v5/results/primary_budget_summary.csv`; select `excluded = none` and `k = 2` for the primary two-member comparison. Budgets include the initial fifteen specimens. Random means are exact expectations conditional on the same fifteen-object starts. Fabric rules average twenty shared AMG orders. Archived random paths are retained for path-level comparisons.

Fields in `PRIMARY_RESULTS_k2.csv`:

| Field | Meaning |
|---|---|
| `case` | Paphos or Maya pool |
| `target` | Reference comparison; Maya `main` has four groups and `terminal` has twelve categories |
| `rule` | Stable algorithm key; `Proportional` denotes proportional-medoid selection |
| `mean_budget` | Mean total number of specimens, including the initial fifteen |
| `CI_low`, `CI_high` | Pointwise resampling interval |
| `random_mean` | Exact random expectation conditional on matched initial sets |
| `oracle_mean` | Membership-informed lower bound |
| `oracle_excess` | Mean budget minus that lower bound |
| `orders` | Number of fabric orders averaged |

Algorithm keys and object identifiers are preserved. They should not be inferred from abbreviated display labels alone.

## Contents and evidence map

| Location | Purpose |
|---|---|
| `Analysis/data` | Consolidated archive of the original 9,400 main and sensitivity acquisition configurations, with path metadata and per-path hashes |
| `Analysis/inputs`, `Analysis/selector`, `Analysis/source_measurements` | Starts, reference labels, Paphos measurements and transformed inputs, selection implementations and external-Maya adapter |
| `Revision_Analysis_v5` | Exact random references, paired differences, regret, cluster sensitivities and pXRF diagnostics |
| `Revision_Analysis_v4` | Oracle bounds and selected-set score diagnostics, including Figure S2 and Tables S49–S51 |
| `Figure_Presentation` | Finished Figures 1–3, Figure S4 continuation, and display-name mappings |
| `Revision_Diagnostics/results` | Category attainment, last-member bottlenecks and screening correspondence for Table S47 |
| `Submission_Tables` | Numerical table exporter and verified object/metadata crosswalks |
| `Supporting_results` | Group-coverage and path comparisons, response-panel results, member distributions, source overlap and object-level evidence |
| `Targeted_Analyses` | Fixed-budget evidence, target-set/member dependence, and Maya fixed-seed paths and contrasts |
| `Sensitivity_Analyses` | Batch-feedback and pXRF-only Paphos partition sensitivities |
| `Supporting_analyses` | Retained prediction experiment, Kourion experiments, full-data reconstruction checks and frozen-control specification |

Reconstruction checks that failed their stated criteria are included. The original Maya workbook and its concentration matrices are excluded under the source-access conditions below.

## Environment and numerical reproduction

Use Python 3.12 and the versions in `requirements.txt`. Work on a copy if supplied-file checksums must remain unchanged: analysis commands write calculated results to result folders.

The following commands use supplied paths, memberships, endpoints and shared bootstrap arrays. They do not require the external Maya workbook.

```text
python Analysis/evaluate_paths.py
python Revision_Analysis_v5/analyse_v5.py
python Revision_Analysis_v5/regret_sensitivities_v5.py
python Revision_Analysis_v4/analyse_revision_v4.py --oracle-only
python Submission_Tables/export_tables.py
python Targeted_Analyses/fixed_path_diagnostics.py
python Targeted_Analyses/summarize_fixed_seeds.py
python Supporting_analyses/paphos_prediction/reproduce_prediction.py --verify-archived
```

`Analysis/evaluate_paths.py` rescores all 9,400 original archived configurations and verifies primary and sensitivity attainment endpoints without fitting models. The primary Paphos selectors can also be rerun from the supplied inputs:

```text
python Analysis/reproduce_selectors.py --case Paphos --workers 1
```

Add `--stop 1001` for one archived start. The full range is 1001–1100.

### Source-dependent Maya reproduction

Obtain the provider workbook identified under Sources and rights, then run:

```text
python Analysis/prepare_maya_inputs.py --source-xlsx PATH_TO_mmc2.xlsx
python Analysis/reproduce_selectors.py --case Maya --workers 1
python Revision_Analysis_v5/diagnose_pxrf_v5.py --maya-input Analysis/selector/maya_inputs.npz
python Revision_Analysis_v4/analyse_revision_v4.py --geometry-input Analysis/selector/maya_inputs.npz
python Supporting_analyses/maya_reconstruction/reconstruction_gate.py
```

The adapter checks the source workbook digest. Keep `Analysis/selector/maya_inputs.npz` private and outside redistributed archives. Full Maya selection and feature reconstruction require this external input; archived-path evaluation and supplied summaries do not.

### Retained experiments

The earlier 20-test / 77-candidate Paphos prediction experiment in Tables S6–S7 is distinct from the 97-object complete-pool replay. Original paths, test/start designs, prediction metrics, evaluator code and a separate replay implementation are in `Supporting_analyses/paphos_prediction`; its README identifies shared inputs and commands.

For Kourion coverage, feedback and Paphos reconstruction:

```text
python Supporting_analyses/kourion_coverage/score_and_report.py
python Supporting_analyses/kourion_coverage/check_label_sensitivity.py
python Supporting_analyses/kourion_feedback/evaluate_feedback.py
python Supporting_analyses/paphos_reconstruction/run_judgment_gate.py
```

Selection scripts and inputs remain in the corresponding folders. Successful and unsuccessful reported outcomes are retained.

## Targeted supplementary analyses

`Targeted_Analyses` contains result-informed exploratory checks: fixed-budget evidence, fixed-path dependence and Maya fixed-seed contrasts. The consolidated original path archive contains 8,200 earlier configurations plus 1,200 Maya fixed-seed paths.

| Output in `Targeted_Analyses/results` | Interpretation |
|---|---|
| `fixed_budget_evidence_snapshot.csv` | Six Paphos targets at k=2 and total budgets 35, 55 and 75; completion probabilities/fractions, completed sets and pointwise intervals |
| `fixed_path_set_dependence_summary.csv` | Acquisition positions and waiting for all 20 Paphos set instances and all 16 Maya broad/fine categories under a relaxed set constraint |
| `fixed_path_member_dependence_summary.csv` | Required objects in the nine two-member Paphos sets; original candidates and paths retained |
| `maya_fixed_seed_budget_summary.csv` | Four MU/DWU arms, three seeds, two resolutions and k=2/3/5 |
| `maya_fixed_seed_contrasts.csv` | Paired updated-minus-frozen differences per seed and after averaging seeds within each start |
| `maya_fixed_seed_endpoints.csv` | 7,200 attainment endpoints from 1,200 paths |

Compressed configuration-level CSVs accompany the summaries. Random in snapshot tables is the exact conditional reference; Archived random denotes saved paths. Exact-random mean completed sets sums marginal hypergeometric completion probabilities. Fabric results cross 100 starts with 20 shared AMG orders. Additional waits are not additive effects and do not simulate unavailable specimens.

To regenerate Maya fixed-seed paths after preparing the private adapter input:

```text
python Targeted_Analyses/maya_fixed_seeds.py --matrix Analysis/selector/maya_inputs.npz --smoke --output local_fixed_seed_checkpoints
python Targeted_Analyses/maya_fixed_seeds.py --matrix Analysis/selector/maya_inputs.npz --output local_fixed_seed_checkpoints --workers 8
python Targeted_Analyses/summarize_fixed_seeds.py --checkpoints local_fixed_seed_checkpoints
```

The smoke check covers original scheduled-seed paths at starts 1001, 1050 and 1100. Fixed seeds are 202610041, 202610042 and 202610043, matched across budgets and arms. Default selector calls preserve original scheduling. Summaries average the three settings within each start before resampling 100 starts; the 300 start–seed combinations are not independent archaeological samples. Execution checkpoints are excluded from this release.

## Batch feedback and Paphos partition sensitivities

`Sensitivity_Analyses/PROTOCOL.json` records settings implemented before the sensitivity paths were generated. These are result-informed revision analyses, not preregistered primary analyses. Original measurements, memberships, 100 starts per case, model parameters and the original 9,400 configurations remain unchanged.

`Sensitivity_Analyses/results/S52_batch_feedback.csv` compares feedback after individual, five-specimen and ten-specimen batches. Forests and response scaling stay fixed within a batch; DWU density follows remaining candidates. NAA values are revealed when a batch returns. `selection_budget` is the first position supplying all required members; `returned_budget` is the committed count when those results are available. Random expectations integrate the original conditional CDF under the same return schedule. `delta_random` and `delta_initial` are paired within starts. Both MU/DWU and all six Paphos targets are scored; Maya retains both systems and k=2/3/5. The compact table reports k=2; full results and start-level endpoints are supplied.

`Sensitivity_Analyses/results/S53_pxrf_partition.csv` compares Paphos quota and fabric rules using 10, 20 and 21 Ward clusters from pXRF alone. Ward21 matches the number of released selection codes. Original reference-member IDs, all 97 objects, stratified starts and twenty shared AMG orders are retained. New cluster codes follow their smallest object ID. Global spread, forest and random paths remain comparators. This partition sensitivity does not resolve the historical timing of source metadata.

New full paths are in `Sensitivity_Analyses/results/new_acquisition_paths.json.gz`. `batch_endpoints.csv`, `batch_exact_random.csv` and `partition_endpoints.csv.gz` retain start-level calculations. Validation JSON records path completeness, paired scoring, batch1 path comparisons and first-batch hidden-response perturbations.

To regenerate these sensitivities after preparing the private Maya input:

```text
python Analysis/prepare_maya_inputs.py --source-xlsx PATH_TO_mmc2.xlsx
python Sensitivity_Analyses/run_sensitivities.py --phase all --workers 8
```

The script resumes completed jobs. The release supplies consolidated paths and results, rather than individual cached jobs.

## Figure and table sources

Finished Figures 1–3 are in `Figure_Presentation/figures` as TIFF and vector PDF/SVG. Their source tables remain in `Revision_Analysis_v5/results` and `Targeted_Analyses/results`. Figure 1 membership schematics use the supplied object crosswalks; positions represent membership, not chemical distance. All five SICILY and eight CYP-B objects are retained, including grey non-target objects marked †. Its heatmap reads all 60 k=2 completion values at budget 55 from `fixed_budget_evidence_snapshot.csv`; values at budgets 35 and 75 remain in that CSV. Original attainment curves and pointwise intervals are retained. Figure 2 presents Paphos paired differences; Figure 3 presents Maya budgets and contrasts. Display-name mappings retain the original algorithm IDs.

`TABLE_ARCHIVE_MAP.csv` covers original and additional Tables S52/S53. `Supporting_results/extended_supplement/Extended_results.docx` contains relocated prediction, Kourion, reconstruction, regret and path/geometry records and methods; a Chinese reading companion is beside it. Scientific tables, figure source data and analysis entry points retain their existing paths.

## Sources, access and rights

Paphos measurements: Hein, Dobosz and Kilikoglou (2021), Mendeley Data, version 1, https://doi.org/10.17632/ydgs3z4g76.1, CC BY 4.0. Reference assignments follow https://doi.org/10.1016/j.jas.2021.105436. Source metadata distinguish published assignments from later database labels.

Maya measurements and assignments: LeMoine and Halperin (2021), https://doi.org/10.1016/j.jasrep.2021.102819, supplement mmc2, https://ars.els-cdn.com/content/image/1-s2.0-S2352409X21000316-mmc2.xlsx. The original workbook and full raw/standardized concentration matrices are not redistributed. Obtain them under the provider's terms. The adapter, source identifiers, archived paths and derived summaries are supplied. No publisher article PDFs or full-text extracts are included.

Author-created data, derived results and documentation use Creative Commons Attribution 4.0 International (CC BY 4.0): https://creativecommons.org/licenses/by/4.0/. Author-created software uses the MIT License. `LICENSES.md` inside SI2 gives the full terms and scopes. These grants cover only rights held by the package contributors; third-party materials retain their original licences and provider terms.
