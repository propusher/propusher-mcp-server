---
name: build-program
description: Use when creating or editing a multi-week program in the coach's Propusher library, a day grid of up to 8 weeks with workouts, questionnaires and outside activities. Covers reusing workouts by id or defining them in place, day numbering, and update_program replacing the grid.
---

# Build a program

A program is a day grid of up to 8 weeks: `day` 0 is week 1 day 1, 7 is week 2
day 1, 55 the last. Several items can share a day.

## Look up first

`list_programs` (`mine`, then `library`) for something to start from.
`list_workouts` for workouts to reuse, `list_exercises` for new ones,
`list_questionnaires` for check-ins. Never invent ids; an unknown id is
rejected and nothing is written.

## Create

`create_program`: `title`, `description`, `workouts`, and optionally
`questionnaires` and `activities`.

- `workouts`: `{ "day": 0, "workoutId": "<_id>" }` reuses a workout (own or
  library), or `{ "day": 2, "title": "...", "description": "...", "duration": 3600, "classification": { ... }, "exercises": [ ... ] }`
  defines one in place, its exercises written as in the build-workout skill.
  Workouts are copied into the program as snapshots; nothing is added to the
  coach's workout library.
- `questionnaires`: `{ "day": 6, "questionnaireId": "<_id>", "time": "08:00" }`.
- `activities`: `{ "day": 3, "activity": "<from the schema>", "time": "18:00", "duration": 2700, "notes": "easy 5k" }`
  for what the client does outside Propusher (a run, a swim, a match);
  `duration` in seconds, 3600 by default.

Times are `HH:MM`, 12:00 by default.

## Edit

`get_program` first. `workouts` **replaces the whole grid**, so send every slot
you want. `questionnaires` replaces only the questionnaires and `activities`
only the activities; leave either out to keep it, send `[]` to clear it.

A new program is live straight away; an edit is a draft the coach applies on
the program's page (the `url` in the answer). You cannot assign a program to a
client or team; the coach does that in Propusher.
