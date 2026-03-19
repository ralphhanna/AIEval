# Refactor Large Hotspots

## Problem

Some high-traffic files have grown too large, especially the portfolio controller and the application composition root.

## Why It Matters

Large files:
- increase cognitive load
- make code review harder
- increase merge friction
- hide responsibility boundaries

## Specific Targets

- `apps/api/src/app/portfolio/portfolio.controller.ts`
- `apps/api/src/app/app.module.ts`

## Recommended Changes

1. Split portfolio endpoints by concern, for example:
   - portfolio overview
   - accounts
   - positions
   - analytics / calculations
2. Move orchestration logic out of controllers into dedicated application services.
3. Keep `AppModule` focused on composition, not inline request behavior.
4. Extract redirect and static-serving rules into dedicated middleware or setup helpers.

## Expected Impact

- simpler reasoning
- safer changes
- clearer ownership boundaries
- easier onboarding
