# Architecture Context
## Stack And Boundaries
- Framework: Vue 3 + TypeScript + Vite.
- Timezone business logic stays outside Vue UI components.
- UI layer captures user inputs and delegates all datetime logic to domain/utils.
- Domain/utils layer owns parsing, validation, UTC normalization, and conversion.
- Config layer owns typed country-to-IANA timezone mapping.
- Test layer includes unit tests for time logic and Playwright tests for user flows.
## Core User Flow
1. User selects:
   - source timezone
   - local datetime
   - target timezone
2. Input boundary converts source-local datetime to UTC immediately.
3. Internal state, data passing, and comparisons use UTC only.
4. Output boundary calls reusable `convert(utc, timezone)` for display/result rendering.
## Logical Components
- `TimezoneInputAdapter` (boundary):
  - Validates source timezone and input datetime shape.
  - Converts local source datetime into UTC representation.
- `TimezoneConversionService` (domain):
  - Exposes reusable `convert(utc, timezone)` function.
  - Returns typed success/failure results.
- `TimezoneCatalog` (config):
  - Provides typed country -> IANA timezone mapping.
- `ConversionViewModel` (UI-facing orchestration):
  - Coordinates user selections and calls domain functions.
  - Contains no timezone math.
## Error Handling Strategy
- Invalid timezone identifier -> typed invalid-timezone error.
- Invalid UTC datetime input -> typed invalid-utc-datetime error.
- Errors are explicit and safe for UI display (no uncaught runtime formatting errors).
## Non-Functional Constraints
- Use `Intl.DateTimeFormat` for timezone-aware rendering/conversion output.
- Avoid manual offset arithmetic.
- Keep behavior deterministic with fixed timestamps in tests.
