---
name: build-questionnaire
description: Use when creating or editing a questionnaire in the coach's Propusher library (check-in, intake, readiness, RPE or wellness form). Covers question types, scale labels and colors, and update_questionnaire replacing the question list.
---

# Build a questionnaire

## Look up first

`list_questionnaires` (`mine`, then `library`); reuse or adapt one that fits.

## Create

`create_questionnaire`: `title` and `inputs`, the questions in order, each
`{ "type": ..., "prompt": ... }`:

- `scale`: `min` and `max` (1 to 10 by default), optional `scaleLabels`
  `[{ "value": 1, "label": "Fresh", "color": "green" }]`. Colors run red,
  orange, amber, lime, green from worst to best; zinc is neutral. A
  lower-is-better scale (fatigue, soreness, stress) puts green at the low end.
- `select`: `options`, `multiple` for more than one choice.
- `text`, `number`, `time` (`HH:MM`), `duration`, `bodymap` (pick muscle
  groups).

## Edit

`get_questionnaire` first. `inputs` **replaces the list**: echo each kept
question with its `_id` so it keeps its identity instead of being re-created;
leave `_id` out only for a new question.

A new questionnaire is live straight away; an edit is a draft the coach applies
on its page (the `url` in the answer). To send it to clients regularly, put it
in a program (see the build-program skill).
