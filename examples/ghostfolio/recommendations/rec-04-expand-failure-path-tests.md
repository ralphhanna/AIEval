# Expand Failure-Path Tests

## Problem

Test infrastructure is present and credible, but the visible evidence suggests stronger coverage for core logic than for operational failure paths.

## Recommended Changes

1. Add targeted regression tests for:
   - Redis exceptions
   - invalid config values
   - malformed Accept-Language headers
   - failed external data provider calls
2. Pair every historical production bug fix with a regression test where possible.
3. Add a few integration-style tests around key API flows to complement mocked unit tests.

## Expected Impact

- fewer repeat production bugs
- stronger confidence in operational behavior
- better exception-handling quality
