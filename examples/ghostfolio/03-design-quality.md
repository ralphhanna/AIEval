# 03-design-quality — design_quality

**Score:** 7.8/10

## Summary
The overall architecture is strong: Ghostfolio uses a well-organized Nx workspace, a NestJS backend, Angular frontend, Prisma, Redis, and modular feature packaging. The main design weakness is that some modules and controllers have grown too large at the edges.

## Strengths
- The repo is explicitly organized as an Nx workspace with separate backend and frontend concerns.
- Modular NestJS design is visible through many feature modules and service modules.
- Shared infrastructure like Prisma, configuration, queues, and caching is separated into distinct modules.

## Weaknesses
- Some edge modules appear to absorb too many concerns over time.
- Conditional composition in AppModule makes the composition root heavy.
- Large files indicate erosion of single-responsibility boundaries in high-traffic areas.

## Findings
- **Strong workspace structure** (low): The repo exposes apps, libs, prisma, docker, test/import, and tools directories, which is a healthy split for a full-stack Nx application. Evidence: repo root
- **AppModule is a heavy composition root** (medium): The root module imports many application modules and also embeds conditional queue UI configuration, Redis settings, static file rules, and redirect logic. Evidence: apps/api/src/app/app.module.ts
- **Architecture is modular but not consistently thin** (medium): The existence of very large controller files suggests that module boundaries exist but are not always enforced inside features. Evidence: apps/api/src/app/portfolio/portfolio.controller.ts

## Recommendations
- Refactor oversized feature controllers/services behind thinner endpoint handlers and dedicated application services.
- Keep AppModule focused on composition and move complex static-serving and request-routing behavior into isolated components.
- Add lightweight architecture notes for the main backend domains and shared libraries.

## Confidence
high
