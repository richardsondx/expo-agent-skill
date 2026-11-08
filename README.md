# Expo Agent Skill

Expert guidance for any AI coding tool that supports the
[Agent Skills open format](https://agentskills.io/home) — Expo and
React Native state, composition, performance, navigation, animation,
accessibility, charts, and Liquid Glass, for iOS apps built with Expo.

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
- **Motion craft** — `animate-expo` (Reanimated, gestures, sheets,
  haptics), motion review, motion audit and improvement plans
- **Motion judgement** — finding places that earn motion, removing
  motion that does not, naming motion precisely
- **Breaking UI** — worst-case data: long names, empty lists, huge counts
- **Prototyping** — several variants of one UI piece, with a switcher
- **Accessibility** — VoiceOver, Dynamic Type at AX5, roles and labels
- **Charts** — concepts only, with text equivalents
- **Liquid Glass** — only when asked, with an opaque fallback
- **SDK versions** — read `package.json`, check versioned Expo docs,
  never trust training data
- **Profiling** — Hermes profiler, React DevTools, Sentry, Maestro
- **House rules** — `npx expo install` only, lint and typecheck before
  done, Expo Router, generated native folders, Ionicons only
- **Animated 3D icons** — 3dicon, vendored with its own skill file

## What we cover

This repo covers Expo animation and motion craft: building motion
that runs on the UI thread, with springs that carry velocity and
gestures that track 1:1. It covers review and audit of motion, so
existing animations can be checked against a strict bar and fixed
in priority order. It covers finding places that would genuinely
benefit from motion, and removing motion that does not earn its
place. It covers naming motion precisely, so the right words
produce the right feel. It covers breaking UI with worst-case data,
and prototyping several variants of one piece before choosing.
It also covers the Expo motion principles distilled from Emil
Kowalski's work, in
`skills/expo-expert-skill/references/emil-motion-principles.md`.

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
    expo-profiling.md, expo-house-rules.md, animated-icons-3dicon.md,
    emil-motion-principles.md
skills/3dicon/
  SKILL.md, EXPO-USAGE.md, iconloop/, docs/limitations.md,
  examples/timer/, requirements.txt, .env.example
skills/animate-expo/
  SKILL.md, RECIPES.md
skills/review-animations/
  SKILL.md, STANDARDS.md
skills/improve-animations/
  SKILL.md, AUDIT.md, PLAN-TEMPLATE.md
skills/find-animation-opportunities/, skills/remove-animations/,
skills/animation-vocabulary/, skills/break-ui/,
skills/prototype/
  SKILL.md (plus CATALOG.md, PICKER.md where listed above)
```

## Credits and attribution

- [SwiftUI Agent Skill](https://github.com/AvdLee/SwiftUI-Agent-Skill)
  by [Antoine van der Lee](https://www.avanderlee.com) — the source
  skill this Expo port is adapted from, MIT.
- [3dicon](https://github.com/samyost1/3dicon) by
  [Sam Yost](https://github.com/samyost1) — vendored in
  `skills/3dicon/`, MIT.
- [Skills](https://github.com/emilkowalski/skills) by
  [Emil Kowalski](https://github.com/emilkowalski) — `animate-expo`,
  `review-animations`, `improve-animations`,
  `find-animation-opportunities`, `remove-animations`,
  `animation-vocabulary`, `break-ui`, and `prototype` vendored in
  `skills/`, MIT. The Expo motion principles in
  `skills/expo-expert-skill/references/emil-motion-principles.md`
  are distilled from his `gestures`, `choreograph`, `apple-design`,
  `emil-design-eng`, and `animate` skills.
- Richardson's Expo house rules come from his Stride project:
  `npx expo install` only, lint and typecheck before done, Expo
  Router, generated native folders, Ionicons only, and links that
  never fail silently.

## License

MIT. See [LICENSE](LICENSE) for the Expo adaptation copyright and
the attribution notices for Antoine van der Lee, Sam Yost, and
Emil Kowalski.
