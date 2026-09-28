# Differentiation — Causal Staleness in Microservice Caches

Review date: 2026-09-28  
Source audit head: `5177f8c1a030edb0084018ee9f74bf5a7de52ffe`  
Status: **PROVISIONAL — NOVELTY UNVERIFIED**

## Research question

Can application-level downstream work caused by stale state propagating through a multi-tier microservice DAG be measured reproducibly and reduced by cost-driven selective revalidation beyond existing coherence, bounded-staleness, dependency-aware inconsistency detection, and causal-cache mechanisms?

## Closest established work and exact boundary

| Established work | Overlap | Not our contribution | Candidate differentiation |
|---|---|---|---|
| MuCache, NSDI 2024 | SUBSTANTIAL | Caching and non-blocking coherence/invalidation in microservice graphs | Measuring request-level downstream harm after stale state is consumed |
| Skybridge, OSDI 2025 | SUBSTANTIAL | Bounding cache replication staleness | Connecting stale-state age/visibility to application work and side effects |
| T-Cache, ICDCS 2015 | SUBSTANTIAL | Dependency-aware inconsistency detection; inconsistency as a cost | Propagation cost across service paths and cost-driven path revalidation, if distinct |
| CausalMesh, 2025 preprint | PARTIAL | Causal cache metadata and consistency | Empirical downstream-work metric and policy in multi-tier microservice DAGs |

## Candidate metric

A working **Staleness Propagation Cost (SPC)** must be defined from observable application effects, not only stale-read count or visibility delay. Candidate components may include wasted downstream CPU, invalid external calls, retries, compensations, incorrect cache fills, and additional critical-path latency attributable to stale input.

The metric must:

- identify the counterfactual or oracle used for attribution;
- state units and aggregation rules;
- avoid double-counting shared downstream work;
- separate stale-state age from resulting harm;
- declare whether side effects are weighted and how weights are justified.

The name and formulation remain provisional.

## Candidate contributions

1. A reproducible attribution method for downstream work caused by stale state in a service DAG.
2. A workload and fault model that varies propagation depth, fan-out, invalidation delay, and consequence severity.
3. Cost-driven selective revalidation evaluated against:
   - TTL-only caching;
   - asynchronous invalidation;
   - synchronous validation;
   - bounded-staleness configuration;
   - dependency-aware inconsistency detection where implementable.
4. Evidence showing benefit beyond stale-read reduction alone.

## Claims this paper must not make

- Microservice graph caching or invalidation is new.
- Bounded staleness, causal metadata, dependency-aware caching, or inconsistency cost is new.
- Stale-read frequency or visibility delay alone measures application harm.
- A metric name establishes originality.
- Synthetic weights prove real operational cost.

## Falsifiers

The candidate contribution is weakened or rejected if:

- SPC collapses to stale-read count, age, or transaction inconsistency already covered by prior work;
- causal attribution cannot separate stale input from unrelated retries or failures;
- selective revalidation offers no advantage over a tuned TTL or synchronous validation baseline;
- results depend entirely on arbitrary side-effect weights;
- prior work already measures equivalent downstream propagation cost under another name.

## Evidence required to pass this gate

- [ ] Synonym search covers inconsistency cost, wasted work, retry/compensation amplification, freshness debt, and dependency-aware validation.
- [ ] Backward/forward citation chains from MuCache, Skybridge, T-Cache, and causal-cache work are recorded.
- [ ] SPC units, attribution method, and double-counting rules are specified.
- [ ] Baselines isolate coherence, staleness bounds, causal consistency, and selective validation.
- [ ] Candidate originality remains unverified until the closest-work table is complete.

## Gate decision

**NOT COMPLETE.** The differentiation target, metric constraints, and falsifiers are fixed, but the gate remains open pending the synonym/citation search and formal closest-work table.
