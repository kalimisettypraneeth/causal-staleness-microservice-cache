# Causal Staleness in Microservice Caches

Research artifact for studying stale-state propagation through multi-tier microservice dependency graphs and selective revalidation using freshness metadata.

## Working paper title
**Causal Staleness Propagation and Selective Revalidation in Multi-Tier Microservice Caches**

## Research question
How does stale state propagate through multi-tier microservice dependency graphs, and can causal freshness metadata reduce the downstream computational and transactional cost of stale reads without synchronous validation on every request?

## Current status
Research scaffold established. Cache consistency and bounded staleness are treated as established areas; SPC remains a candidate metric pending prior-art validation.
