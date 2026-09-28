# Prior-Art Search Log

| Date | Database/Search Engine | Query | Filters | Number/Type of Results | Relevant Results |
|---|---|---|---|---|---|
| 2026-09-26 | USENIX / OSDI | bounded staleness distributed caches | peer-reviewed systems work | OSDI paper | Skybridge (OSDI 2025) directly studies bounded staleness for distributed caches. |
| 2026-09-27 | USENIX NSDI | microservice graph caching coherence invalidation | NSDI / peer-reviewed | conference paper | MuCache (NSDI 2024) provides caching plus non-blocking coherence/invalidation for microservice graph topologies. |
| 2026-09-27 | USENIX OSDI | bounded staleness distributed caches Skybridge | OSDI / peer-reviewed | conference paper | Skybridge provides an out-of-band stream for bounded cache staleness. |
| 2026-09-27 | arXiv | causal cache serverless dependency consistency CausalMesh | research preprint | preprint | CausalMesh (2025) studies and formally verifies causally consistent caching in stateful serverless computing. |
| 2026-09-28 | IEEE DOI / arXiv | T-Cache inconsistency cost dependency cache serializability | ICDCS + preprint | conference paper | T-Cache (ICDCS 2015, DOI 10.1109/ICDCS.2015.75) treats inconsistency as costly and uses dependency information to detect inconsistent cached transactions. |
| 2026-09-28 | Mechanism-gap review | stale state downstream work microservice DAG inconsistency cost selective revalidation | exact concept + synonyms | research and engineering leads | Broad dependency-aware or cost-aware cache claims overlap T-Cache; the remaining candidate must measure application-level downstream work caused by propagation. |
| 2026-09-28 | Differentiation review | cache retry compensation wasted work stale propagation dependency graph | mechanism-focused | citation/search leads | Future review must test whether retry, compensation, wasted-compute, and freshness-debt literature already captures the proposed metric under another name. |

## Evidence discipline

- Peer-reviewed systems papers define the closest verified overlap.
- Preprints are labeled separately and do not establish peer-reviewed priority.
- The proposed downstream-cost metric and selective-revalidation policy remain **UNVERIFIED** pending synonym, mechanism, and citation-chain searches.
