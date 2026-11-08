# 3dicon in Expo — Usage

The full skill is `SKILL.md` in this folder. This note covers
only the Expo side: how to use what 3dicon makes.

## Output

- Animated WebP works natively in `expo-image`. No Lottie.
  No new native dependency.
- Place output in `assets/icons/`. Ship the still alongside
  the loop: `timer.webp` plus `still.png`.

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

## Reduced motion

Animated WebP cannot honour reduced motion on its own. Always
swap to the still when `useReducedMotion` is true. This is not
optional.

## Size and frame rate

- File size scales with frame count. Example: 122 frames at
  24fps, 288px, is 857 KB.
- Encode at the source frame rate. Never sample down to 8fps
  to save size. It produces judder that looks like a bad render.
- Resolution is the quality lever, not the WebP quality setting.
  Raise `--size` before touching `--quality`.

## Costs

- Still: about 13 cents. Full run: about 48 cents, about 4 minutes.
- Check keys are present before running. A run that dies halfway
  has already spent the money.

## Hard rule

Show ONE still and get approval before animating. Then propose
ONE motion and get approval before running it. See `SKILL.md`.

## Never do this

- Never stack a code spring or drift on top of a baked loop.
- Never ship the loop without the still.
- The soft contact shadow does not survive matting. Check the
  icon on light surfaces before shipping.

## More detail

- Pipeline, motion choices, and reporting: `SKILL.md`
- Measured limits: `docs/limitations.md`
- Worked example: `examples/timer/README.md`
- Expo summary: `../expo-expert-skill/references/animated-icons-3dicon.md`
