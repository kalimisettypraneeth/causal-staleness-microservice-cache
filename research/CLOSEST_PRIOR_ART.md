# Closest Prior Art — Causal Staleness in Microservice Caches

Review date: 2026-09-29  
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
| Consistency Rationing in the Cloud: Pay Only When It Matters | 2009 / PVLDB | Dynamically vary consistency by data category and estimated consistency-violation penalty versus operation cost | Amazon S3 prototype with TPC-W | response time, service calls, incorrect operations, monetary penalty including compensation for booking errors | https://www.vldb.org/pvldb/vol2/vldb09-759.pdf | DIRECT | Application-consequence and compensation-penalty-aware consistency switching is established. The candidate must measure causally attributable physical downstream work in separate units, not rename a monetary or application penalty model. |
| Coordination Avoidance in Database Systems | 2015 / PVLDB | Use invariant confluence to decide when coordination is necessary to preserve application correctness | Database invariants and TPC-C prototype on a 200-server cluster | invariant preservation, coordination, throughput/performance | https://www.vldb.org/pvldb/vol8/p185-bailis.pdf | SUBSTANTIAL | Application semantics and invariant risk already guide consistency/coordination choices. The candidate must not equate “consequential stale data” with novelty; it needs version-conditioned causal attribution and a fresh-state counterfactual. |
| Study of Piggyback Cache Validation for Proxy Caches | 1997 / USENIX USITS | Improve coherency while reducing validation traffic by piggybacking checks | Trace-driven proxy-cache workloads | coherency/staleness and request traffic | https://www.usenix.org/conference/usits-97/study-piggyback-cache-validation-proxy-caches-world-wide-web | SUBSTANTIAL | Selective/combined validation techniques and traffic trade-offs are established; a new policy must use a distinct downstream-work signal and compare validation cost. |
| LRC: Dependency-Aware Cache Management for Data Analytics Clusters | 2017 / research paper | Use application DAG dependencies for cache replacement | Data-analytics DAGs, including Spark implementation | application runtime, hit behavior, reference counts | https://arxiv.org/abs/1703.08280 | PARTIAL | Dependency-DAG-aware cache policy is established outside microservices. DAG awareness alone is not novel. |
| Stale View Cleaning: Getting Fresh Answers from Stale Materialized Views | 2015 / PVLDB | Clean samples from stale materialized views and estimate fresh aggregate answers without full maintenance | TPC-D-derived data and a real video-distribution application | answer error/accuracy, cleaning and maintenance cost | https://arxiv.org/abs/1509.07454 | SUBSTANTIAL | Fresh-answer estimation from stale state and selective cleaning are established. It does not attribute downstream microservice work to a consumed stale version. |
| Making Cache Monotonic and Consistent | 2023 / PVLDB | Enforce consistency and monotonicity for cache-served application reads | database/application-server/cache model with batch and online request policies | consistency/monotonicity, latency and policy cost | https://www.vldb.org/pvldb/vol16/p891-cao.pdf | SUBSTANTIAL | This forward citation of T-Cache confirms that transactional/monotonic cache correctness remains active prior art. Correctness enforcement is not the proposed physical-work attribution. |
| PBS at Work: Advancing Data Management with Consistency Metrics | 2013 / ACM SIGMOD demonstration | Expose PBS version/time staleness metrics for configuration and operational analysis | partial-quorum data-store configurations | k-staleness and t-visibility | https://pages.cs.wisc.edu/~shivaram/publications/pbs-demo-sigmod12.pdf | SUBSTANTIAL | This forward continuation of PBS operationalizes staleness metrics; it still measures version/time recency rather than stale-caused downstream work. |

