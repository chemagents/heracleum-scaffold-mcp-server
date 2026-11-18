# Reproduction questions

Ask these questions through CoScientist after registering the server. Return computed values, cohort definitions, methods, limitations and all generated figures/tables with download URLs and SHA-256 hashes. Download full tables rather than relying on previews.

The tool names below describe expected execution; they are not evidence that a run succeeded. [cases/cases.json](cases/cases.json) contains separate scientific checks; [cases/prompts.jsonl](cases/prompts.jsonl) contains prompts without reference answers. Keep expected numerical values out of blind test prompts.

**H01**

> Which datasets make up the Heracleum study (IJMS 2026, 27, 7947)? Recompute sizes, overlaps and docking coverage, audit molecular identity across source files, and list conflicting records separately.

Tools: `heracleum2026_dataset_audit`.

**H02**

> How concentrated is Heracleum chemical space around a few scaffolds? Recompute scaffold coverage separately for the reference and natural sets, distinguishing scaffold counts from molecule counts.

Tools: `heracleum2026_chemical_space`.

**H03**

> Is the mean docking score sufficient to describe the Heracleum off-target panel? Recompute PCA, explained variance, target loadings and correlation with the mean score, and explain the information lost by averaging.

Tools: `heracleum2026_docking_profile`.

**H04**

> How reliable is the Heracleum docking protocol? Check native-pose recovery and active/inactive discrimination. Distinguish archived-table calculations, paper-reported values and fresh runs; do not count excluded targets as failures.

Tools: `heracleum2026_docking_protocol_validation`.

**H05**

> Can the ten predicted Heracleum toxicity endpoints be reduced to one index? Recompute distributions, correlations, PCA and scale saturation, and explain which endpoints are unsuitable for ranking.

Tools: `heracleum2026_toxicity_landscape`.

**H06**

> Does the predicted Heracleum profile depend on scaffold class? Reproduce the permutation analysis across all toxicity and docking axes with multiple-testing correction. Return effect sizes and the random expectation.

Tools: `heracleum2026_scaffold_separability`.

**H07**

> Do Heracleum models generalize to unfamiliar scaffolds? Reproduce Random Forest and ElasticNet for neurotoxicity and eye corrosion under random and scaffold five-fold validation. Use published-snapshot records and saved scaffold groups. Return pooled OOF R², RMSE, MAE, 95% R² intervals from 1,000 bootstrap repetitions, R² drops and a scaffold-overlap audit. Report source problems separately.

Tools: `heracleum2026_scaffold_transfer`.

**H08**

> Which natural Heracleum furanocoumarins should be prioritized for experiments? Reproduce published-snapshot tiers, then separately produce the identity-verified list. Return criteria for every compound and differences between the lists.

Tools: `heracleum2026_dataset_audit`, `heracleum2026_prioritize_candidates`.

**H09**

> Does agreement between Heracleum scaffolds, docking and ADMET profiles validate biological toxicity? Trace the provenance and dependencies of all three evidence types and identify the external experiments needed for independent validation.

Tools: `heracleum2026_dataset_audit`, `heracleum2026_docking_profile`, `heracleum2026_toxicity_landscape`.

Record adapter/core revisions, parameters and input hashes with each run. Compare computed results with the article independently of whether the final narrative sounds plausible. See the [README](README.md#reproduction-fidelity) for known discrepancies.
