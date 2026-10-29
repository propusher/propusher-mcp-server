---
name: propusher
description: Use when working with Propusher through its MCP tools, or when a coach asks about their Propusher library (exercises, workouts, programs, questionnaires). Covers connecting, how the tools fit together, which writes are drafts, and good practice. For building one kind of item, the build-exercise, animate-exercise, build-workout, build-program and build-questionnaire skills have the details.
---

# Propusher

Propusher is a web app coaches use to plan and deliver training: an exercise
library with 3D animations, workout templates, multi-week programs and
questionnaires, which the coach then assigns to clients and teams. The MCP
server at `https://app.propusher.com/mcp` gives you the coach's library. It does
not reach clients, teams or calendars.

## Connecting

- Sign-in is OAuth: the first tool call opens a browser, the coach logs in to
  Propusher and approves the connection. Nothing to configure, no API key.
- The coach can see and revoke connected apps from their account page.

## The tools

Every list tool takes `scope`: `"mine"` (the coach's own) or `"library"`
(Propusher's shared library, read-only), plus `search` (every word must appear).
Lists return summaries; the `get_` tool for an id returns the full record.

| Kind | Read | Write |
| --- | --- | --- |
| Exercises | `list_exercises` (group, equipment filters), `get_exercise` | `create_exercise`, `update_exercise` |
| Animations | `preview_animation` | `set_exercise_animation` |
| Workouts | `list_workouts` (focus, bodyFocus, context, phase filters), `get_workout` | `create_workout`, `update_workout` |
| Programs | `list_programs`, `get_program` | `create_program`, `update_program` |
| Questionnaires | `list_questionnaires`, `get_questionnaire` | `create_questionnaire`, `update_questionnaire` |

There are no delete tools; the coach deletes in the app.

How they build on each other:

1. **Exercises** are the base. A workout entry points at one by `exerciseId`,
   own or library. Library exercises can be used as they are; only the coach's
   own can be edited or animated.
2. **Workouts** are ordered exercises, each with its prescription (sets, reps,
   weight, a per-set `setPlan`, rest, coaching notes) and a classification.
3. **Programs** are a day grid of up to 8 weeks (day 0 to 55) of workout
   slots, either reusing a workout by `workoutId` or defining one in place, plus
   scheduled questionnaires and activities. Workouts are copied into the program
   as snapshots.
4. **Questionnaires** are forms clients fill in (check-ins, intake, RPE). They
   can be scheduled inside programs.

How to build each kind, with the shapes the write tools take: the
**build-exercise**, **animate-exercise**, **build-workout**,
**build-program** and **build-questionnaire** skills.

## Drafts: what you edit waits for the coach

- **Edits are drafts.** `update_*` and `set_exercise_animation` hold the change
  beside the item, marked as suggested by AI. Clients keep seeing the published
  version until the coach applies it in Propusher; a second edit adds to the
  same draft. `get_*` returns the published values plus `draft.changes`.
- **Creates are live.** A new item from `create_*` is saved straight away,
  never as a draft. Do not call it a draft.
- **`live: true`** writes an edit straight to the published item. Send it only
  when the coach explicitly asks for the change to go live now.
- **`publish_draft(type, id)`** applies an item's pending draft, as Apply in
  the editor does. Only when the coach asks. Nothing deletes items or discards
  drafts.
- Every write answers `_id`, `url` (a path in the app: open
  `https://app.propusher.com` + `url`) and, for edits, `draft`. **Tell the coach
  what you made or changed and give them that link.**

## Good practice

- **Look up before you create.** Search `mine`, then `library`, for an exercise,
  workout, program or questionnaire that already does the job. Reuse library
  exercises by id instead of creating copies, and extend an existing workout
  rather than making a near-duplicate. Create an exercise only when nothing fits.
- **Never invent ids.** Take them from list results; an unknown id is rejected
  and nothing is written.
- **Read before you edit.** `get_` the item first: the edits that take a list
  replace it whole.
- **Classify workouts** as you write them; the closed vocabularies are in the
  tool schemas.
- **Say where to review.** End with the item names, which edits are drafts, and
  their links.
