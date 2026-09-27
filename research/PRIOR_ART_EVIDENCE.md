# Prior-Art Evidence — Milestone 1

Status: evidence-backed first pass. Proposed Staleness Propagation Cost (SPC) and selective-revalidation novelty remain **UNVERIFIED**.

## Established work that constrains novelty

### MuCache — microservice-graph caching and coherence
Zhang et al., **"MuCache: A General Framework for Caching in Microservice Graphs"**, NSDI 2024, introduces inter-service caching for arbitrary microservice applications and a non-blocking cache coherence/invalidation protocol for graph topologies. It evaluates latency and throughput on microservice benchmarks.

Verified source:
- USENIX NSDI 2024: https://www.usenix.org/conference/nsdi24/presentation/zhang-haoran

### Skybridge — bounded staleness for distributed caches
Lyerly et al., **"Skybridge: Bounded Staleness for Distributed Caches"**, OSDI 2025, addresses bounded visibility delay in asynchronously replicated distributed caches and evaluates a lightweight replication stream for bounding staleness.

Verified source:
- USENIX OSDI 2025: https://www.usenix.org/conference/osdi25/presentation/lyerly

### CausalMesh — causal caching
Zhang et al., **"CausalMesh: A Formally Verified Causal Cache for Stateful Serverless Computing"** (2025 preprint) studies causally consistent caching when computation moves among servers and formally verifies its protocol.

Verified source:
- arXiv: https://arxiv.org/abs/2508.15647

## Overlap classification

| Work | Overlap | Why |
|---|---|---|
| MuCache, NSDI 2024 | SUBSTANTIAL | Directly studies caching, coherence/invalidation, and graph-structured microservice applications. |
| Skybridge, OSDI 2025 | SUBSTANTIAL | Directly studies and quantitatively bounds staleness in distributed caches. |
| CausalMesh, 2025 | PARTIAL | Directly studies causal cache semantics and dependency-related consistency, but in stateful serverless/client-roaming settings. |

## Claims we must NOT make

- Caching in microservice graphs is new.
- Cache coherence/invalidation for microservice graphs is new.
- Bounded staleness in distributed caches is new.
- Causal metadata or causal consistency for caches is new.
- Measuring stale-read frequency or visibility delay alone is a new contribution.

## Candidate defensible gap — UNVERIFIED

The candidate contribution is narrower: quantify the **application-level downstream cost caused by stale state as it propagates through a multi-tier microservice dependency DAG**, define a reproducible cost model/metric only if prior art permits it, and use request-visible freshness/dependency information to selectively revalidate paths where expected stale-state cost justifies validation.

The project must experimentally separate this from MuCache-style coherence, Skybridge-style bounded replication staleness, causal-consistency protocols, TTL, asynchronous invalidation, and synchronous validation.

SPC is only a working name. No originality claim is made for the term or concept until synonym/mechanism/citation-chain searches are complete.

## Next search layer

1. stale data propagation cost microservices
2. cache dependency graph stale reads downstream effects
3. freshness debt / staleness debt / inconsistency cost
4. dependency-aware cache validation / selective revalidation
5. causal metadata cache freshness validation
6. citation chains from MuCache, Skybridge, and CausalMesh
