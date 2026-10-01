# Differentiation — Causal Staleness in Microservice Caches

Review date: 2026-09-29  
Reconciled evidence head: `fb329f6bd04d4656354845e3ca2a9e9fa48ebd2d`  
Status: **PROVISIONAL — NOVELTY UNVERIFIED**

## Research question

Can application-level downstream work caused by stale state propagating through a multi-tier microservice DAG be measured reproducibly and reduced by cost-driven selective revalidation beyond existing coherence, bounded-staleness, dependency-aware inconsistency detection, and causal-cache mechanisms?

## Closest established work and exact boundary

| Established work | Overlap | Not our contribution | Candidate differentiation |
|---|---|---|---|
| MuCache, NSDI 2024 | SUBSTANTIAL | Caching and non-blocking coherence/invalidation in microservice graphs | Measuring request-level downstream harm after stale state is consumed |
| Skybridge, OSDI 2025 | SUBSTANTIAL | Bounding cache replication staleness | Connecting stale-state age/visibility to application work and side effects |
| T-Cache, ICDCS 2015 | SUBSTANTIAL | Dependency-aware inconsistency detection; inconsistency as a cost | Propagation cost across service paths and cost-driven path revalidation, if distinct |
| CausalMesh, PVLDB 2024/2025 | SUBSTANTIAL | Causal cache metadata, consistency, and protocol artifacts | Empirical downstream-work attribution conditioned on a stale version and fresh replay |
| PBS, piggyback validation, and LRC | SUBSTANTIAL / PARTIAL | Probabilistic staleness bounds, selective validation, and DAG-aware cache policy | Physical downstream-work attribution rather than age, validation traffic, or graph awareness |
| Stale-answer cost and Consistency Rationing | DIRECT | Application penalty/cost thresholds and consequence-aware consistency switching | Separate physical CPU/RPC/retry/compensation/cache-fill units before any weighting |
| Invariant Confluence | SUBSTANTIAL | Application invariants as coordination boundaries | Version-conditioned incremental work, not generic semantic consequence |
| Pivot Tracing, W3C Trace Context, Hindsight, Sagas, and metastability work | FOUNDATIONAL / SUBSTANTIAL / PARTIAL | Cross-tier causal correlation, trace propagation, rare-event capture, compensation, and retry/work amplification | Join an explicit stale-version marker to incremental work, validate with fresh replay, and prevent double counting |

## Candidate metric

A working **Staleness Propagation Cost (SPC)** must be defined from observable application effects, not only stale-read count or visibility delay. Candidate components may include wasted downstream CPU, invalid external calls, retries, compensations, incorrect cache fills, and additional critical-path latency attributable to stale input.

The metric must:

- identify the counterfactual or oracle used for attribution;
- state units and aggregation rules;
- avoid double-counting shared downstream work;
- separate stale-state age from resulting harm;
- declare whether side effects are weighted and how weights are justified.

The name and formulation remain provisional.

### Preregistered attribution specification

For every root request (r), publish the unweighted vector
(Delta W_r=(Delta CPU_{ns},Delta RPC_{count},Delta RPC_{bytes},Delta retry_{count},Delta compensation_{count},Delta cacheFill_{count},Delta criticalPath_{ns})), computed as stale execution minus a matched fresh execution. Do not clip negative deltas. Any scalar score is secondary, declares unit conversions and weights, and includes sensitivity analysis.

A matched fresh-state oracle must fix request bytes/logical ID, dependency-version snapshot, invalidation and fault schedule, random seeds, images/configuration, and recorded/idempotent external effects. Record output and side-effect digests. Exclude and report pairs with missing context or unexplained digest/scheduling mismatch above a preregistered tolerance.

Accounting rules:

1. charge every physical span once to one attempt and deduplicate by span/work ID;
2. identify retries by stable logical-operation ID plus attempt number, counting physical work per attempt and logical retries only after the first;
3. count compensation only when linked to a stale-caused side effect absent from the matched fresh run;
4. charge a coalesced/batched shared-work ID once, using equal-share allocation for the primary result and full-charge/causal-trigger sensitivity analyses;
5. charge a cache fill once by fill ID; waiters may inherit latency but not duplicate its CPU/RPC work; and
6. admit work only when it descends from an explicit consumed-stale-version marker and is absent or smaller in the matched fresh trace.

Null controls hold offered load and faults constant with no stale reads, and repeat below/above the overload knee. If queueing/service-time variables explain the delta without the stale marker, if the effect persists in the no-staleness control, or if replay nondeterminism invalidates pairing, reject or narrow the stale-causation claim.

## Candidate contributions

1. A reproducible attribution method for downstream work caused by stale state in a service DAG.
2. A workload and fault model that varies propagation depth, fan-out, invalidation delay, and consequence severity.
3. Cost-driven selective revalidation evaluated against:
   - TTL-only caching;
   - asynchronous invalidation;
   - synchronous validation;
   - bounded-staleness configuration;
   - dependency-aware inconsistency detection where implementable;
   - piggyback validation;
   - stale-answer-cost thresholding;
   - Consistency-Rationing-style penalty-cost control;
   - ordinary W3C-compatible tracing and Pivot-Tracing-style joins.
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

- [x] Formal closest-work evidence covers coherence, bounded staleness, dependency detection, causal caching, cost/penalty policies, tracing, compensation, and retry amplification.
- [x] Baseline classes isolate coherence, staleness bounds, causal consistency, selective validation, cost policies, and tracing infrastructure.
- [x] Public artifact availability and initial topology/environment/license constraints are inventoried.
- [x] A bounded synonym search covers inconsistency/stale-answer penalty, freshness debt, wasted work, retry/work amplification, compensation, selective stale-view cleaning, monotonic caching, and dependency-aware validation; no equivalent was verified, without claiming exhaustive absence.
- [x] Backward/forward citation-chain checks are documented for all five seeds; representative forward constraints include T-Cache → monotonic-consistent caching, PBS → PBS-at-Work, and stale-answer cost → stale-view cleaning. MuCache/Skybridge forward chains remain shallow because of recency and are labeled accordingly.
- [x] Exact physical units, deterministic fresh-replay pairing, fan-out/retry/compensation/cache-fill/shared-work rules, deduplication keys, sensitivity reporting, and overload/nondeterminism falsifiers are specified.
- [x] The direct-evidence search is bounded and accurately labeled: verified adjacent mechanisms do not provide the proposed per-request stale-version-conditioned physical-work vector, but this is not an exhaustive novelty conclusion.
- [ ] Selected baselines pass pinned build, smoke, behavioral, and workload-compatibility checks.
- [x] Candidate claims remain labeled unverified.

## Gate decision

**NOT COMPLETE.** The earlier “formal closest-work table missing” blocker is obsolete and has been cleared. The gate remains open for executable-baseline verification. Scholarly search closure is bounded rather than universal, and implementation may reopen the novelty audit if new terminology or evidence appears. No experiment-design or implementation gate may start from this reconciliation alone.
