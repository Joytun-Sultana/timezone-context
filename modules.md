# Modules Context

## Proposed Module Layout

- `src/config/timezones.ts`
  - Typed country -> IANA timezone mapping.
  - Exported types for country keys and timezone values.
- `src/utils/timezone.ts`
  - Reusable timezone conversion and validation utilities.
  - Public `convert(utc, timezone)` function contract.
- `src/domain/conversion.ts` (or equivalent domain service file)
  - Orchestrates boundary conversion to UTC and downstream display conversion.
  - Wraps utility calls with app-specific result types if needed.
- `src/composables/useTimezoneConverter.ts` (optional UI orchestration)
  - Handles form state and calls domain/service methods.
  - No timezone math beyond delegation.

## Module Responsibilities

- Config module:
  - Single source for timezone catalog.
  - Strong typing for compile-time safety.
- Utils/domain module:
  - Validate timezone IDs.
  - Validate UTC datetime input.
  - Convert UTC to target timezone output.
  - Convert source-local user input to UTC at boundary.
- UI/composable module:
  - Collect user selections.
  - Surface typed errors and successful outputs.

## Public Contracts (Planned)

- `convert(utcDatetime, targetTimezone) -> ConvertResult`
  - Input: UTC datetime string + target IANA timezone.
  - Output: typed success payload or typed failure payload.
- `toUtcFromLocal(localDatetime, sourceTimezone) -> UtcResult`
  - Input: local datetime + source IANA timezone.
  - Output: normalized UTC datetime or typed failure.

## Dependency Direction

- UI -> composable/domain -> utils/config.
- Config is dependency-free.
- Utils depends on standard APIs (`Intl`, `Date`) and shared types only.
- No reverse coupling from utils/domain into Vue components.
