# Ghostfolio AI-Eval Report

Repository: `ghostfolio/ghostfolio`  
Framework: `ralphhanna/AI-Eval`  
Date: 2026-03-19

## Repository Summary

Ghostfolio is an open-source wealth management application organized as an Nx workspace with a NestJS backend and Angular frontend. This evaluation follows the AI-Eval instruction to generate per-prompt outputs, markdown summaries, and detailed recommendation files.

## Score Table

| Category | Score |
|---|---|
| simplicity | 6.8/10 |
| design_quality | 7.8/10 |
| test_quality | 7.2/10 |
| error_handling | 6.3/10 |
| exception_handling | 5.6/10 |
| logging | 5.4/10 |
| aggregate | 6.6/10 |

## Overall Conclusion

Ghostfolio is a mature, real-world codebase with strong architectural bones and meaningful automated testing. The biggest quality drag is not overall design but operational robustness at the edges: oversized orchestration files, silent catch blocks, and modest observability.

## Priority Actions

1. **Harden exception handling and remove silent catches** — replace empty catches with explicit logging and safe error translation.
2. **Refactor oversized portfolio/controller hotspots** — split large files into narrower services and handlers.
3. **Improve observability and failure-path tests** — add structured logs and regression tests around cache/config/portfolio failures.

## Simplicity — 6.8/10

Ghostfolio is understandable at the repo level, but several core API paths concentrate too much behavior in single files and heavy bootstrap/module composition makes local reasoning harder than it should be.

**Strengths**
- Clear top-level workspace organization with apps, libs, prisma, docker, and tools.
- Consistent Nx task scripts make common workflows predictable.
- Use of Nest modules and Angular/Nx conventions reduces accidental complexity in many areas.

**Weaknesses**
- Very large orchestration points reduce readability and make changes riskier.
- Startup and static-serving logic mixes configuration, security, localization, and redirect behavior.
- Feature-rich monorepo setup increases the amount of context needed for small changes.

**Findings**
- **Oversized portfolio controller** (medium): The portfolio controller is 674 lines / 618 LOC, which is too much surface area for a single controller and suggests multiple responsibilities. Evidence: apps/api/src/app/portfolio/portfolio.controller.ts
- **Dense root module composition** (medium): AppModule wires a long list of modules plus conditional Bull Board, Redis, and static-server configuration in one file, making startup behavior harder to reason about. Evidence: apps/api/src/app/app.module.ts
- **Nx monorepo scripts are broad but coherent** (low): The package.json scripts are extensive, but they stay aligned around Nx targets for build, lint, test, serve, and workspace management. Evidence: package.json

**Recommendations**
- Split portfolio.controller.ts into narrower controllers or endpoint-focused modules.
- Move static redirect, Accept-Language handling, and feature-flag setup into dedicated providers or middleware.
- Document the intended ownership boundaries between apps/api, apps/client, libs, and services more explicitly.

## Design Quality — 7.8/10

The overall architecture is strong: Ghostfolio uses a well-organized Nx workspace, a NestJS backend, Angular frontend, Prisma, Redis, and modular feature packaging. The main design weakness is that some modules and controllers have grown too large at the edges.

**Strengths**
- The repo is explicitly organized as an Nx workspace with separate backend and frontend concerns.
- Modular NestJS design is visible through many feature modules and service modules.
- Shared infrastructure like Prisma, configuration, queues, and caching is separated into distinct modules.

**Weaknesses**
- Some edge modules appear to absorb too many concerns over time.
- Conditional composition in AppModule makes the composition root heavy.
- Large files indicate erosion of single-responsibility boundaries in high-traffic areas.

**Findings**
- **Strong workspace structure** (low): The repo exposes apps, libs, prisma, docker, test/import, and tools directories, which is a healthy split for a full-stack Nx application. Evidence: repo root
- **AppModule is a heavy composition root** (medium): The root module imports many application modules and also embeds conditional queue UI configuration, Redis settings, static file rules, and redirect logic. Evidence: apps/api/src/app/app.module.ts
- **Architecture is modular but not consistently thin** (medium): The existence of very large controller files suggests that module boundaries exist but are not always enforced inside features. Evidence: apps/api/src/app/portfolio/portfolio.controller.ts

