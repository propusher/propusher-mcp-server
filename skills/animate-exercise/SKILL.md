---
name: animate-exercise
description: Use when writing or fixing a Propusher exercise's 3D animation, the looping figure that is also its thumbnail. Covers describe_animation_format, starting from a similar exercise's frames, checking poses with preview_animation, and saving with set_exercise_animation.
---

# Animate an exercise

1. `describe_animation_format` once, before your first preview or save: axes,
   the 23 joints, the frame format, equipment, worked examples.
2. Start from something that looks right. `get_exercise` on a similar library
   or own exercise returns its animation as frames in exactly the shape you
   write. Copy and adjust rather than guessing numbers.
3. `preview_animation(frames, equipment)` renders up to 6 frames as images
   without saving. Look at each one and fix what is wrong (a limb the wrong way
   is usually a sign flip on one axis; check the feet), then preview again.
   Two to four frames are enough for most lifts; do not repeat the first pose
   at the end. If it says no renderer is configured, save anyway and ask the
   coach to check it in the app.
4. `set_exercise_animation(id, frames, equipment)` on the coach's own exercise
   replaces its animation. To animate a library exercise, `create_exercise` an
   own one first (see the build-exercise skill).
5. Check the exercise's equipment and muscle groups against the new
   animation, as the build-exercise skill describes, and update them if they
   no longer match.

The new animation is a draft: clients, pickers and the library keep the
published one until the coach applies it on the exercise's page, where it plays
with its own draft thumbnail. Tell them to look at it there.
