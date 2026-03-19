# 02-simplicity — simplicity

**Score:** 6.8/10

## Summary
Ghostfolio is understandable at the repo level, but several core API paths concentrate too much behavior in single files and heavy bootstrap/module composition makes local reasoning harder than it should be.

## Strengths
- Clear top-level workspace organization with apps, libs, prisma, docker, and tools.
- Consistent Nx task scripts make common workflows predictable.
- Use of Nest modules and Angular/Nx conventions reduces accidental complexity in many areas.

## Weaknesses
- Very large orchestration points reduce readability and make changes riskier.
- Startup and static-serving logic mixes configuration, security, localization, and redirect behavior.
- Feature-rich monorepo setup increases the amount of context needed for small changes.

## Findings
- **Oversized portfolio controller** (medium): The portfolio controller is 674 lines / 618 LOC, which is too much surface area for a single controller and suggests multiple responsibilities. Evidence: apps/api/src/app/portfolio/portfolio.controller.ts
- **Dense root module composition** (medium): AppModule wires a long list of modules plus conditional Bull Board, Redis, and static-server configuration in one file, making startup behavior harder to reason about. Evidence: apps/api/src/app/app.module.ts
- **Nx monorepo scripts are broad but coherent** (low): The package.json scripts are extensive, but they stay aligned around Nx targets for build, lint, test, serve, and workspace management. Evidence: package.json

## Recommendations
- Split portfolio.controller.ts into narrower controllers or endpoint-focused modules.
- Move static redirect, Accept-Language handling, and feature-flag setup into dedicated providers or middleware.
- Document the intended ownership boundaries between apps/api, apps/client, libs, and services more explicitly.

## Confidence
medium