**Recommendations**
- Refactor oversized feature controllers/services behind thinner endpoint handlers and dedicated application services.
- Keep AppModule focused on composition and move complex static-serving and request-routing behavior into isolated components.
- Add lightweight architecture notes for the main backend domains and shared libraries.

## Test Quality — 7.2/10

Ghostfolio has meaningful automated test infrastructure and evidence of substantial API-domain testing. The main concern is not absence of tests, but uneven confidence caused by concentration around selected areas and lack of visible coverage reporting in the inspected material.

**Strengths**
- Nx/Jest test orchestration is set up cleanly across the workspace.
- The repo exposes dedicated test scripts for all projects and targeted test execution.
- At least one inspected portfolio calculator spec is substantial and uses realistic dependency mocking.

**Weaknesses**
- The inspected evidence does not show explicit coverage thresholds or coverage enforcement.
- Large, heavily mocked tests can still leave integration gaps.
- Visible test strength is stronger in some backend calculation areas than in observability or failure-path validation.

**Findings**
- **Workspace-wide Jest integration exists** (low): Jest is configured through Nx, and the root jest config delegates to getJestProjectsAsync(). Evidence: jest.config.ts
- **Convenient test scripts exist** (low): package.json includes workspace-wide test, project-specific tests, watch mode, and single-test execution. Evidence: package.json
- **Evidence of substantial domain tests** (medium): The inspected portfolio calculator spec includes many mocked services and a long test body, indicating serious effort around financial-calculation behavior. Evidence: apps/api/src/app/portfolio/calculator/roai/portfolio-calculator-cash.spec.ts

**Recommendations**
- Add or expose coverage thresholds in CI so test quality is enforced, not just present.
- Increase targeted tests for failure paths, cache failures, and exception propagation.
- Add a small number of higher-level integration tests around key API flows to complement mocked unit tests.

## Error Handling — 6.3/10

Error handling is present, especially through Nest validation and guarded startup behavior, but the overall pattern is inconsistent. Some areas handle expected failures carefully, while others appear to fail silently or rely on broad fallback behavior.

**Strengths**
- Global ValidationPipe with whitelist and forbidNonWhitelisted is a solid baseline for request validation.
- Feature flags and configuration defaults show defensive intent during startup.
- Some operational issues are actively tracked in the repository.

**Weaknesses**
- Silent failure paths reduce debuggability.
- Fallback behavior sometimes masks configuration issues instead of surfacing them clearly.
- Operational bugs in portfolio and cache paths suggest error handling is not consistently robust.

**Findings**
- **Strong API input validation baseline** (low): main.ts enables ValidationPipe with forbidNonWhitelisted, whitelist, and transform. Evidence: apps/api/src/main.ts
- **Fallbacks suppress malformed LOG_LEVELS input** (medium): main.ts parses LOG_LEVELS inside a try/catch and silently falls back without reporting invalid configuration. Evidence: apps/api/src/main.ts
- **Recent production bugs show fragile failure paths** (medium): Open issues report infinite loading states and calculation problems in portfolio flows, indicating some failure scenarios still escape clean handling. Evidence: issues #4299, #6229, #5006

**Recommendations**
- Log configuration parse failures and other fallback events at startup.
- Add explicit domain-level error mapping for cache, portfolio, and import flows.
- Expand automated tests for malformed inputs and downstream service failures.

## Exception Handling — 5.6/10

Exception handling is the weakest technical area in the visible evidence. There are multiple silent catch blocks and at least one documented Redis-related crash, which indicates that exception paths are not consistently surfaced or normalized.

**Strengths**
- NestJS provides a framework that can support clean exception handling patterns.
- Some recent bugs have been recognized and discussed publicly, which helps drive remediation.

