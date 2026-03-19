# Improve Observability

## Problem

The visible code shows baseline Nest logging, but not much evidence of structured operational observability. Silent catches actively reduce useful diagnostics.

## Why It Matters

For a continuously running finance-related application, supportability matters almost as much as correctness.

## Recommended Changes

1. Add structured logs around:
   - Redis cache failures
   - portfolio recalculation failures
   - import failures
   - external market-data/provider errors
2. Include request or job correlation IDs where feasible.
3. Log fallback decisions, not just hard failures.
4. Add a short operator-facing observability section to the docs.

## Expected Impact

- faster root-cause analysis
- easier support for self-hosted users
- better confidence during incident response
