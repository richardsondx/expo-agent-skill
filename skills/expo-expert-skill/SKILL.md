---
name: expo-expert-skill
description: Use when writing, reviewing, or refactoring Expo / React Native apps for iOS, including state and Context data flow, component composition, resizable layouts, safe areas, performance, FlatList, Expo Router navigation, sheets, animation, accessibility, charts, Liquid Glass, SDK version checks, profiling, and animated 3D icons. Also use for large-display, tablet, and foldable layouts (useWindowDimensions, two-column reflow), Dynamic Type, and reduced-motion handling.
---

# Expo Expert Skill

Adapted from the SwiftUI Expert Skill by Antoine van der Lee
(https://github.com/AvdLee/SwiftUI-Agent-Skill). The principles ported
here are the ones that survive the move from SwiftUI to React Native:
small components that own only the data they read, stable list identity,
lazy work, honest fallbacks, and accessibility as a correctness rule,
not a polish step. Swift-only ideas did not port. See README.md.

## Operating Rules

- Treat each component as an invalidation boundary: give it only the
  props it reads. Keep fast-changing state close to the smallest
  subtree that needs it.
- Read `references/expo-sdk-versions.md` before writing or migrating
  API usage. Read the `expo` version in `package.json` first, then
  fetch the matching versioned docs. Never trust training data for
  Expo, EAS, or React Native APIs.
- Install packages only with `npx expo install`. Never `npm install`
  a native package by hand.
- Run `npx expo lint` and `npx tsc --noEmit` before declaring a task done.
- Prefer Expo modules over third-party libraries. Check your available
  skills before adding a dependency.
- Use Expo Router for all navigation. Routes live in `src/app/`.
  Keep non-route code outside `src/app/`.
- `ios/` and `android/` are generated. Never create or edit them by
  hand. Configure native behaviour in `app.json` and config plugins.
- Only adopt Liquid Glass when the user explicitly asks for it
  (see `references/liquid-glass.md`). Ship an opaque fallback.
- Present performance work as suggestions, not requirements, unless
  a measurement shows a real problem.
- Focus on correctness and performance. Do not enforce one
  architecture (MVVM, Redux, Zustand, and so on).
- Separate business logic from components so it can be tested,
  without mandating how.
- Follow Apple's Human Interface Guidelines.
- Icons: use Ionicons via `@expo/vector-icons` through one Icon
  component. NEVER `expo-symbols` / `SymbolView`: it renders
  nothing on web or Android.
- External links must never fail silently. If opening a link fails,
  show the URL with a Copy Link option.
- Respect reduced motion. Use `useReducedMotion` and swap animated
  art for a still.
- Layout must survive Dynamic Type at AX5. Never clip a hero number.
- No fixed 390x844 frames. Use flex, percentages, and
  `useWindowDimensions`.

## Task Workflow

### Review existing Expo code

- Read the code under review and name which topics apply.
- Flag APIs that may have changed. Check them against the versioned
  Expo docs, not memory.
- Run the Topic Router below for each relevant topic.
- For a broad review, split it into small focus areas and present
  them one at a time.

### Improve existing Expo code

- Audit against the Topic Router topics.
- Replace removed APIs with the versioned-docs equivalent. Flag
  soft-deprecated APIs and leave them in place during feature work
  unless the user asked to migrate.
- Refactor hot paths so fewer components re-render.
- Extract large component bodies into small subcomponents.
- Suggest image downsampling when a full-size remote image is
  rendered at icon size (see `references/image-optimization.md`).

### Implement a new Expo feature

- Design data flow first: owned state vs injected state vs server state.
- Structure components for cheap re-renders (extract early).
- Apply the right animation pattern (see animation references).
- Use `Pressable` for tappable elements. Add accessibility labels
  and roles to every custom control.
- Provide a fallback for every version-gated feature.

### Profile a slow app

Trigger when the user asks to profile, measure, or find a slowdown.
Full reference: `references/expo-profiling.md`.

1. Say what feels slow, and on what device.
2. Reproduce it in a development build, not Expo Go, if native
   modules are involved.
3. Measure first (Hermes profiler, React DevTools, or a Maestro
   run). Do not optimise from a guess.
4. Fix the hottest cause, re-measure, and report the before/after.

## Topic Router

Consult the reference file for each topic relevant to the task:

| Topic | Reference |
|-------|-----------|
| State management | `references/state-management.md` |
| Context, theme, and environment | `references/environment-patterns.md` |
| Component composition | `references/view-structure.md` |
| Style composition and identity | `references/modifier-patterns.md` |
| Performance | `references/performance-patterns.md` |
| Lists and FlatList | `references/list-patterns.md` |
| Resizable layout, safe areas, two-column reflow | `references/layout-best-practices.md` |
| Large display, tablet, foldable | `references/large-display.md` |
| Sheets, navigation, Expo Router | `references/sheet-navigation-patterns.md` |
| ScrollView and scroll position | `references/scroll-patterns.md` |
| Focus management | `references/focus-patterns.md` |
| Animations (basics) | `references/animation-basics.md` |
| Animations (transitions) | `references/animation-transitions.md` |
| Animations (advanced) | `references/animation-advanced.md` |
| Accessibility | `references/accessibility-patterns.md` |
| Charts (concepts) | `references/charts.md` |
| Charts accessibility | `references/charts-accessibility.md` |
| Image optimisation | `references/image-optimization.md` |
| Headers and toolbars | `references/toolbar-patterns.md` |
| Web content and in-app browser | `references/web-content.md` |
| Liquid Glass (iOS 26+) | `references/liquid-glass.md` |
| Text patterns | `references/text-patterns.md` |
| Localisation | `references/localization.md` |
| Deprecated API handling | `references/expo-sdk-versions.md` |
| Handling soft-deprecated APIs | `references/soft-deprecation.md` |
| Previews: Expo Go, dev client, Storybook | `references/previews.md` |
| Profiling (Hermes, DevTools, Maestro) | `references/expo-profiling.md` |
| Expo house rules | `references/expo-house-rules.md` |
| Animated 3D icons (3dicon) | `references/animated-icons-3dicon.md` |

## Design Tokens (Richardson)

Pastel grounds (cream `#FAF6EF`, lavender `#E9E4F7`, mint `#DDF3E3`,
peach `#FFE7D2`, baby blue `#DCE9F7`, butter `#F7DC6F`) or deep
indigo night `#14142B`. Ink `#1A1A2E` on light, white on night.
One accent per screen (violet `#7C5CFF`, green `#7BC96F`,
pink `#FF6B9D`, blue `#4A90D9`, amber `#F5A623`). Hero numbers
64-120pt, weight 800. Cards 24-32pt radius, soft shadow.
Pills for buttons. Floating tab bar. Springs, not easings
(`withSpring`, damping 0.82, stiffness 180). Warm plain copy,
second person. See `references/layout-best-practices.md` for the
full token list.

## Correctness Checklist

These are hard rules — violations are always bugs:

- [ ] Local state is private to its component (`useState`, not a
  shared mutable object)
- [ ] A child receives a setter only when it must change parent state
- [ ] Parent-owned values are not copied into child `useState` as a
  seed, unless the one-time seed is documented
- [ ] `FlatList` uses a stable `keyExtractor` (never the index)
- [ ] `FlatList` rows render one item each; no side effects in render
- [ ] Context values are stable (memoised); no new object each render
- [ ] Safe areas come from `react-native-safe-area-context`, not
  hard-coded insets, and are not double-applied
- [ ] No fixed screen-size frames; layout uses flex and
  `useWindowDimensions`
- [ ] Animations respect reduced motion
- [ ] Every custom control has `accessibilityLabel` and
  `accessibilityRole`
- [ ] Layout survives Dynamic Type at AX5
- [ ] Images are sized to their display size, not full resolution
- [ ] External links show a fallback when opening fails
- [ ] Lint and typecheck pass: `npx expo lint`, `npx tsc --noEmit`
- [ ] New packages were installed with `npx expo install`
- [ ] Preview and test data are self-contained; no live service
  needed to render a screen