**Weaknesses**
- Silent catch blocks swallow exceptions without logging or remediation.
- Exception behavior appears uneven across infrastructure and startup paths.
- Operational exception paths around Redis have already caused application crashes.

**Findings**
- **Silent catch in startup config parsing** (high): main.ts catches JSON parsing errors for LOG_LEVELS and does nothing with the exception. Evidence: apps/api/src/main.ts
- **Silent catch in Accept-Language processing** (high): AppModule catches parsing errors when reading Accept-Language for redirects and suppresses them entirely. Evidence: apps/api/src/app/app.module.ts
- **Redis exception path previously crashed the app** (high): Issue #4822 documents an application crash caused by an exception in @keyv/redis inside the Redis cache service. Evidence: issue #4822

**Recommendations**
- Eliminate empty catch blocks; at minimum log the exception and the fallback path used.
- Standardize exception translation for infrastructure services such as Redis, Prisma, and external data providers.
- Add regression tests specifically for previously reported exception bugs.

## Logging — 5.4/10

Ghostfolio has baseline logging through Nest and startup banners, but the visible code does not suggest deep observability. Logging appears functional rather than structured, and silent catches directly undermine diagnosis.

**Strengths**
- Startup logging is configurable through LOG_LEVELS and environment-dependent defaults.
- Nest logger integration gives a standard baseline.

**Weaknesses**
- Silent catch blocks erase important operational context.
- The inspected evidence does not show structured correlation, metrics, or tracing patterns.
- Recent bug reports still rely heavily on user-provided logs, suggesting observability could be stronger.

**Findings**
- **Logging is configurable but basic** (medium): main.ts derives log levels from configuration and falls back to a production/development default set. Evidence: apps/api/src/main.ts
- **Silent exception handling weakens logs** (medium): Empty catch blocks remove the very information logs should surface during config and request processing failures. Evidence: apps/api/src/main.ts, apps/api/src/app/app.module.ts
- **Observed issue workflow depends on raw logs** (low): Several bug reports cite manual server logs instead of richer operational diagnostics, which is normal but suggests limited observability tooling from the visible repo surface. Evidence: issues #4299, #5924, #5006

**Recommendations**
- Introduce structured logging around cache failures, external provider failures, and portfolio recalculation paths.
- Log suppressed fallback events with enough context to diagnose bad configuration and malformed headers.
- Consider adding request correlation IDs and a small observability guide for self-hosted operators.

## Aggregate — 6.6/10

Ghostfolio is a mature and substantial full-stack project with a strong modular foundation and credible automated testing, but it is being held back by oversized orchestration files, uneven exception handling, and modest observability. The core architecture is good; the main risk is operational maintainability at the edges.

**Strengths**
- Strong Nx full-stack architecture with clear separation of backend, frontend, and shared concerns.
- Real test infrastructure and evidence of substantial business-logic tests.
- Good baseline request validation and deployment guidance.

**Weaknesses**
- Exception handling quality is inconsistent and sometimes silent.
- Logging/observability is serviceable but thin.
- Complex feature areas have grown into oversized controllers/composition files.

**Findings**
- **Architecture is sound, but hotspots are too large** (high): Repo-level design is strong, yet controller/composition hotspots weaken maintainability and clarity. Evidence: apps/api/src/app/portfolio/portfolio.controller.ts, apps/api/src/app/app.module.ts
- **Exception handling needs hardening** (high): Silent catches and a documented Redis crash show a pattern that should be addressed before calling the system highly robust. Evidence: apps/api/src/main.ts, apps/api/src/app/app.module.ts, issue #4822
- **Testing is a real strength but should cover more failure behavior** (medium): There is meaningful unit-test infrastructure, but more regression tests for operational failures would materially improve confidence. Evidence: jest.config.ts, package.json, portfolio-calculator-cash.spec.ts

**Recommendations**
- Refactor oversized API hotspots into thinner controllers/services with narrower responsibilities.
- Remove empty catch blocks and standardize infrastructure exception handling.
- Strengthen structured logging and failure-path tests for cache, import, and portfolio-calculation flows.