| Pivot Tracing: Dynamic Causal Monitoring for Distributed Systems | 2015 / ACM SOSP | Correlate metrics and events across thread, process, application, and machine boundaries using propagated baggage and happened-before joins | Java-based HDFS, HBase, MapReduce, and YARN cluster | cross-tier causal queries, root-cause localization, execution overhead | DOI 10.1145/2815400.2815415; paper https://sigops.org/s/conferences/sosp/2015/current/2015-Monterey/122-mace-online.pdf | SUBSTANTIAL | Cross-service causal-path attribution and propagated per-request metadata are established. The candidate must condition attribution on stale-state consumption, measure physical work deltas, and validate them against a fresh-state counterfactual. |
| W3C Trace Context | 2021 / W3C Recommendation | Standardize trace identifiers and vendor-neutral propagation across distributed components | Distributed applications and microservices | trace-id, parent-id, flags, tracestate propagation | https://www.w3.org/TR/trace-context/ | FOUNDATIONAL | Request identifiers across service edges are standard infrastructure, not a contribution. The paper must specify sampling, fan-out, asynchronous boundaries, and missing-context handling. |
| Sagas | 1987 / ACM SIGMOD | Decompose long-lived transactions and run compensating transactions when constituent actions cannot all complete | Long-lived database transactions | completed subtransactions and compensating actions | DOI 10.1145/38713.38742; https://dl.acm.org/doi/10.1145/38714.38742 | FOUNDATIONAL | Compensation is established transactional recovery. Novelty must be attribution of compensation work specifically caused by stale input, not defining compensation or merely counting saga steps. |
| The Benefit of Hindsight: Tracing Edge-Cases in Distributed Systems | 2023 / USENIX NSDI | Persist detailed distributed traces retroactively after symptoms such as high tail latency, errors, or bottlenecked queues | High-rate distributed applications | trace capture rate/overhead and symptom-triggered retrieval | https://www.usenix.org/conference/nsdi23/presentation/zhang-lei | SUBSTANTIAL | Rare harmful paths can be captured after symptoms without tracing every request eagerly. The candidate needs a stale-state trigger/oracle and unbiased accounting, not just symptom-triggered trace retention. |
| Metastable Failures in the Wild | 2022 / USENIX OSDI | Characterize triggers and amplification mechanisms in severe distributed-system failures | 22 incidents from 11 organizations plus controlled reproductions | triggers, sustaining effects, queue/retry/work amplification | https://www.usenix.org/conference/osdi22/presentation/huang-lexiang | PARTIAL | Retry and work amplification are established failure mechanisms. The cache paper must isolate stale input as the cause, distinguish ordinary overload/metastability, and count only incremental work. |

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

### Physical-unit vector before weighting

For each root request (r), report the unweighted vector
(Delta W_r=(Delta CPU_{ns},Delta RPC_{count},Delta RPC_{bytes},Delta retry_{count},Delta compensation_{count},Delta cacheFill_{count},Delta criticalPath_{ns})).
Each delta is stale execution minus its matched fresh execution. Negative deltas remain visible; they are not clipped. A scalar policy score, if later used, is secondary, declares dimensional conversion/weights, and requires weight-sensitivity analysis.

### Deterministic fresh-state oracle

A stale/fresh pair is admissible only when the harness fixes the request bytes and logical root ID, dependency-version snapshot, invalidation schedule, pseudo-random seeds, service/container images, configuration, and fault schedule. External side effects use an idempotent sandbox or recorded deterministic responses. The run records output and side-effect digests. Pairs with digest mismatch unrelated to the stale version, missing trace context, or nondeterministic scheduling beyond a preregistered tolerance are excluded and reported; exclusion counts and reasons are published.

### Fan-out, retry, and shared-work accounting

