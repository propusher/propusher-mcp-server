---
name: build-exercise
description: Use when creating or editing an exercise's details in the coach's Propusher library (title, description, muscle groups, equipment). Covers finding an existing exercise first, create_exercise and update_exercise, and reading the exercise's 3D animation for its equipment and the muscles that actually work. For writing the animation itself, use the animate-exercise skill.
---

# Build an exercise

## Find one first

`list_exercises` with `scope: "mine"`, then `scope: "library"`. `search`
matches every word in the title or description, so search one or two words
("squat", not "paused back squat"); `group` and `equipment` narrow it.
Library exercises can be used in workouts as they are. Create one only when
nothing fits, and say you did.

## The details

- `title`: the name coaches and clients know it by.
- `description`: how to do it, in a few plain lines the client reads.
- `equipment`: what the movement needs, from the values in the tool schema.
- `groupList`: the muscle groups it trains, from the values in the tool schema
  (`quads`, `glutes`, `upper-back`, ...). Prefer the specific groups; use
  `upper-body`, `lower-body` or `full body` only when the work really is that
  broad.

Sending `groupList` or `equipment` replaces the stored list, so send the whole
list you want.

## Equipment from the animation

When the exercise has an animation, check its objects first: `get_exercise`
returns them as `animation.equipment` beside the frames. Map them to the
`equipment` values:

- `barbell`: `barbell`.
- `dumbbell_left` and/or `dumbbell_right`: `dumbbell` (one or two, the same
  value).
- a `box`: read it from its `size` and what the figure does with it. One the
  figure lies or sits on (long and about knee height) is a `bench`; one it
  steps or jumps onto is `box-solid` (or `box-soft` when the coach says so).

Then add what the animation cannot show from the title and description:
the animation has only bars, dumbbells and boxes, so kettlebells, cables,
bands, machines, a pull-up bar or a mat come from the name and text. If the
objects and the name disagree (a "dumbbell press" animated with a barbell),
tell the coach. Without an animation, take the equipment from the title and
description alone.

## Muscle groups from the animation

When the exercise has an animation, base `groupList` on what the figure
actually does, not only on the name. `get_exercise` returns it as `animation`
key poses: per frame, the `joints` that differ from standing and the `root`
(pelvis) position. Read as much out of it as you can:

1. **What moves.** Compare the frames: a joint whose pose changes between them
   is doing the work; one that stays put is at most holding position.
2. **Which way, against what.** Take the direction from the figure's axes
   (+Y up, +Z the way it faces) and the load from `equipment` (a bar or
   dumbbells in the hands) and gravity:
   - knees bending and straightening with the pelvis dropping and rising:
     `quads`, `glutes`;
   - hips hinging with the knees nearly straight and the torso tipping
     forward: `hamstrings`, `glutes`, `lower-back`;
   - hip bending to lift a knee or leg: `hip flexors`;
   - ankles pointing the foot to rise onto the toes: `calves`;
   - arms pressing away from the chest: `chest`, `shoulders`, `triceps`;
     pressing overhead: `shoulders`, `triceps`;
   - arms pulling towards the body or down from overhead: `upper-back`,
     `biceps`;
   - elbows alone bending under load: `biceps`; straightening: `triceps`;
   - spine joints (`vl2` to `vl5`) curling forward: `abs`; arching back:
     `lower-back`; a long still torso under load or on the floor: `core`.
3. **Check it against the name.** If the animation and the title disagree (a
   "row" that presses), say so to the coach instead of picking one quietly.

An animation shows motion, not effort: it cannot tell a slow eccentric from a
fast one or which of two muscles leads, and a hold (plank, wall sit) shows no
movement at all. Fill those gaps from the title and description. Without an
animation, tag from the title and description alone.

When you write or change an exercise's animation (the animate-exercise skill),
check its `equipment` and `groupList` against the new animation the same way.

## Create

`create_exercise`: `title`, optional `description`, `groupList`, `equipment`.
It is saved live straight away and starts without an animation.

## Edit

Only the coach's own exercises can be edited; a library one is read-only, so
create the coach's own version instead. `get_exercise` first, then
`update_exercise` with the `id` and only the fields that change. The edit is a
draft the coach applies on the exercise's page (the `url` in the answer).
`update_exercise` leaves the animation alone.
