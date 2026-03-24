# UTC Strategy Context

## Principle

UTC is the single source of truth for all internal datetime representation.

## Boundary Rules

- Input boundary:
  - Convert user-provided source-local datetime + source timezone to UTC immediately.
  - Reject invalid local datetime/timezone combinations with typed errors.
- Internal boundary:
  - Store, compare, and pass UTC values only.
  - Avoid persisting or passing local timezone-specific datetimes between layers.
- Output boundary:
  - Convert from UTC to target timezone only when preparing display/output.

## Conversion Contract

- Reusable function: `convert(utc, timezone)`.
- Input:
  - UTC datetime representation (agreed format, typically ISO UTC string).
  - Target IANA timezone.
- Output:
  - Typed success payload with converted representation.
  - Typed failure for invalid UTC or invalid timezone.

## DST Handling

- DST is handled through standards-based timezone rules via `Intl` APIs.
- No manual offset arithmetic (`+/- hours`) for production conversion logic.
- Tests must include DST edge dates to confirm behavior.

## Failure Semantics

- Invalid UTC input -> explicit invalid UTC error.
- Invalid timezone input -> explicit invalid timezone error.
- Failures are non-throwing at UI boundaries when possible (typed result pattern preferred).
