# 07-logging — logging

**Score:** 5.4/10

## Summary
Ghostfolio has baseline logging through Nest and startup banners, but the visible code does not suggest deep observability. Logging appears functional rather than structured, and silent catches directly undermine diagnosis.

## Strengths
- Startup logging is configurable through LOG_LEVELS and environment-dependent defaults.
- Nest logger integration gives a standard baseline.

## Weaknesses
- Silent catch blocks erase important operational context.
- The inspected evidence does not show structured correlation, metrics, or tracing patterns.
- Recent bug reports still rely heavily on user-provided logs, suggesting observability could be stronger.

## Findings
- **Logging is configurable but basic** (medium): main.ts derives log levels from configuration and falls back to a production/development default set. Evidence: apps/api/src/main.ts
- **Silent exception handling weakens logs** (medium): Empty catch blocks remove the very information logs should surface during config and request processing failures. Evidence: apps/api/src/main.ts, apps/api/src/app/app.module.ts
- **Observed issue workflow depends on raw logs** (low): Several bug reports cite manual server logs instead of richer operational diagnostics, which is normal but suggests limited observability tooling from the visible repo surface. Evidence: issues #4299, #5924, #5006

## Recommendations
- Introduce structured logging around cache failures, external provider failures, and portfolio recalculation paths.
- Log suppressed fallback events with enough context to diagnose bad configuration and malformed headers.
- Consider adding request correlation IDs and a small observability guide for self-hosted operators.

## Confidence
medium
