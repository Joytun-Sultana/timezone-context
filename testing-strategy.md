# Testing Strategy Context

## Testing Goals

- Prove UTC-first behavior across app boundaries.
- Verify reliable conversion across multiple timezones and DST boundaries.
- Ensure invalid inputs are handled with typed, predictable failures.
- Cover both utility-level logic and user-facing flows.

## Unit Test Scope (Vitest)

Primary target: timezone utilities/domain conversion functions.

Required cases:

- Multi-timezone conversions using fixed UTC timestamps.
- DST transition behavior (spring forward and fall back) for at least one DST-observing timezone.
- Invalid UTC datetime input handling.
- Invalid/unsupported timezone identifier handling.
- Source-local to UTC boundary conversion behavior.

Determinism rules:

- Use fixed test timestamps (no `Date.now()`).
- Assert explicit expected outputs.
- Never rely on machine local timezone.

## Playwright E2E Scope

Cover end-to-end user flow:

1. User selects source timezone, datetime, and target timezone.
2. App normalizes input to UTC internally.
3. App displays target timezone conversion output.

Required E2E scenarios:

- Happy path conversion across distant zones (for example Asia <-> Americas).
- DST-sensitive conversion date coverage.
- Invalid timezone/input handling UI feedback.

## Quality Gate

Before feature completion:

- `npm run lint` passes.
- Formatting checks pass (project Prettier command).
- Unit tests pass.
- Playwright tests pass.
