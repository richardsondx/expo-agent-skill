# Emil Motion Principles (Expo)

Distilled from Emil Kowalski's motion work — `gestures`,
`choreograph`, `apple-design`, `emil-design-eng`, and `animate` in
https://github.com/emilkowalski/skills — translated into Reanimated
/ Expo terms. The web originals use CSS, Motion, and Vaul; none of
that code is copied here. For implementation, use
`skills/animate-expo/` first.

## The gate: should this animate at all?

Answer before writing any motion code. If it fails, write zero
lines. That is a success, not a dodge.

| How often the user sees it | Decision |
|---|---|
| 100+ times a day — tab switches, keyboard, scrolling, settings toggles | No animation. Platform default or nothing. |
| Tens of times a day — press feedback, list navigation, row selection | Near-imperceptible only: under 150ms, or nothing. |
| Occasional — sheets, modals, toasts, onboarding steps | Standard animation. |
| Rare / first time — success states, empty-state art, celebration | The delight budget lives here. |

Tab switches never slide. Tabs are peers, not a hierarchy. Name
the purpose in one word first: **feedback**, **spatial
consistency**, **state indication**, **preventing a jarring
change**, **explanation**, or **delight** (rare tier only). If you
cannot name it, do not build it.

## Springs carry velocity through interruption

A spring is the default for anything a finger touched. When a
gesture ends, the animation must continue at the finger's exact
velocity, so there is no seam between dragging and animating.
When retargeted mid-flight, a spring keeps its current velocity.
Never restart from zero on interruption: start from the live
on-screen value. A closing sheet the user grabs again must follow
the finger, not finish closing first.

```tsx
import { useSharedValue, withSpring } from 'react-native-reanimated';

// Reposition / snap back after a drag: carry the release velocity.
const y = useSharedValue(0);
y.set(withSpring(target, { duration: 400, dampingRatio: 0.8, velocity }));
```

Default settle, no overshoot: `{ duration: 400, dampingRatio: 1 }`.
Sheet or drawer: `{ duration: 300, dampingRatio: 0.8, velocity }`.
Add `overshootClamping: true` when the value must not pass a hard edge.
Bounce only when the gesture carried momentum.

## Transform and opacity only

`transform` and `opacity` are free. Everything else is a layout
pass. `width`, `height`, `margin`, `padding`, `flex`, `top`, and
`left` re-run layout on every frame for that node and its siblings.
Exception: an absolutely positioned element with no children, such
as a tab pill or progress fill — it is out of flow. Never start an
entrance from `scale(0)`. Start from `scale(0.9–0.97)` plus
`opacity: 0`. Keep `transform` order deliberate: translate first,
then scale, unless you want the translate scaled too.

```tsx
// Press feedback: scale takes the label and icons with it.
const scale = useSharedValue(1);
// on press-in: scale.set(withTiming(0.97, { duration: 120 }));
// on press-out: scale.set(withTiming(1, { duration: 120 }));
```

## Gestures track 1:1, on the UI thread

While the finger is down, the UI *is* the finger. No easing, no
duration, no spring during tracking. A spring during tracking makes
the element trail the finger, like dragging through syrup. Drive
position from a shared value, never from `setState` per frame. One
React render per frame is the single biggest cause of jank in
React Native apps. Shared value plus `useAnimatedStyle` means React
never re-renders at all.

```tsx
import { Gesture } from 'react-native-gesture-handler';
import { useSharedValue, useAnimatedStyle } from 'react-native-reanimated';

const translateY = useSharedValue(0);
const style = useAnimatedStyle(() => ({
  transform: [{ translateY: translateY.get() }],
}));
// Gesture.Pan().onUpdate((e) => { translateY.set(e.translationY); })
```

Never read or write a shared value during render. Touch shared
values only in worklets, handlers, and effects. Never schedule back
to the JS runtime inside `onUpdate` — 60–120 calls a second.
Schedule back only in `onEnd`, or at a threshold. `GestureHandlerRootView`
must wrap the app, or gestures do nothing with no error.

## Rubber-band at edges, dismiss on velocity or distance

Dragging past a boundary should move the element a fraction of
the distance, not stop dead. A hard stop reads as frozen.

