# Prior-Art Evidence — Milestone 1

Review date: 2026-09-28.

Status: deeper evidence-backed audit. Proposed stale-state downstream-cost metric and selective revalidation remain **UNVERIFIED**.

## Established work that constrains novelty

### MuCache — microservice-graph caching and coherence

Zhang et al., **"MuCache: A General Framework for Caching in Microservice Graphs"**, NSDI 2024, introduces inter-service caching for arbitrary microservice applications and a non-blocking coherence/invalidation protocol for graph topologies.

Verified source:
- https://www.usenix.org/conference/nsdi24/presentation/zhang-haoran

### Skybridge — bounded staleness for distributed caches

Lyerly et al., **"Skybridge: Bounded Staleness for Distributed Caches"**, OSDI 2025, provides an out-of-band replication stream for bounding cache staleness.

Verified source:
- https://www.usenix.org/conference/osdi25/presentation/lyerly

### T-Cache — dependency-aware inconsistency detection and cost trade-offs

Eyal, Birman, and van Renesse, **"Cache Serializability: Reducing Inconsistency in Edge Transactions"**, ICDCS 2015, proposes T-Cache for read-only transactions where inconsistency is tolerable but has a cost. It uses dependency information to detect inconsistent cached transactions and exposes a performance/consistency trade-off.

Verified sources:
- DOI: https://doi.org/10.1109/ICDCS.2015.75
- Preprint: https://arxiv.org/abs/1409.8324

### CausalMesh — causal caching

Zhang et al., **"CausalMesh: A Formally Verified Causal Cache for Stateful Serverless Computing"** (2025 preprint) studies causally consistent caching and formally verifies its protocol.

Verified source:
- https://arxiv.org/abs/2508.15647

## Overlap classification

| Work | Overlap | Why |
|---|---|---|
| MuCache, NSDI 2024 | SUBSTANTIAL | Caching, coherence/invalidation, and graph-structured microservices. |
| Skybridge, OSDI 2025 | SUBSTANTIAL | Quantitatively bounded staleness in distributed caches. |
| T-Cache, ICDCS 2015 | SUBSTANTIAL | Dependency metadata, inconsistency cost, and selective detection in caches. |
| CausalMesh, 2025 | PARTIAL | Causal cache semantics and dependency-related consistency in a different setting. |

## Claims this project must not make

- Caching or coherence in microservice graphs is new.
- Bounded cache staleness is new.
- Causal/dependency metadata for caches is new.
- Treating inconsistency as costly or using dependencies to detect it is new.
- Measuring stale-read frequency or visibility delay alone is a contribution.

## Candidate defensible gap — UNVERIFIED

The remaining candidate is specifically **application-level downstream work caused by stale state propagating through a multi-tier microservice DAG**, together with cost-driven selective revalidation based on expected downstream harm. Experiments must distinguish this from MuCache coherence, Skybridge bounded replication delay, T-Cache transaction inconsistency detection, causal-consistency protocols, TTL, invalidation, and synchronous validation.

The working metric name "Staleness Propagation Cost" is not an originality claim. Differentiation must search equivalent terms such as inconsistency cost, wasted downstream work, retry/compensation amplification, dependency-aware validation, and freshness-aware execution.
