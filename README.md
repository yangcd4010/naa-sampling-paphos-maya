# Data and code: NAA sampling in Paphos and Classic Maya ceramics

This dataset supports the retrospective evaluation of NAA acquisition in a fixed pool of 97 Paphos amphora fragments and 183 Classic Maya ceramics.

## Start here

`PRIMARY_RESULTS_k2.csv` is the directly readable table of primary two-member results. `SI2_reproducibility.zip` contains the necessary source inputs, reference definitions, acquisition paths, numerical results and executable analyses. Unzip it once. Its `FILE_INVENTORY.csv` lists the payload and SHA-256 digests.

Primary results are in `Revision_Analysis_v5/results/primary_budget_summary.csv`; use `excluded = none` and `k = 2`. Random means are exact expectations conditional on the same fifteen-object starts. Fabric rules average twenty shared orders. Archived random paths are retained for path-level comparisons. Directory labels referenced by Supporting Information 1 remain stable identifiers.

Primary CSV fields: `case` identifies the pool; `target` identifies the reference comparison; `rule` is the algorithm key; `mean_budget` includes the initial fifteen specimens; `CI_low` and `CI_high` are pointwise resampling intervals; `random_mean` is the matched-start exact random expectation; `oracle_mean` is the membership-informed lower bound; `oracle_excess` is mean budget minus that bound; `orders` is the number of fabric orders averaged. `Proportional` is the proportional-medoid implementation. Maya `main` denotes four groups and `terminal` twelve categories.

## Contents and evidence map

| Location | Purpose |
|---|---|
| `Analysis/data` | One consolidated archive of all 8,200 main and sensitivity acquisition configurations, with path metadata and per-path hashes |
| `Analysis/inputs`, `Analysis/selector`, `Analysis/source_measurements` | Starts, reference labels, Paphos source measurements and transformed inputs, selection implementations, external-Maya adapter |
| `Revision_Analysis_v5` | Exact random references, paired differences, regret, cluster sensitivities and pXRF diagnostic summaries; main Figures 2–3 and Figures S3–S4 |
| `Revision_Analysis_v4` | Oracle bounds and selected-set score diagnostics; Figure S2 and S49–S51 |
| `Revision_Presentation_v6` | Current Figure 1 code, reading the shared current results |
| `Revision_Diagnostics/results` | Category attainment, last-member bottlenecks and screening correspondence for S47 |
| `Submission_Tables` | Table exporter and verified object/metadata crosswalks |
| `Supporting_results` | Original group-coverage and path comparisons, response-panel results, member distributions, source overlap and object-level evidence supporting earlier SI tables |
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
python Revision_Analysis_v4/plot_geometry_v4.py
```

The first command reads the single path archive and verifies primary and sensitivity attainment endpoints. The following commands use supplied memberships, endpoints and shared bootstrap arrays. No provider workbook is needed for these commands.

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

## Sources and rights

Paphos measurement workbooks: Hein, Dobosz and Kilikoglou (2021), Mendeley Data, version 1, https://doi.org/10.17632/ydgs3z4g76.1, CC BY 4.0. Reference assignments follow https://doi.org/10.1016/j.jas.2021.105436. Source metadata distinguish published assignments and later database labels.

Maya: LeMoine and Halperin (2021), https://doi.org/10.1016/j.jasrep.2021.102819, supplement mmc2. The original workbook and full raw/standardized concentration matrices are not redistributed. Obtain them under the provider's terms; the adapter and source identifiers are supplied. No publisher article PDFs or full-text extracts are included.

`LICENSES.md` preserves the source-attribution and rights boundaries. It does not assign a reuse licence to author-created code or derived data. Third-party licences retain their own scope.
