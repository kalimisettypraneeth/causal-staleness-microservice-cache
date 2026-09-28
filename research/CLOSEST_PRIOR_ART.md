# Closest Prior Art — Causal Staleness in Microservice Caches

Review date: 2026-09-28  
Status: **EVIDENCE TABLE IN PROGRESS — NOVELTY UNVERIFIED**

Overlap taxonomy: `DIRECT`, `SUBSTANTIAL`, `PARTIAL`, `ADJACENT`, `FOUNDATIONAL`, `NONE IDENTIFIED`.

| Work | Year / venue | Research question or mechanism | Workload / setting | Metrics or decision signals | Artifact / primary source | Overlap | Exact differentiation still requiring proof |
|---|---|---|---|---|---|---|---|
| MuCache: A General Framework for Caching in Microservice Graphs | 2024 / USENIX NSDI | Add inter-service caches to arbitrary microservice graphs with non-blocking coherence/invalidation | Microservice graph applications and large-scale A/B evaluation | latency, throughput, cache effectiveness, 99th-percentile outcomes | Paper: https://www.usenix.org/conference/nsdi24/presentation/zhang-haoran; code: https://github.com/eniac/mucache | DIRECT | Graph caching, dependencies, coherence, and invalidation are established. The candidate must measure harm after stale data is consumed, not repackage coherence. |
| Skybridge: Bounded Staleness for Distributed Caches | 2025 / USENIX OSDI | Bound update visibility delay with an out-of-band replication stream | Large distributed cache deployments | wall-clock staleness bound, fraction of writes meeting bound, system footprint | https://www.usenix.org/conference/osdi25/presentation/lyerly | DIRECT | Staleness age/visibility bounds are established. Downstream application work must be separated from time-to-visibility. |
| Cache Serializability: Reducing Inconsistency in Edge Transactions (T-Cache) | 2015 / IEEE ICDCS | Detect inconsistent read-only cached transactions using dependency information when inconsistency is tolerable but costly | Synthetic clustered access and workloads based on real-world topologies | inconsistency detection, consistent-transaction rate, overhead, performance/consistency trade-off | DOI 10.1109/ICDCS.2015.75; preprint https://arxiv.org/abs/1409.8324 | DIRECT | Dependency-aware detection and the idea that inconsistency has cost are established. The candidate must quantify downstream work/side effects across service paths and prove non-equivalence. |
| CausalMesh: A Causal Cache for Stateful Serverless Computing | 2024/2025 / PVLDB and extended verification report | Provide causally consistent caching when workflows migrate across servers | Stateful serverless workflows with multiple caches | transactional/causal semantics, abort/coordination properties, performance | Paper DOI 10.14778/3704965.3704969; code https://github.com/eniac/causalmesh; extended report https://arxiv.org/abs/2508.15647 | SUBSTANTIAL | Causal metadata and consistency are established. The candidate must not call causal dependency tracking novel. |
| Probabilistically Bounded Staleness for Practical Partial Quorums | 2012 / PVLDB | Predict version and wall-clock staleness for partial quorums | Dynamo-style replicated stores and production-inspired workloads | version-based and time-based staleness probabilities | https://www.vldb.org/pvldb/vol5/p776_peterbailis_vldb2012.pdf | SUBSTANTIAL | Quantifying staleness probability/age is established; the proposed metric must measure attributable application harm rather than another staleness bound. |
| Use of Stale Answers in Database Applications | 1992 / ICIS | Refresh cached data when the expected cost of using stale answers crosses a threshold | Database applications using cached objects | application-defined stale-answer cost versus refresh cost | https://aisel.aisnet.org/icis1992/1/ | DIRECT | Cost-triggered refresh is decades-old prior art. “Revalidate when expected stale cost is high” is not sufficient differentiation. |
| Study of Piggyback Cache Validation for Proxy Caches | 1997 / USENIX USITS | Improve coherency while reducing validation traffic by piggybacking checks | Trace-driven proxy-cache workloads | coherency/staleness and request traffic | https://www.usenix.org/conference/usits-97/study-piggyback-cache-validation-proxy-caches-world-wide-web | SUBSTANTIAL | Selective/combined validation techniques and traffic trade-offs are established; a new policy must use a distinct downstream-work signal and compare validation cost. |
| LRC: Dependency-Aware Cache Management for Data Analytics Clusters | 2017 / research paper | Use application DAG dependencies for cache replacement | Data-analytics DAGs, including Spark implementation | application runtime, hit behavior, reference counts | https://arxiv.org/abs/1703.08280 | PARTIAL | Dependency-DAG-aware cache policy is established outside microservices. DAG awareness alone is not novel. |

## Candidate metric boundary

A working **Staleness Propagation Cost (SPC)** cannot be a renamed form of:

- stale-read count;
- version or wall-clock staleness;
- transaction inconsistency detection;
- a generic application-defined penalty for stale answers;
- cache hit/miss or validation traffic;
- causal-consistency violations.

A defensible metric must attribute concrete downstream effects to stale input, with explicit units and counterfactuals. Candidate components include otherwise-unnecessary CPU time, RPCs, retries, compensations, invalid cache fills, and critical-path delay.

## Attribution requirements

1. Record a request/causal identifier across every service edge.
2. Define an oracle or replay that identifies the outcome under fresh state.
3. Attribute only the delta in downstream work caused by stale input.
4. Define rules for shared work, fan-out, retries, and compensation to prevent double counting.
5. Report physical units separately; any weighted composite must publish weights and sensitivity analysis.
6. Separate stale-state age from harm because old data can be harmless and recent data can be consequential.

## Current synthesis

The broad ideas of graph-aware caching, bounded staleness, causal/dependency metadata, inconsistency cost, cost-triggered refresh, and selective validation are established. The remaining candidate is narrower: a reproducible causal attribution of **application-level downstream work caused by stale state propagating through a microservice DAG**, plus a policy that uses predicted attributable work—not staleness alone—to decide revalidation.

Mandatory baselines include MuCache-style coherence/invalidation, Skybridge-style bounded visibility, T-Cache-style dependency detection, TTL, asynchronous invalidation, synchronous validation, and a cost-threshold refresh policy.

## Remaining searches before gate completion

- [ ] Retry, compensation, saga, and wasted-work literature under stale or inconsistent state.
- [ ] Provenance/causal-attribution methods for distributed request graphs.
- [ ] Cost-aware validation or freshness policies using application consequences.
- [ ] Forward/backward citation chains from MuCache, Skybridge, T-Cache, PBS, and stale-answer-cost work.
- [ ] Evidence that the proposed physical-unit metric is not known under another name.
- [ ] Artifact compatibility and reproducibility status for mandatory baselines.

## Gate decision

**NOT COMPLETE.** This table rejects broad cost-aware/dependency-aware/selective-validation novelty, but downstream-work attribution, compensation/retry, provenance, and citation-chain searches remain open.
