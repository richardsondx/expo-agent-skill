# Animation Advanced (Expo)

SwiftUI idea: phase and keyframe animations, animatable values,
and performance. The Expo form is reanimated shared values,
sequences, and layout animations — with the same warning: keep
the work off the render path.

## Sequences and keyframes

- Chain steps in order: anticipate, act, settle. A squash before
  a jump, a recoil after a tap.
- Keep each step short. A long chain that cannot be interrupted
  feels frozen.
- Let the user interrupt a sequence. A tap during an animation
  must do something, not queue up behind a finished animation.

## Interpolating values

Derive secondary motion from one driving value instead of running
two animations that can drift apart.

```tsx
// One shared value drives opacity and position together,
// so they can never fall out of sync.
const progress = useSharedValue(0);
const style = useAnimatedStyle(() => ({
  opacity: progress.value,
  transform: [{ translateY: (1 - progress.value) * 16 }],
}));
```

- One driver, many outputs. That is the whole trick.
- Do not run a separate timer per animated property.

## Layout animations

- Animate layout changes (insert, remove, reorder) with the
  library's layout animation support. Check the versioned docs
  for the current API.
- A list that reorders should animate the move, not flash the
  new order in place.
- Keep layout animations short. A slow layout animation makes
  the app feel heavy.

## Performance

- Animations that run on every frame must not trigger React
  re-renders. Use shared values and animated styles.
- Avoid animating properties the platform cannot animate cheaply.
  Check the versioned docs for your animation library before
  assuming a property is cheap.
- Measure a janky animation with the profiler
  (see `expo-profiling.md`). Do not tune blind.

## Never stack animations

- Do not put a code spring on top of a baked animated loop.
  Pick one. See `animated-icons-3dicon.md`.
- Do not run two libraries' animations on the same element.

## Checklist

- [ ] One driver, many outputs.
- [ ] Sequences are interruptible.
- [ ] Layout changes animate, not flash.
- [ ] No animation triggers re-renders every frame.
- [ ] No stacked animations on one element.

## Worked example: a ring that sweeps

A progress ring sweeps from 0 to 72% on appear. The number inside
counts up at the same time. One shared value drives both: the ring
sweep and the number.

- The ring and the number can never disagree, because they share
  a driver.
- The sweep takes about 600ms. The number counts up over the same
  600ms.
- On reduced motion, both jump to the end state at once. No sweep,
  no count-up, same final screen.

## Common mistakes

- A sequence that cannot be interrupted. The user taps during a
  2-second chain and nothing happens until it finishes. Every
  sequence must be interruptible.
- Two animations on the same property from two places. One wins,
  the other fights it, and the element jitters.
- Animating a property the platform cannot do cheaply, then
  blaming the device. Check the versioned docs for your animation
  library before choosing what to animate.
- A layout animation on a list that also re-renders every row.
  The animation and the re-render fight, and the list flashes.

## Keyframes

- Use keyframes when the motion has distinct beats: anticipate,
  act, settle. A squash, then a jump, then a landing wobble.
- Each beat is short. A keyframe chain is a sentence, not a novel.
- Name the beats in a comment. A chain with no names is hard to
  tune later, because no one can tell which beat is wrong.