- **Fan-out:** every physical span is charged once to its owning attempt; a parent aggregates unique child span IDs and never re-adds descendants through multiple paths.
- **Retries:** every attempt has a stable logical-operation ID plus attempt number. Attempt CPU/RPC bytes count physically; logical retry count is the number of attempts after the first. The same attempt is never counted once as RPC work and again as a separate aggregate work unit.
- **Compensation:** compensation spans carry the side-effect ID they undo and count only when the matched fresh run does not require that compensation.
- **Shared work:** coalesced/batched work has one physical work ID. Primary results allocate it once using preregistered equal-share attribution across participating root requests; sensitivity results also report full-charge and causal-trigger allocation.
- **Cache fills:** a fill is charged once by fill ID. Waiters inherit latency but not duplicate fill CPU/RPC work.
- **Join rule:** only descendants of an explicit consumed-stale-version marker and absent (or smaller) in the matched fresh trace contribute to the stale-conditioned delta.

### Null explanations and falsification

Run matched fresh/stale experiments below and above the overload knee, plus a no-staleness overload control with the same offered load and fault schedule. If queue depth/service time predicts the work delta without the stale marker, if the delta persists in the no-staleness control, or if replay mismatch exceeds the preregistered tolerance, attribute the observation to ordinary overload/metastability or nondeterminism and reject or narrow the causal claim.

## Current synthesis

The broad ideas of graph-aware caching, bounded staleness, causal/dependency metadata, inconsistency cost, penalty-aware consistency rationing, invariant-aware coordination, cost-triggered refresh, and selective validation are established. The remaining candidate is narrower: a reproducible causal attribution of **application-level downstream work caused by stale state propagating through a microservice DAG**, plus a policy that uses predicted attributable work—not staleness alone—to decide revalidation.

Mandatory baselines include MuCache-style coherence/invalidation, Skybridge-style bounded visibility, T-Cache-style dependency detection, TTL, asynchronous invalidation, synchronous validation, a stale-answer cost threshold, and a Consistency-Rationing-style penalty-cost policy. Attribution must also compare against ordinary W3C-compatible distributed tracing/Pivot-Tracing-style causal correlation, while rare-event capture should account for Hindsight-style symptom-triggered retrieval.

## Remaining searches before gate completion

- [x] Foundational saga compensation and general retry/work-amplification literature; no stale-specific equivalence inferred.
- [x] Bounded direct search for stale/inconsistent-state retry, compensation, and wasted-work studies completed; adjacent penalty, cleaning, tracing, and amplification work was found, but no verified source in this bounded search measured the proposed per-request stale-version-conditioned physical-work vector. This is not an exhaustive novelty claim.
- [x] Distributed trace-context propagation, happened-before causal correlation, and symptom-triggered trace capture.
- [x] Stale-state-conditioned causal-attribution search bounded; no verified equivalent found in the searched primary-source set, and the candidate oracle/accounting protocol is now stated for falsification.
- [x] Cost- and penalty-aware validation/consistency policies using application consequences (1992 stale-answer cost and 2009 Consistency Rationing).
- [x] Application-invariant-aware coordination boundaries (Invariant Confluence).
- [x] Backward/forward chains were checked for the five seeds; representative constraining links are recorded (including T-Cache → monotonic-consistent caching, PBS → PBS-at-Work, and stale-answer cost → stale-view cleaning). MuCache/Skybridge chains are necessarily shallow because of recency and are not treated as complete-universe evidence.
- [x] Bounded synonym search covered freshness debt/penalty, inconsistency cost, stale-answer cost, wasted work, work/retry amplification, selective cleaning, monotonic caching, and consequence-aware consistency; no equivalent verified, without asserting exhaustive absence.
- [x] Public artifact availability, initial license, topology, and environment constraints inventoried in `research/BASELINE_ARTIFACTS.md`.
- [ ] Build, smoke, behavioral-conformance, and workload-compatibility verification for selected executable baselines.

## Gate decision

**NOT COMPLETE.** This table now also rejects novelty claims based on application penalty cost, compensation cost, or invariant-aware consistency switching. Public baseline-artifact availability is inventoried, but the scholarly search and attribution specification are now bounded and documented, but baseline build, smoke, behavioral-conformance, and workload-compatibility verification remains open.
