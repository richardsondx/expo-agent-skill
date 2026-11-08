# Animation Basics (Expo)

SwiftUI idea: implicit vs explicit animation, and always name the
value that drives the change. The Expo form is
`react-native-reanimated`: springs and shared values, with the
same discipline.

## Springs, not easings

Richardson's motion language uses springs. In reanimated, that is
`withSpring`. The house values are damping 0.82, stiffness 180.

```tsx
import { withSpring } from "react-native-reanimated";
const scale = withSpring(pressed ? 0.96 : 1, {
  damping: 0.82,
  stiffness: 180,
});
```

- Durations: 300-500ms for transitions, 120-200ms for presses
  (scale 0.96).
- Staggered entrances: cards rise 16pt and fade, 60ms apart, on
  first appear.
- Hero numbers count up on appear. Rings sweep.
- A breathing pulse on the primary live element: 2.4s loop,
  scale 1.0 to 1.04.

## Name the driver

An animation must be driven by a named value. Do not animate
"whenever anything changes". If the value is not named, the
animation fires at the wrong time, or not at all.

## Animate layout, not re-renders

- Prefer animating a shared value over re-rendering a component
  sixty times a second.
- Keep animated styles out of the React render path where the
  library allows it. Check the versioned docs for the animation
  library you use.
- Never animate by calling `setState` in a loop.

## Reduced motion

Always respect reduced motion. `useReducedMotion` from
`react-native-reanimated` tells you when to stop.

```tsx
import { useReducedMotion } from "react-native-reanimated";
const reduced = useReducedMotion();
// When reduced is true, jump to the end state. No spring, no loop.
```

- Animated WebP cannot honour reduced motion on its own. Swap to
  the still (see `animated-icons-3dicon.md`).
- Never ship an animation that blocks input.

## Checklist

- [ ] Springs use the house values unless a reason says otherwise.
- [ ] Each animation names its driving value.
- [ ] No `setState` loop drives an animation.
- [ ] Reduced motion is respected everywhere.
- [ ] No animation blocks input.

## Worked example: a press

A pill button is pressed. It scales to 0.96 in 120-200ms, with a
spring. On release it springs back. A light haptic fires on the
press. That is the whole interaction.

- The scale change is small. A button that shrinks to 0.8 looks
  broken, not responsive.
- The haptic and the scale happen together. A haptic that fires
  after the animation ends feels late.
- The button stays tappable during the animation. An animation
  that blocks the next tap is a bug, not polish.

## Common mistakes

- Animating with `setState` in a timer. Sixty re-renders a second
  for a scale change is waste. A shared value does it without
  re-rendering.
- A spring with no damping value. The default may overshoot in a
  way that looks playful on one screen and broken on another.
  Name the damping and stiffness every time.
- Animating opacity and position with two separate drivers. They
  drift apart under load. One driver, many outputs.
- Forgetting reduced motion on a looping animation. A pulse that
  never stops is the worst offender. Loops stop when reduced
  motion is on.

## Choosing a duration

- Press feedback: 120-200ms.
- A screen or sheet: 300-500ms.
- A staggered entrance: 60ms between cards, each card 300ms.
- A count-up number: about 600ms, easing out, so it lands softly.
- Anything longer than 500ms needs a reason, and the reason is
  usually that the animation is doing too much.