Dismiss on distance **or** velocity. Passing a distance threshold
(about 40–50% of travel) dismisses, but so does a fast flick over
a short distance. Distance-only thresholds make flicks feel ignored.
Project where the gesture is going from its velocity, then snap to
the target nearest that projected point. Snap-back is fast: if the
gesture does not pass the threshold, return quickly. Bounce belongs
only at the end of a drag, never on a button-press close.

## Easing: ease-out for entering, never ease-in

Entering or exiting: `ease-out`. Moving or morphing on screen:
`ease-in-out`. Constant motion: `linear`. Default: `ease-out`.
Never `ease-in` on UI. It starts slow, delaying the exact moment
the user is watching. The built-in easings are weak — use strong
bezier curves:

```tsx
import { Easing } from 'react-native-reanimated';

const EASE_OUT = Easing.bezier(0.23, 1, 0.32, 1);
const EASE_IN_OUT = Easing.bezier(0.77, 0, 0.175, 1);
const EASE_SHEET = Easing.bezier(0.32, 0.72, 0, 1);
```

Durations: press feedback 100–150ms, toggle or chip 150–200ms,
sheet or modal a spring at about 300ms perceived. Screen
transitions use the platform default.

## Choreography: cut, then run in parallel

Cut moving parts and run the rest in parallel. Sequence only when
order carries meaning: reading order, cause and effect, or one
thing physically arriving before another.

- One entrance per container. If the panel slides in, its content
  is already there. Never animate a parent's entrance *and*
  stagger its children.
- Never stagger high-frequency or keyboard-triggered UI. Dropdown
  items, menu rows, and tab content appear together, or with no
  animation.
- Uniform stagger is forbidden. Vary timing by importance: the lead
  moves first and furthest, support follows smaller, decoration
  fades in place. Product UI: 30–80ms between items, total under
  about 300ms. Stagger only the first handful of a long list.
- Exits are simultaneous, shorter, and simpler. An exit matches its
  entry: something that slid in from the right leaves toward the right.
- Choreography never blocks interaction. Everything on screen must
  be clickable as soon as it is visible.
- Every sequence must survive interruption. Each run stops the
  in-flight motion before starting new motion. Never stack runs.

## Press, not hover

There is no hover on mobile. Every hover affordance has to live in press, position, or nothing.

- Feedback on press-in, commit on press-out. Waiting for the tap
  to complete before showing anything feels dead.
- Press feedback: `scale: 0.97` in 100–150ms on any pressable.
- 44×44pt minimum touch target. If the visual is smaller, add
  `hitSlop` — do not grow the visual.
- Haptics fire on the same frame as the visual, one per user action,
  never on scroll or per frame, and never as the only feedback.
  Many users have haptics off. The visual must stand alone. For
  haptic calls, check the versioned Expo docs for `expo-haptics`.

## Reduced motion ships with the animation

Reduced motion means fewer and gentler, not zero. Keep opacity
and color changes that explain a state change. Drop translation,
scale, parallax, and overshoot. Screen transitions become a fade.
Tracking stays, since the user is producing that motion. What
reduces is momentum flings, bounce, and decorative motion.

```tsx
import { useReducedMotion, ReduceMotion, withSpring } from 'react-native-reanimated';

const reduced = useReducedMotion();
const y = useSharedValue(reduced ? 0 : SHEET_HEIGHT);
// or: withSpring(0, { duration: 300, dampingRatio: 0.8, reduceMotion: ReduceMotion.System });
```

## Judge feel on a release build, on the slowest device

Feel is judged on a release build on the slowest device you
support. Nothing else counts as verified. Expo Go is not a
performance environment, and the simulator hides the problems
you are looking for. Feel-check on device: flick it, interrupt
it mid-flight, reverse it, catch a sheet mid-close and drag it
back. Record it and step through frame by frame. Then imagine
the 50th use of the day. If you would skip it, cut it.

## Where to go next

Build it: `skills/animate-expo/`. Review it:
`skills/review-animations/`. Audit a codebase:
`skills/improve-animations/`. Find or remove motion:
`skills/find-animation-opportunities/`, `skills/remove-animations/`.
Name it: `skills/animation-vocabulary/`. Full web originals:
https://github.com/emilkowalski/skills
