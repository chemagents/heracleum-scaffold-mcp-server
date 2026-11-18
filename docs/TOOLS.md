# Каталог инструментов

Каталог получен через MCP `tools/list` из кода, побайтно сверенного с закреплённой ревизией backend. Полные ограничения параметров и схема результата находятся в [api/tools.json](../api/tools.json).

Область каждого вызова — закреплённый набор статьи. Аргументы не являются произвольной загрузкой CSV. Числовые значения по умолчанию нужно интерпретировать вместе с выбранным протоколом.

## `heracleum2026_dataset_audit`

Study: Heracleum2026; paper DOI: 10.3390/ijms27177947; dataset: heracleum_2026.
What are the DS1/DS2 sizes, overlap, archived docking coverage and source identity conflicts?
Compare master versus source canonical SMILES by synID and export problematic rows separately.
Unusable docking profiles mean all-zero archived scores, not geometry/valence errors; all molecular
graphs in this audit parse. No new docking, conformer validation or SMILES-versus-InChI test is run.

| Аргумент | Обязательный | По умолчанию | Допустимые значения |
|---|---|---|---|
| Нет параметров | — | — | — |

[Как читать результат](INTEGRATION.md#результат-инструмента).

## `heracleum2026_chemical_space`

Study: Heracleum2026; paper DOI: 10.3390/ijms27177947; dataset: heracleum_2026.
How concentrated is Heracleum furanocoumarin chemistry around psoralen and angelicin?
Recompute both source datasets' Murcko/generic counts, top-2/5/15 coverage and 50% coverage.

| Аргумент | Обязательный | По умолчанию | Допустимые значения |
|---|---|---|---|
| `include_figures` | Нет | `true` | См. JSON Schema |

[Как читать результат](INTEGRATION.md#результат-инструмента).

## `heracleum2026_docking_profile`

Study: Heracleum2026; paper DOI: 10.3390/ijms27177947; dataset: heracleum_2026.
Does averaging 44 docking scores hide target-profile information? Recompute PCA, loadings,
explained variance, PC1–mean correlation and strong-score counts on the deposited 2055 profiles.

| Аргумент | Обязательный | По умолчанию | Допустимые значения |
|---|---|---|---|
| `include_figures` | Нет | `true` | См. JSON Schema |

[Как читать результат](INTEGRATION.md#результат-инструмента).

## `heracleum2026_docking_protocol_validation`

Study: Heracleum2026; paper DOI: 10.3390/ijms27177947; dataset: heracleum_2026.
How reliable is the Heracleum docking protocol? Recompute top-1 through top-5 recovery
from the 44 published Table S1a target observations, with 42 evaluated targets and two exclusions.
Default published_table aggregates source observations without rerunning docking.
public_reconstruction runs the declared public-crystal Vina protocol or verifies an existing
completed run, recalculates symmetry-aware heavy-atom RMSD without superposition, and exports
structures and preparation failures. Original prepared author receptors and raw active/inactive
controls remain unavailable; reconstructed outcomes are never labeled exact author-input replay.

| Аргумент | Обязательный | По умолчанию | Допустимые значения |
|---|---|---|---|
| `mode` | Нет | `"published_table"` | ["published_table", "public_reconstruction"] |
| `include_structures` | Нет | `true` | См. JSON Schema |

[Как читать результат](INTEGRATION.md#результат-инструмента).

## `heracleum2026_toxicity_landscape`

Study: Heracleum2026; paper DOI: 10.3390/ijms27177947; dataset: heracleum_2026.
Can ten predicted Heracleum toxicities be reduced to one index? Recompute distributions,
saturation, Spearman correlations and standardized PCA of the deposited ADMETlab values.

| Аргумент | Обязательный | По умолчанию | Допустимые значения |
|---|---|---|---|
| `include_figures` | Нет | `true` | См. JSON Schema |

[Как читать результат](INTEGRATION.md#результат-инструмента).

## `heracleum2026_scaffold_separability`

Study: Heracleum2026; paper DOI: 10.3390/ijms27177947; dataset: heracleum_2026.
Are five scaffold classes separated beyond chance across ten toxicity and three docking-PC axes?
Run mean-pairwise KS permutations and BH FDR. 10000 is the original protocol; Other is excluded.

| Аргумент | Обязательный | По умолчанию | Допустимые значения |
|---|---|---|---|
| `permutations` | Нет | `10000` | См. JSON Schema |
| `include_figures` | Нет | `true` | См. JSON Schema |

[Как читать результат](INTEGRATION.md#результат-инструмента).

## `heracleum2026_scaffold_transfer`

Study: Heracleum2026; paper DOI: 10.3390/ijms27177947; dataset: heracleum_2026.
Does a Morgan-fingerprint surrogate transfer to unseen generic scaffolds? Refit RF/ElasticNet
with random and GroupKFold CV, OOF metrics and CI. Default both/both completes ALL FOUR model ×
endpoint combinations and both split schemes in one scientific call; individual selections remain
supported. No separate calls are needed for the full comparison. Published snapshot reproduces notebook inputs;
strict excludes identity conflicts. Model targets are ADMETlab predictions, not experimental labels.
eye_corrosion is distinct from eye_irritation. Compare absolute scaffold performance together with
the random-minus-scaffold R² drop; a smaller drop alone does not establish a more robust model.

| Аргумент | Обязательный | По умолчанию | Допустимые значения |
|---|---|---|---|
| `endpoint` | Нет | `"both"` | ["both", "neurotoxicity", "eye_corrosion"] |
| `model` | Нет | `"both"` | ["both", "RF", "ElasticNet"] |
| `identity_policy` | Нет | `"strict"` | ["strict", "published_snapshot"] |
| `bootstrap` | Нет | `1000` | См. JSON Schema |
| `include_oof` | Нет | `true` | См. JSON Schema |
| `include_figures` | Нет | `true` | См. JSON Schema |

[Как читать результат](INTEGRATION.md#результат-инструмента).

## `heracleum2026_prioritize_candidates`

Study: Heracleum2026; paper DOI: 10.3390/ijms27177947; dataset: heracleum_2026.
Which natural Heracleum compounds merit follow-up and why? Apply the original eight-endpoint,
docking, PAINS and Brenk rules. Strict leaves identity conflicts unranked; published_snapshot reproduces
the deposited tier counts despite conflicts. All 102 records remain the median reference in both modes.
Compare Tier 1 with published Table S4 separately by original DS2 and master prediction structures;
report named-priority claim disagreements rather than repeating them as computed findings.

| Аргумент | Обязательный | По умолчанию | Допустимые значения |
|---|---|---|---|
| `identity_policy` | Нет | `"strict"` | ["strict", "published_snapshot"] |
| `strong_threshold` | Нет | `-10` | См. JSON Schema |
| `include_figures` | Нет | `true` | См. JSON Schema |

[Как читать результат](INTEGRATION.md#результат-инструмента).
