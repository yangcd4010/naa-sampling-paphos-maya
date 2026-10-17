# NAA sampling in Paphos and Classic Maya ceramics: data and numerical analyses

Version: v1.0.7

Data and numerical analysis code supporting *Retrospective evaluation of neutron activation analysis sampling for archaeological comparisons in Paphos and Classic Maya ceramics* (manuscript).

The study evaluates acquisition budgets for repeated members of archaeological comparison groups in 97 Paphos amphora fragments and 183 object-matched pXRF and NAA records from ten Classic Maya sites.

Repository: https://github.com/yangcd4010/naa-sampling-paphos-maya/releases/tag/v1.0.7

Zenodo: https://doi.org/10.5281/zenodo.23250701. Select v1.0.7 and use its version-specific DOI when citing this release.

## Files and first steps

| File | Contents |
|---|---|
| `PRIMARY_RESULTS_k2.csv` | Primary two-member attainment budgets, intervals, exact random expectations and oracle bounds |
| `README.md` | File guide and execution instructions |
| `SI2_reproducibility.zip` | Data, acquisition paths, numerical code, result tables, extended report and finished figures |

Extract the archive into its own directory and run commands from that directory:

```bash
unzip SI2_reproducibility.zip -d SI2_reproducibility
cd SI2_reproducibility
```

`FILE_INVENTORY.csv` lists file sizes and SHA-256 checksums. `TABLE_ARCHIVE_MAP.csv` maps supplementary table identifiers to their locations. `LICENSES.md` gives the licence terms and scopes.

| Directory | Contents |
|---|---|
| `Analysis/` | Selection implementations, data adapters, supplied measurements, starts and 9,400 acquisition configurations |
| `Attainment_Analysis/` | Primary budgets, exact random expectations, paired contrasts, regret and screening diagnostics |
| `Oracle_Geometry/` | Oracle bounds and predictor-distance diagnostics |
| `Category_Diagnostics/` | Maya category attainment and screening correspondence |
| `Targeted_Analyses/` | Fixed-budget, fixed-path dependence and fixed-seed results |
| `Sensitivity_Analyses/` | Batch-feedback and pXRF partition analyses (Tables S52–S53) |
| `Submission_Tables/` | Table exporter, object crosswalks and resampling inputs |
| `Supporting_results/` | Detailed results, figure source data and the English extended report |
| `Supporting_analyses/` | Code and inputs for the auxiliary analyses reported in the extended report |
| `Figure_Presentation/` | Finished Figures 1–3, the Figure S4 continuation and display-name mappings |

## Primary results and definitions

The complete primary table is `Attainment_Analysis/results/primary_budget_summary.csv`; select `excluded = none` and `k = 2` for the primary comparison. Budgets include the initial fifteen specimens. Random means are exact expectations conditional on the same starts. Fabric-rule means average twenty shared AMG orders.

| Field in `PRIMARY_RESULTS_k2.csv` | Meaning |
|---|---|
| `case`, `target` | Dataset and archaeological comparison |
| `rule` | Algorithm key, such as `Max-min`, `Proportional`, `Updated DWU` or `APDC` |
| `mean_budget` | Mean total specimen budget |
| `CI_low`, `CI_high` | Pointwise 95% resampling interval |
| `random_mean` | Exact conditional random expectation |
| `oracle_mean` | Membership-informed lower bound |
| `oracle_excess` | Mean budget minus the oracle bound |
| `orders` | Number of fabric orders averaged |

Algorithm identifiers are retained in the data. `Figure_Presentation/display_mapping.json` relates them to figure labels.

## Environment and numerical reproduction

Use Python 3.12 and the versions in `requirements.txt`:

```bash
pip install -r requirements.txt
```

Run analyses on a working copy because commands write results. These commands use supplied inputs and do not require the external Maya workbook:

```bash
python Analysis/evaluate_paths.py
python Attainment_Analysis/analyse_attainment.py
python Attainment_Analysis/regret_sensitivities.py
python Oracle_Geometry/analyse_oracle_geometry.py --oracle-only
python Submission_Tables/export_tables.py
python Targeted_Analyses/fixed_path_diagnostics.py
python Targeted_Analyses/summarize_fixed_seeds.py
```

