---
name: animate-exercise
description: Use when writing or fixing a Propusher exercise's 3D animation, the looping figure that is also its thumbnail. Covers the animation format (scene axes, the 23 joints, frames, equipment, worked examples), starting from a similar exercise's frames, checking poses with preview_animation, and saving with set_exercise_animation.
---

# Animate an exercise

An exercise can carry a looping 3D animation of a human figure (about 1.75 m
tall). It plays wherever the exercise appears in the app and on the client's
phone, and its first frame becomes the exercise's thumbnail. You build one by
describing a few key poses; the frames in between are interpolated.

## How to work

1. Read the format below before your first preview or save.
2. Start from something that looks right. `get_exercise` on a similar library
   or own exercise returns its animation as frames in exactly this format.
   Copy and adjust rather than guessing numbers.
3. `preview_animation(frames, equipment)` renders up to 6 frames as images
   without saving. Look at each one and fix what is wrong (a limb the wrong way
   is almost always a sign flip on one axis; check the feet), then preview
   again. The camera is fixed on the middle of the body, so a full overhead
   reach can run off the top of the picture: that is the camera, not your pose.
   If it says no renderer is configured, save anyway and ask the coach to check
   it in the app.
4. `set_exercise_animation(id, frames, equipment)` on the coach's own exercise
   replaces its animation. To animate a library exercise, `create_exercise` an
   own one first (see the build-exercise skill).
5. Check the exercise's equipment and muscle groups against the new
   animation, as the build-exercise skill describes, and update them if they
   no longer match.

The new animation is a draft: clients, pickers and the library keep the
published one until the coach applies it on the exercise's page, where it plays
with its own draft thumbnail. Tell them to look at it there.

## Scene

The figure starts standing at the origin, facing +Z.

- +X is the figure's LEFT, +Y is up, +Z is the way it faces, the floor is y 0.
- Distances are metres.
- Every direction is a direction in this scene, fixed to the room, not
  relative to the body: "point the shin at [0, -1, 0]" means straight down at
  the floor whatever the thigh is doing.

## Joints (23)

You pose a joint by saying where its own segment points.

| Joint | Segment |
| --- | --- |
| `hip_L`, `hip_R` | the thigh (hip to knee) |
| `knee_L`, `knee_R` | the shin (knee to ankle) |
| `ankle_L`, `ankle_R` | the foot (ankle to ball of foot) |
| `midfoot_L`, `midfoot_R` | the toes |
| `vl2`, `vl3`, `vl4`, `vl5` | the spine, lower to upper (four segments) |
| `vl6`, `vl7` | the neck, then the head |
| `shoulder_L`, `shoulder_R` | the upper arm (shoulder to elbow) |
| `elbow_L`, `elbow_R` | the forearm (elbow to wrist) |
| `wrist_L`, `wrist_R` | the hand |
| `knuckles_L`, `knuckles_R` | the fingers |
| `pelvis` | not posed in `joints`: it is the root, see below |

Each joint hangs off the one before it in its chain, so posing a hip swings the
whole leg; you then say separately where the shin points.

## A frame

```json
{
  "durationMs": 700,
  "joints": {
    "hip_L":  { "aim": [0, -0.3, 0.95] },
    "knee_L": { "aim": [0, -1, -0.2] },
    "elbow_L": { "aim": [0, 0.7, 0.7], "roll": 45 }
  },
  "root": { "pos": [0, 0.9, 0], "euler": [0, 90, 0] },
  "autoGround": true
}
```

- `durationMs`: time until the next frame. The last frame loops back to the
  first, so do not repeat the opening pose at the end. Default 800, range 50 to
  20000.
- `aim`: `[x, y, z]` direction, any length (it is normalised). A joint you
  leave out keeps its standing direction, so a frame of `{}` is the standing
  figure.
- `roll`: degrees of twist about the segment's own aim: forearm pronation,
  shoulder rotation, a turned foot.
- `euler`: `[x, y, z]` degrees about the scene axes, applied X then Y then Z.
  On any joint for what aim cannot say (a spine twist), and on `root` to turn
  the whole figure (`[0, 90, 0]` makes it face +X, its own left).
- `root.pos`: where the pelvis sits, in metres. Standing is `[0, 1.12, 0]`.
- `autoGround` (default true): moves the whole figure so its lowest joint ends
  up at the height it has when standing, so a squat sits on the floor instead
  of sinking through it and a plank rests on its hands. Giving `root.pos` turns
  it off for that frame; do that for jumps, hangs and anything lying down.

## An animation

`{ "frames": [ ... ], "equipment": [ ... ] }`, 2 to 60 frames. Two to four
frames cover most lifts: the start, the end, and sometimes a mid-point. More
frames mostly cost realism rather than add it.

## Equipment (optional)

- `"barbell"`: held in both hands.
- `"dumbbell_left"`, `"dumbbell_right"`: held in that hand.
- `{ "type": "box", "position": [x, y, z], "size": [w, h, d] }`: a bench, step
  or plyo box.

Bars and dumbbells attach themselves to the wrists every frame: never position
them or pose the hands around them. A box is placed by its minimum corner, so a
1.2 x 0.45 x 0.5 m bench under the figure is
`{ "type": "box", "position": [-0.6, 0, -0.25], "size": [1.2, 0.45, 0.5] }`.
Wall bars and elastic bands exist in the app's own editor but not here.

## Example: bodyweight squat, 2 frames

```json
{
  "frames": [
    { "durationMs": 900, "joints": {
        "shoulder_L": { "aim": [0.15, -0.1, 0.98] }, "shoulder_R": { "aim": [-0.15, -0.1, 0.98] },
        "elbow_L": { "aim": [0.15, -0.1, 0.98] },    "elbow_R": { "aim": [-0.15, -0.1, 0.98] }
    } },
    { "durationMs": 900, "joints": {
        "hip_L": { "aim": [0.15, -0.25, 0.96] },  "hip_R": { "aim": [-0.15, -0.25, 0.96] },
        "knee_L": { "aim": [0, -0.95, -0.3] },    "knee_R": { "aim": [0, -0.95, -0.3] },
        "ankle_L": { "aim": [0, -0.8, 0.6] },     "ankle_R": { "aim": [0, -0.8, 0.6] },
        "vl2": { "aim": [0, 0.87, 0.5] }, "vl3": { "aim": [0, 0.87, 0.5] },
        "vl4": { "aim": [0, 0.87, 0.5] }, "vl5": { "aim": [0, 0.87, 0.5] },
        "shoulder_L": { "aim": [0.15, -0.1, 0.98] }, "shoulder_R": { "aim": [-0.15, -0.1, 0.98] },
        "elbow_L": { "aim": [0.15, -0.1, 0.98] },    "elbow_R": { "aim": [-0.15, -0.1, 0.98] }
    } }
  ]
}
```

The feet keep their standing direction, so they stay flat on the floor while
the shins tip back. Feet are the usual thing people forget.

## Example: dumbbell biceps curl, 2 frames

```json
{
  "frames": [
    { "durationMs": 700, "joints": {} },
    { "durationMs": 700, "joints": {
        "elbow_L": { "aim": [0.1, 0.75, 0.65] }, "elbow_R": { "aim": [-0.1, 0.75, 0.65] },
        "wrist_L": { "aim": [0.1, 0.75, 0.65] }, "wrist_R": { "aim": [-0.1, 0.75, 0.65] }
    } }
  ],
  "equipment": ["dumbbell_left", "dumbbell_right"]
}
```

The arms hang at the sides in the first frame because an empty pose is the
standing figure; only the forearms move.
