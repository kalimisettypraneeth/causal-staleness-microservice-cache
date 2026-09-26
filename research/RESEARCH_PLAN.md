# Research Plan

## Hypothesis
Stale values can impose measurable downstream computational and transactional costs as they traverse a service dependency graph, and causal freshness metadata may reduce that cost with less overhead than synchronous validation on every request.

## Candidate metric
Staleness Propagation Cost (SPC), subject to prior-art validation and possible renaming.

## Independent variables
- cache TTL
- invalidation delay
- dependency depth/fanout
- write/read ratio
- event-delivery lag
- cache hit rate
- request freshness requirements

## Dependent variables
- stale-read probability
- duration and path length of staleness
- redundant downstream calls
- transactional retries/compensation
- compute/network overhead
- p95/p99 latency
- validation overhead

## Baselines
1. TTL-only
2. asynchronous invalidation
3. synchronous validation
4. proposed freshness-metadata selective revalidation
