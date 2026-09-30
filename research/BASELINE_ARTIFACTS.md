# Mandatory baseline artifact inventory

Review date: 2026-09-29

## Status

**PUBLIC ARTIFACT INVENTORY COMPLETE FOR CURRENT MANDATORY BASELINES; EXECUTION NOT VERIFIED.**

This inventory distinguishes public code, paper specifications, standards-based infrastructure, and harness-native controls. Repository metadata, README instructions, current public heads, and top-level license files were checked where available. No baseline is marked runnable, reproduced, or behaviorally equivalent because no build, deployment, smoke test, or conformance test was executed during this review.

## Reproducibility rule

Repository discovery is not reproduction. Mark a baseline executable only after recording:

1. repository and pinned revision;
2. usable license or explicit permission;
3. successful build and deployment commands;
4. environment manifest, including topology, cache/store versions, tracing configuration, and workload;
5. smoke-test output; and
6. a behavioral check against the paper's mechanism.

If official code is absent or incompatible, any replacement must be labeled a reimplementation, include conformance tests, and document deviations. A simplified TTL, invalidation, or tracing component must not be presented as an exact scholarly artifact.

## Inventory

| Mandatory baseline or infrastructure | Artifact evidence checked | Initial classification | Experiment decision |
|---|---|---|---|
| MuCache-style coherence/invalidation | Official public repository `eniac/mucache`, pinned discovery head `e04e7a03242673e355433c3fe9ef06017fb47910`; README, experiment guide, and MIT license verified | Licensed official artifact with large environment dependency | The provided reproduction uses a 13-machine CloudLab cluster, Kubernetes 1.26.1, prebuilt images, four real applications, and four synthetic topologies. Treat full execution as a separate reproduction task. A reduced integration must be labeled and validated against MuCache's coherence/invalidation behavior. |
| Skybridge-style bounded visibility | Paper identified; no public author repository verified in the bounded GitHub search | Paper/specification only | Implement a labeled visibility-bound baseline only if its observable contract can be tested. Do not call a generic delayed invalidation queue “Skybridge.” |
| T-Cache dependency-aware inconsistency detection | Paper identified; no public author artifact verified in the bounded GitHub search | Paper/specification only | Reimplement only the required dependency/inconsistency decision rule with synthetic transaction tests and known consistent/inconsistent cases. |
| CausalMesh causal caching | Official public repository `eniac/causalmesh`, pinned discovery head `65a192dcd34d82191e34109a8e468a4343a91fee`; README and MIT license verified | Licensed official artifact with substantial environment dependency | The artifact uses CloudLab, Redis, Nightcore, Rust server code, Go clients, and multiple protocol modes. It is a causal-consistency comparator, not a substitute for stale-work attribution. Pin all dependencies and validate protocol mode before use. |
| Probabilistically Bounded Staleness | Paper and equations identified; no maintained software artifact verified | Analytical/model baseline | Implement version/time-bound calculations with unit tests and use them as staleness predictors, not as causal-work measures. |
| TTL-only caching | Harness-native | Directly implementable | Define expiry semantics, clock source, jitter, refresh-on-read behavior, and warmup. Sweep TTL rather than choosing a single favorable value. |
| Asynchronous invalidation | Harness-native | Directly implementable | Record delivery delay, loss/reordering policy, retries, queue depth, and version carried by each invalidation. |
| Synchronous validation | Harness-native | Directly implementable | Record validation RPCs, latency, failures, and cache-fill effects. Ensure the fresh-state oracle is independent of this baseline. |
| Piggyback validation | Paper identified; no maintained artifact verified | Paper/specification only | Implement only as a labeled reimplementation with validation-traffic and coherency tests. |
| Stale-answer cost threshold | Historical paper identified; no maintained artifact verified | Paper/specification only | Implement a transparent cost-threshold policy and report its units separately from physical-work attribution. |
| Consistency Rationing penalty-cost policy | Paper identified; no maintained author artifact verified | Paper/specification only | Use a labeled reimplementation with explicit penalty and operation-cost functions. Do not substitute the candidate physical-work metric for the published application-penalty concept. |
| Pivot-Tracing-style causal correlation | Paper/prototype evidence identified, but no maintained canonical implementation repository was verified in the bounded search | Infrastructure/specification reference | Use W3C-compatible trace context and explicit happened-before joins in the harness. Label this as equivalent infrastructure only after trace-join tests across fan-out, async, and retry paths. |
| W3C Trace Context | Normative standard; supported by common tracing libraries | Standards-based infrastructure | Record propagation library/version, sampling policy, missing-context handling, and asynchronous-boundary behavior. Trace identifiers alone are not an experimental contribution. |
| Hindsight-style rare-event capture | Paper identified; no public author repository verified in the bounded GitHub search | Paper/specification only | If implemented, label it as symptom-triggered retention inspired by Hindsight and test selection bias and capture completeness. |
| Fresh-state replay/oracle | Candidate-harness component | Must be designed and validated | Replay must hold request, randomness, dependency versions, and external side effects constant. Publish mismatch and nondeterminism handling. |
| Stale-version-conditioned work attribution | Candidate-harness component | Must be designed and validated | Propagate an explicit stale-version marker, join it to physical CPU/RPC/retry/compensation/cache-fill work, and prevent double counting across shared work and fan-out. |

## Readiness classes

- **Class A — official and MIT-licensed, environment validation pending:** MuCache; CausalMesh.
- **Class B — standards or harness-native controls:** W3C Trace Context; TTL; asynchronous invalidation; synchronous validation.
- **Class C — analytical or paper/specification only:** Skybridge; T-Cache; PBS; piggyback validation; stale-answer cost; Consistency Rationing; Pivot-Tracing-style joins; Hindsight.
- **Candidate instrumentation requiring independent validation:** fresh-state replay and stale-version-conditioned physical-work attribution.

No class means “reproduced.” Class A means only that license and public source are available for a future build gate.

## Workload compatibility

A baseline is comparable only if it can consume the same declared request trace, cache versions, invalidation schedule, fan-out topology, and side-effect model while emitting the same physical-unit telemetry. Full systems such as MuCache and CausalMesh must not be reduced to a feature label or compared as black boxes without documenting topology, consistency contract, and workload differences.

## Completion gate

Public-artifact discovery is complete for the current mandatory list. Baseline compatibility remains open until every selected comparator has either:

- a pinned, licensed, build- and smoke-verified artifact plus behavioral validation; or
- a preregistered reimplementation with unit/conformance tests and explicit deviations.

Missing code, incompatible topology, or a large CloudLab deployment is a constraint to report, not permission to silently remove or weaken a baseline.
