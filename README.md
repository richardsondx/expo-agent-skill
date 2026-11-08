# Expo Agent Skill

Expert guidance for any AI coding tool that supports the
[Agent Skills open format](https://agentskills.io/home) — Expo and
React Native state, composition, performance, navigation, animation,
accessibility, charts, and Liquid Glass, for iOS apps built with Expo.

This is an Expo adaptation of the
[SwiftUI Agent Skill](https://github.com/AvdLee/SwiftUI-Agent-Skill)
by Antoine van der Lee. The principles that transfer were translated
into React Native / TypeScript. The parts that do not transfer are
listed honestly below — nothing was faked into an Expo equivalent.

It also vendors [3dicon](https://github.com/samyost1/3dicon) by
Sam Yost: looping animated 3D icons as animated WebP, which
`expo-image` renders natively.

## How to use

```bash
npx skills@latest add https://github.com/richardsondx/expo-agent-skill --skill expo-expert-skill
```

Then ask your agent, for example:

> Use the expo expert skill and review this Expo code for state and performance problems.

See [INSTALLATION.md](INSTALLATION.md) for Claude Code, manual, and
other installs.

## What's inside

- **State management** — local vs shared vs server state, Context
  granularity, stable context values
- **Composition** — small components, one job each, props they read only
- **Layout** — flex, safe areas, two-column reflow, large displays
- **Performance** — memoisation, FlatList tuning, Hermes profiling
- **Lists** — stable keys, windowing, pagination
- **Navigation** — Expo Router, sheets, deep links
- **Animation** — springs, transitions, reduced motion
- **Accessibility** — VoiceOver, Dynamic Type at AX5, roles and labels
- **Charts** — concepts only, with text equivalents
- **Liquid Glass** — only when asked, with an opaque fallback
- **SDK versions** — read `package.json`, check versioned Expo docs,
  never trust training data
- **Animated 3D icons** — 3dicon, vendored with its own skill file

## What transfers easily

| SwiftUI idea | Expo form |
|---|---|
| Small views, only the data they read | Small components, only the props they read |
| `@Observable` granularity | Context split by topic, memoised values |
| Stable `ForEach` identity | Stable `keyExtractor`, never the index |
| Lazy lists | `FlatList` windowing, `getItemLayout` where it helps |
| Safe areas | `react-native-safe-area-context` |
| Springs and transitions | `react-native-reanimated` springs |
| VoiceOver traits and labels | `accessibilityLabel` / `accessibilityRole` |
| Dynamic Type | Font scaling that survives AX5 |
| Image downsampling | Sized images, `expo-image` |
| Liquid Glass, only when asked | Same rule, opaque fallback, check versioned docs |
| Previews with mock data | Expo Go, dev client, Storybook |

## What does NOT transfer

| SwiftUI-only thing | Status |
|---|---|
| macOS scenes, window styling, macOS views | Dropped. No Expo equivalent. |
| Swift Charts API specifics | Dropped. Charts are concepts only here; Victory and react-native-gifted-charts are named as options, with no invented API. |
| Swift Document APIs (`Document`, `DocumentReader`) | Dropped. No equivalent in this skill. |
| Instruments `.trace` scripts (`xctrace`) | Dropped. Replaced by `references/expo-profiling.md`: Hermes profiler, React DevTools, Sentry, Maestro — concepts only, no invented flags. |
| `@State` / `@Binding` / `@Environment` syntax | Translated to hooks and Context, not copied. |
| `NavigationSplitView`, `ToolbarOverflowMenu`, `ArrangementView` | Translated to Expo Router and header patterns, not copied. |
| Styled text editing (`AttributedString`) | Dropped. No reference file; use a text input and check versioned docs. |

## 3dicon — animated 3D icons in Expo

`skills/3dicon/` is the full 3dicon skill, vendored with attribution.
It turns one prompt into a looping animated 3D icon with real
transparency, as an animated WebP. `expo-image` renders animated
WebP natively. No Lottie. No new native dependency.

- Still: about 13 cents. Full run: about 48 cents, about 4 minutes.
- **Hard rule: show ONE still and get approval before animating.**
  Then propose ONE motion and get approval before running it.
- Put output in `assets/icons/`. Ship the still alongside the WebP
  and swap to it when reduced motion is on.
- File size scales with frame count: 122 frames at 24fps, 288px,
  is 857 KB. Encode at the source frame rate. Never sample down
  to 8fps to save size.
- Never stack a code spring or drift on top of a baked loop.
- The soft contact shadow does not survive matting. Check the icon
  on light surfaces before shipping.

Full Expo notes: `skills/3dicon/EXPO-USAGE.md` and
`skills/expo-expert-skill/references/animated-icons-3dicon.md`.

## Skill structure

```text
skills/expo-expert-skill/
  SKILL.md
  references/
    state-management.md, environment-patterns.md, view-structure.md,
    modifier-patterns.md, performance-patterns.md, list-patterns.md,
    layout-best-practices.md, large-display.md,
    sheet-navigation-patterns.md, scroll-patterns.md, focus-patterns.md,
    animation-basics.md, animation-transitions.md, animation-advanced.md,
    accessibility-patterns.md, charts.md, charts-accessibility.md,
    image-optimization.md, toolbar-patterns.md, web-content.md,
    liquid-glass.md, text-patterns.md, localization.md,
    expo-sdk-versions.md, soft-deprecation.md, previews.md,
    expo-profiling.md, expo-house-rules.md, animated-icons-3dicon.md
skills/3dicon/
  SKILL.md, EXPO-USAGE.md, iconloop/, docs/limitations.md,
  examples/timer/, requirements.txt, .env.example
```

## Acknowledgements

- [SwiftUI Agent Skill](https://github.com/AvdLee/SwiftUI-Agent-Skill)
  by [Antoine van der Lee](https://www.avanderlee.com) — the source
  skill this Expo port is adapted from, MIT.
- [3dicon](https://github.com/samyost1/3dicon) by
  [Sam Yost](https://github.com/samyost1) — vendored in
  `skills/3dicon/`, MIT.
- Richardson's Expo house rules come from his Stride project
  (`~/workspace/stride/AGENTS.md`): `npx expo install` only, lint
  and typecheck before done, Expo Router, generated native folders,
  Ionicons only, and links that never fail silently.

## License

MIT. See [LICENSE](LICENSE) for the Expo adaptation copyright and
the attribution notices for Antoine van der Lee and Sam Yost.
