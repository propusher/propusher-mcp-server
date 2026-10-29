---
name: build-workout
description: Use when creating or editing a workout template in the coach's Propusher library, including a reusable block such as a warm-up or finisher. Covers reusing exercises and workouts, the per-exercise prescription (sets, reps, weight, per-set setPlan, rest), classification, and update_workout replacing the exercise list.
---

# Build a workout

## Look up first

1. **Exercises.** `list_exercises` (`mine`, then `library`), searching one or
   two words of the name. Reuse library exercises by id. Only when nothing
   fits, create one (see the build-exercise skill).
2. **Workouts.** `list_workouts` (`mine`, then `library`) with `search` or the
   `focus`, `bodyFocus`, `context`, `phase` filters. If one is close, offer to
   change it or start from it instead of making a near-duplicate.

Never invent ids; an unknown id is rejected and nothing is written.

## Create

`create_workout`: `title`, `exercises` in order (1 to 100), optional
`description`, `duration` (seconds), `block: true` for a reusable chunk such as
a warm-up or finisher, and `classification`: `type` (main training goal),
`secondary`, `bodyFocus`, `context` (its place in the week or setting),
`phase` (point in the competitive year). Leave out what does not apply; the
values are in the tool schema.

Each exercise entry:

```json
{ "exerciseId": "<_id from list_exercises>", "sets": 4, "reps": 8, "weight": 60, "setRest": 90 }
{ "exerciseId": "<_id>", "setPlan": [{ "reps": 10, "weight": 40 }, { "reps": 8, "weight": 50 }, { "reps": 6, "weight": 60, "rest": 120 }] }
```

- `exerciseId`: own or library. Its defaults fill what you leave out.
- Same sets throughout: `type` (`reps`, `duration`, `distance`; defaults to the
  exercise's own), `sets`, `reps`, `weight` (kg), `duration` (seconds),
  `distance` (metres), `setRest` (seconds), `intensity` with `intensityUnit`
  (`percent` of 1RM or `bpm`).
- Sets that differ (pyramid, ramp, drop set): `setPlan`, one object per set
  (`reps`, `duration`, `distance`, `weight`, `intensity`, `rest`). It wins over
  the flat fields.
- `name` overrides the title in this slot; `description` is the coaching note
  the client reads under it.

## Edit

`get_workout` first. Only what you send changes, but `exercises` **replaces the
whole list**: send every entry to keep as `{ "id": "<entry id from get_workout>" }`
in order, with only the changed fields beside it, and `{ "exerciseId": ... }`
for a new one. Left out = removed. When `get_workout` shows
`draft.changes.exercises`, start from that list, not the published one.
`classification` is replaced whole too (`{}` clears it).

A new workout is live straight away; an edit is a draft the coach applies on
the workout's page (the `url` in the answer). Library workouts are read-only:
build the coach's own version instead.
