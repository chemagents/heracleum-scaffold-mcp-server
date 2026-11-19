# heracleum-scaffold-mcp-server

An MCP server for reproducing and auditing the analyses in:

> Rassabina, A. E.; Safronov, V. S.; Fedorov, M. V.
> *From Chemical Scaffold to Predicted Toxicity: A Scaffold-Aware Pipeline for Reliable In Silico Prioritization of Heracleum Furanocoumarins.*
> **International Journal of Molecular Sciences 2026, 27, 7947.**
> https://doi.org/10.3390/ijms27177947

The server connects chemical scaffolds, docking profiles and predicted toxicity, and tests whether models generalize to unfamiliar scaffolds. It uses the published **2,008 reference compounds and 102 natural compounds** (2,073 distinct source IDs), stored scores for **44 targets**, and ADMETlab predictions. The default analyses run on CPU; they do not repeat docking or query ADMETlab.

Scientific code and source data are shared through [chem-repro-suite](https://github.com/chemagents/chem-repro-suite/tree/4be1e94c912db7a2790c831fb01d098ad290a88a), pinned to version 0.4.4, commit `4be1e94c912db7a2790c831fb01d098ad290a88a`. This repository supplies the MCP entry point, deployment configuration and checks. Tools operate on the fixed article dataset; they do not accept arbitrary uploaded datasets.

## Tools

| Tool | What it returns |
|---|---|
| `heracleum2026_dataset_audit` | Cohort sizes, overlaps and conflicting molecular identities |
| `heracleum2026_chemical_space` | Scaffold structures, frequencies and coverage |
| `heracleum2026_docking_profile` | PCA, target loadings and information lost by averaging scores |
| `heracleum2026_docking_protocol_validation` | Archived redocking statistics; optional public reconstruction |
| `heracleum2026_toxicity_landscape` | Endpoint distributions, correlations, PCA and saturation |
| `heracleum2026_scaffold_separability` | Permutation tests across 13 axes with BH FDR correction |
| `heracleum2026_scaffold_transfer` | Random Forest and ElasticNet under random and scaffold five-fold validation |
| `heracleum2026_prioritize_candidates` | Per-compound criteria, tiers and comparison with Table S4 |

Exact parameters and output schemas are in [api/tools.json](api/tools.json). Each result includes metrics, tables, limitations and provenance. Downloadable artifacts have a URL, byte count and SHA-256; full CSV files accompany table previews. `success` means the computation completed, not that every paper claim matched.

For example, ask CoScientist:

> Do the Heracleum models generalize to unfamiliar scaffolds? Compare Random Forest and ElasticNet for neurotoxicity and eye corrosion under random and scaffold five-fold validation. Use the published snapshot, report identity conflicts separately, and return pooled OOF R², RMSE, MAE, bootstrap intervals and a scaffold-overlap audit.

See [REPRODUCTION_QUESTIONS.md](REPRODUCTION_QUESTIONS.md) for further questions and the tools expected to answer them.

## Reproduction fidelity

| Result | Paper | Recomputed result |
|---|---|---|
| Docking PC1 variance / correlation with mean score | 61.2% / 0.927 | 61.166% / 0.926930 |
| Toxicity PC1 variance / components for 80% | 29.6% / 6 | 29.627% / 6 |
| RF neurotoxicity R², random → scaffold | 0.645 → 0.444 | 0.644662 → 0.444450 |
| RF eye corrosion R², random → scaffold | 0.670 → 0.308 | 0.670067 → 0.307590 |
| Natural-compound tiers 1 / 2 / 3 | 21 / 24 / 57 | 21 / 24 / 57 in published-snapshot mode |

`strict` is the default identity policy. Numerical replay of the published cohort requires explicit `identity_policy="published_snapshot"`; 65 source/master identity conflicts remain reported. Strict prioritization returns 18 / 10 / 9 compounds and leaves 65 unranked. Pabularinone meets 7 of 8 endpoint criteria, rather than the claimed 8 of 8.

Transfer validation retrains models using a frozen fold registry from the reference implementation, not author-supplied fold files. Its targets are **ADMETlab outputs, not measured toxicity**. Redocking counts (24/42 top-1 and 36/42 top-5) are recalculated from archived tables; the reported active/inactive control result is not independently recomputed from raw controls. Agreement between structure-derived analyses is chemical coherence, not biological validation.

## Run locally

Requires Git, Python 3.12 and uv:

```bash
git clone https://github.com/chemagents/heracleum-scaffold-mcp-server.git
cd heracleum-scaffold-mcp-server
uv sync --frozen --python 3.12
uv run heracleum-scaffold-mcp-server
```

The default port is 7342. This starts a local development instance; no LLM API key is required by the MCP server.

## Attach to CoScientist

Deploy once on a host reachable by CoScientist and register its Streamable HTTP `/mcp` endpoint. The same service exposes result files at `/files/…`. End users then ask questions through CoScientist.

**No shared hosted endpoint is currently published.** Set `MCP_BASE_URL` to the actual deployment address, without `/mcp`, and generate the configuration:

```bash
uv run python scripts/configure_endpoint.py --base-url "$MCP_BASE_URL"
```

[Deployment, Docker and registration instructions](COSCIENTIST_INTEGRATION.md) cover startup, artifact URLs, optional workers and verification from the client network. [server.json](server.json) records the integration contract and deployment status.

## Tests

```bash
python -m unittest discover -s tests -v
uv run python scripts/smoke_server.py --study heracleum
```

[CI](https://github.com/chemagents/heracleum-scaffold-mcp-server/actions/workflows/smoke.yml) checks configuration, startup, tool discovery and artifact downloads in Python and Docker. These checks do not establish full scientific reproduction or acceptance of every CoScientist answer. The English questions define the system evaluation. Current end-to-end runs use local CoScientist fixes for MCP result capture, source access and reporting; they do not certify the unmodified upstream version. Scientific findings and source limitations are reviewed separately from successful execution.

## License / data

Code: [MIT](LICENSE). Source provenance and dataset licenses are listed in the [data manifest](https://github.com/chemagents/chem-repro-suite/blob/4be1e94c912db7a2790c831fb01d098ad290a88a/chem_repro/data/heracleum/manifest.json) and [NOTICE](https://github.com/chemagents/chem-repro-suite/blob/4be1e94c912db7a2790c831fb01d098ad290a88a/NOTICE.md). Please cite the article when using its data or results.
