# 06-exception-handling — exception_handling

**Score:** 5.6/10

## Summary
Exception handling is the weakest technical area in the visible evidence. There are multiple silent catch blocks and at least one documented Redis-related crash, which indicates that exception paths are not consistently surfaced or normalized.

## Strengths
- NestJS provides a framework that can support clean exception handling patterns.
- Some recent bugs have been recognized and discussed publicly, which helps drive remediation.

## Weaknesses
- Silent catch blocks swallow exceptions without logging or remediation.
- Exception behavior appears uneven across infrastructure and startup paths.
- Operational exception paths around Redis have already caused application crashes.

## Findings
- **Silent catch in startup config parsing** (high): main.ts catches JSON parsing errors for LOG_LEVELS and does nothing with the exception. Evidence: apps/api/src/main.ts
- **Silent catch in Accept-Language processing** (high): AppModule catches parsing errors when reading Accept-Language for redirects and suppresses them entirely. Evidence: apps/api/src/app/app.module.ts
- **Redis exception path previously crashed the app** (high): Issue #4822 documents an application crash caused by an exception in @keyv/redis inside the Redis cache service. Evidence: issue #4822

## Recommendations
- Eliminate empty catch blocks; at minimum log the exception and the fallback path used.
- Standardize exception translation for infrastructure services such as Redis, Prisma, and external data providers.
- Add regression tests specifically for previously reported exception bugs.

## Confidence
high
