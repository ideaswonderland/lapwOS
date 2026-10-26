# ADR-0002 — WORK_PAUSED Source and Reason

**Status:** ACCEPTED

## Context

lapwOS uses the `WORK_PAUSED` event to close an active Work Interval.

A pause may be triggered by an explicit user action or by a system event, such as the operating system entering Sleep or Hibernate.

These cases must remain distinguishable for reporting without introducing separate event types for each pause source.

## Decision

The `WORK_PAUSED` event supports two separate metadata fields:

- `source` identifies the actor or system layer that triggered the pause.
- `reason` identifies the specific reason for the pause.

The initial `source` values are:

- `user`
- `system`

The initial reporting requirement is to distinguish user-triggered pauses from system-triggered pauses using `source`.

The fields are initially defined for `WORK_PAUSED` only.

`source` is required. `reason` must contain a defined value appropriate to the event; the application must not guess an unknown reason.

Validation must enforce allowed values and consistency between `source` and `reason`.

The metadata does not change Work Interval start/end rules or duration calculations.

## Examples

User-triggered pause:

```json
{
  "event_type": "WORK_PAUSED",
  "source": "user",
  "reason": "manual_pause"
}
```

System-triggered pause due to Sleep:

```json
{
  "event_type": "WORK_PAUSED",
  "source": "system",
  "reason": "sleep"
}
```

These examples illustrate the intended structure. The complete allowed `reason` vocabulary must be defined before implementation.

## Consequences

- A single `WORK_PAUSED` event type is retained.
- Reports can distinguish user-triggered and system-triggered pauses.
- More detailed reporting by reason remains possible.
- Event validation must reject missing or invalid metadata.
- Existing Work Interval derivation rules remain unchanged.

## Related Requirements

- `REQ-193` — Sleep / Hibernate During Work
- `REQ-207` — Work Interval Rules
