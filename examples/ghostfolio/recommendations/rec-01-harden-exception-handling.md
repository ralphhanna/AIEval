# Harden Exception Handling

## Problem

The visible backend code includes silent `catch {}` blocks in startup and request-routing paths. A documented Redis-related bug also shows that infrastructure exceptions have previously escaped in ways that crashed the application.

## Why It Matters

Silent exception handling creates two costs:
- operators cannot see the root cause quickly
- developers normalize fallback behavior that may hide configuration and data-quality problems

## Specific Targets

- `apps/api/src/main.ts`
- `apps/api/src/app/app.module.ts`
- Redis cache service and adjacent cache access paths

## Recommended Changes

1. Replace empty catches with explicit logging.
2. Include enough context to explain:
   - what failed
   - what fallback was used
   - whether the request can proceed safely
3. Standardize error translation for infrastructure concerns such as:
   - Redis
   - Prisma / database access
   - external data providers
4. Add regression tests for known exception bugs.

## Example Direction

Instead of swallowing:
```ts
try {{
  customLogLevels = JSON.parse(configService.get('LOG_LEVELS'));
}} catch {{}}
```

Use a logged fallback path:
```ts
try {{
  customLogLevels = JSON.parse(configService.get('LOG_LEVELS'));
}} catch (error) {{
  Logger.warn('Invalid LOG_LEVELS configuration, using defaults');
}}
```

## Expected Impact

- easier production diagnosis
- fewer hidden misconfigurations
- more stable cache and startup behavior
