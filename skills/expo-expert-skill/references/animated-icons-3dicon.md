# Animated 3D Icons — 3dicon in Expo

The full skill is `skills/3dicon/SKILL.md`. This file is the
one-page Expo summary. It links there; it does not repeat the
whole pipeline.

## What it is

3dicon turns one prompt into a looping animated 3D icon with
real transparency, as an animated WebP. The same still is sent
as the first and last frame, so the loop closes with no seam.
The background is removed against a known colour, so soft edges
stay soft instead of leaving a halo.

## Use in Expo

- Animated WebP renders natively in `expo-image`. No Lottie.
  No new native dependency.
- Put output in `assets/icons/`. Ship the still alongside the
  WebP: `timer.webp` and `still.png`.
- Swap to the still when reduced motion is on. Animated WebP
  cannot honour reduced motion on its own. Use `useReducedMotion`
  from `react-native-reanimated` to choose.

```tsx
import { Image } from "expo-image";
import { useReducedMotion } from "react-native-reanimated";

const reduced = useReducedMotion();
<Image
  source={reduced ? require("../assets/icons/timer-still.png")
                  : require("../assets/icons/timer.webp")}
  style={{ width: 96, height: 96 }}
/>
```

## Costs and sizes

- Still: about 13 cents. Full run: about 48 cents, about 4 minutes.
- File size scales with frame count. The worked example is
  122 frames at 24fps, 288px, 857 KB.
- Encode at the source frame rate. Never sample down to 8fps to
  save size. The timer example shipped at 8fps once and looked
  broken; the frame rate was the reason.

## Hard rules

- Show ONE still and get approval before animating. Then propose
  ONE motion and get approval before running it.
- Never stack a code spring or drift on top of a baked loop.
  Pick one. A baked loop plus a spring reads as fidgeting.
- The soft contact shadow does not survive matting. Check the
  icon on light surfaces before shipping. On a dark surface the
  loss is invisible; on a light surface the animated icon floats.
- The last frame is set to the first frame. Do not remove that
  and try to cross-fade instead.

## Motion choices

- Ask what the object does when left alone. That picks the
  strategy: `native` (it moves by itself), `event` (it performs
  its function once, then rests), `part` (one small piece moves),
  `surface` (light or material travels across a fixed form).
- Most objects are inert. Reach for `event` first.
- Add `--emit` when the action would throw something off: a
  fragment, a droplet, a spark.
- Small and many beats large and one. Motion spread over the
  object's details cannot leave the frame.

## Checklist

- [ ] Still approved before any animation spend.
- [ ] One motion proposed and approved.
- [ ] Still shipped alongside the WebP for reduced motion.
- [ ] Checked on a light surface.
- [ ] No code animation stacked on the loop.

## Worked example: the timer

Prompt: "a 3D stopwatch icon in sage green and cream, soft matte
plastic, rounded friendly proportions, small feet and a top button".
Still approved first. Motion: "the stopwatch rocks gently side to
side while its hand sweeps steadily clockwise". Strategy `event`,
energy `lively`, `--emit` off — a stopwatch throws nothing off.

Result: 122 frames at 24fps, 288px, 857 KB. Verify reported a mean
step of 7.25 and 110 of 121 frames moving, with a loop seam of 2.04
— well under a normal frame step, so the loop closes cleanly.

The first version shipped at 41 frames, 8fps, to save size. It
looked broken on device. The fix was the frame rate, not the art.

## Common mistakes

- Animating before the still is approved. The still costs 13 cents.
  The full run costs 48 cents and four minutes, and every later
  stage inherits the still's flaws.
- Sampling down to 8fps to hit a size target. Judder looks like a
  bad render, bad matting, or a bad player. It is none of those.
  It is the frame rate.
- Stacking a code spring on a baked loop. The icon fidgets.
- Shipping the loop without the still. Reduced-motion users get
  motion they asked not to see.
- Checking the icon only on a dark surface. The soft contact
  shadow is gone after matting, and on a light surface the icon
  floats. Check both.
