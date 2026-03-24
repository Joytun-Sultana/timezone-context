# Timezone Mapping Context

## Mapping Objective

Maintain a centralized, typed country-to-IANA timezone mapping used by the app for source and target timezone selection.

## Design Requirements

- Use valid IANA timezone IDs (for example `Europe/Oslo`, `Asia/Dhaka`).
- Mapping must be typed (`as const` + derived unions or explicit type aliases).
- Support countries with multiple timezones using array values.
- Keep mapping framework-agnostic and independent from UI code.

## Proposed Type Shape

- `CountryCode`: union of supported country keys.
- `IanaTimezone`: union/type for supported timezone identifiers.
- `CountryToIanaMap`: read-only record where each country maps to one or more IANA zones.

## Validation Expectations

- At runtime, timezone strings provided to conversion utilities are validated.
- Unsupported or malformed timezone IDs return typed errors.
- Mapping updates should include tests that verify keys and representative values are valid.

## Governance

- Add or modify country/timezone entries only in one config file.
- Keep naming consistent to avoid duplicate keys or alias drift.
- Document any intentionally excluded countries/zones when scope is limited.