`evaluate_paths.py` rescores the 9,400 archived configurations. To rerun Paphos selectors from supplied measurements:

```bash
python Analysis/reproduce_selectors.py --case Paphos --workers 1
```

Add `--stop 1001` to run one start; the complete range is 1001–1100.

### Source-dependent Maya reproduction

Obtain the provider workbook linked under Sources and rights, then run:

```bash
python Analysis/prepare_maya_inputs.py --source-xlsx PATH_TO_mmc2.xlsx
python Analysis/reproduce_selectors.py --case Maya --workers 1
python Attainment_Analysis/diagnose_pxrf.py --maya-input Analysis/selector/maya_inputs.npz
python Oracle_Geometry/analyse_oracle_geometry.py --geometry-input Analysis/selector/maya_inputs.npz
```

The adapter checks the source digest. Maya selection and feature diagnostics require the workbook and adapter-generated input; archived-path evaluation uses supplied inputs. The generated concentration matrix remains subject to the provider's terms.

After preparing that input, sensitivity paths can be regenerated with:

```bash
python Sensitivity_Analyses/run_sensitivities.py --phase all --workers 8
python Targeted_Analyses/maya_fixed_seeds.py --matrix Analysis/selector/maya_inputs.npz --output local_fixed_seed_checkpoints --workers 8
python Targeted_Analyses/summarize_fixed_seeds.py --checkpoints local_fixed_seed_checkpoints
```

Fixed-seed summaries average seeds within each start before resampling starts. Protocol files specify the result-informed supplementary analyses and their numerical settings.

### Auxiliary analyses

Commands for the analyses reported in the extended report:

```bash
python Supporting_analyses/paphos_prediction/reproduce_prediction.py --verify-archived
python Supporting_analyses/kourion_coverage/score_and_report.py
python Supporting_analyses/kourion_coverage/check_label_sensitivity.py
python Supporting_analyses/kourion_feedback/evaluate_feedback.py
python Supporting_analyses/paphos_reconstruction/run_judgment_gate.py
python Supporting_analyses/maya_reconstruction/reconstruction_gate.py
```

The Maya reconstruction command also requires the adapter input. The prediction subdirectory has its own input and execution guide.

## Figure and table sources

Finished Figures 1–3 are supplied as TIFF and vector PDF in `Figure_Presentation/figures`; the Figure S4 continuation is supplied as PDF. Numerical source tables are in `Attainment_Analysis/results`, `Targeted_Analyses/results` and `Supporting_results/figure_source_data.csv`. The budget-55 completion heatmap uses `Targeted_Analyses/results/fixed_budget_evidence_snapshot.csv`, which also contains budgets 35 and 75.

The English extended report is `Supporting_results/extended_supplement/Extended_results.pdf`. Table identifiers and locations are listed in `TABLE_ARCHIVE_MAP.csv`.

## Sources and rights

Paphos measurements: Hein, Dobosz and Kilikoglou (2021), Mendeley Data, version 1, https://doi.org/10.17632/ydgs3z4g76.1, CC BY 4.0. Reference assignments follow https://doi.org/10.1016/j.jas.2021.105436.

Maya measurements and assignments: LeMoine and Halperin (2021), https://doi.org/10.1016/j.jasrep.2021.102819, supplement mmc2: https://ars.els-cdn.com/content/image/1-s2.0-S2352409X21000316-mmc2.xlsx. Obtain the source workbook under the provider's terms; it and its full concentration matrices are not redistributed here.

Author-created data, results and documentation use CC BY 4.0; author-created software uses MIT. Third-party materials retain their original licences and provider terms. See `LICENSES.md`.

## Citation

Yang, C., Zhou, X., and Zuo, Q. (2026). *NAA sampling in Paphos and Classic Maya ceramics: data and numerical analyses*, v1.0.7. Zenodo. Use the version-specific DOI displayed on the v1.0.7 record.
