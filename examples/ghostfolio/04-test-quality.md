# 04-test-quality — test_quality

**Score:** 7.2/10

## Summary
Ghostfolio has meaningful automated test infrastructure and evidence of substantial API-domain testing. The main concern is not absence of tests, but uneven confidence caused by concentration around selected areas and lack of visible coverage reporting in the inspected material.

## Strengths
- Nx/Jest test orchestration is set up cleanly across the workspace.
- The repo exposes dedicated test scripts for all projects and targeted test execution.
- At least one inspected portfolio calculator spec is substantial and uses realistic dependency mocking.

## Weaknesses
- The inspected evidence does not show explicit coverage thresholds or coverage enforcement.
- Large, heavily mocked tests can still leave integration gaps.
- Visible test strength is stronger in some backend calculation areas than in observability or failure-path validation.

## Findings
- **Workspace-wide Jest integration exists** (low): Jest is configured through Nx, and the root jest config delegates to getJestProjectsAsync(). Evidence: jest.config.ts
- **Convenient test scripts exist** (low): package.json includes workspace-wide test, project-specific tests, watch mode, and single-test execution. Evidence: package.json
- **Evidence of substantial domain tests** (medium): The inspected portfolio calculator spec includes many mocked services and a long test body, indicating serious effort around financial-calculation behavior. Evidence: apps/api/src/app/portfolio/calculator/roai/portfolio-calculator-cash.spec.ts

## Recommendations
- Add or expose coverage thresholds in CI so test quality is enforced, not just present.
- Increase targeted tests for failure paths, cache failures, and exception propagation.
- Add a small number of higher-level integration tests around key API flows to complement mocked unit tests.

## Confidence
medium
