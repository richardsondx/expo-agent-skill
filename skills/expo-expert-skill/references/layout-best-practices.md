# Layout Best Practices (Expo)

SwiftUI idea: layouts resize, safe areas are real, and no screen
is one fixed size. The Expo form is flex, safe-area insets, and
`useWindowDimensions`.

## Flex first

- Use flexbox for layout. Rows, columns, gaps, and wrapping cover
  most screens.
- No fixed 390x844 frames. No hard-coded screen width or height.
- Use percentages and flex values for widths that must adapt.
- Test at the extremes: narrowest, widest, shortest, tallest, and
  roughly square. Look for clipped text and overlapping controls.

```tsx
const { width } = useWindowDimensions();
const twoColumn = width >= 700;
```

Read the size where it is consumed. Do not read a global screen
value once at module load and reuse it forever; rotation and
split-screen change it.

## Safe areas

- Use `react-native-safe-area-context`. Wrap the app in its
  provider and read insets where they are needed.
- Content never sits under the notch, the Dynamic Island, or the
  home indicator. A floating tab bar clears the home indicator.
- Do not double-apply insets. If a navigator already adds the top
  inset, the screen inside it must not add it again.

```tsx
import { useSafeAreaInsets } from "react-native-safe-area-context";
const insets = useSafeAreaInsets();
// Apply insets.bottom to a floating bar, once.
```

## Two-column reflow

On wide screens, reflow to two columns instead of stretching one
column across the width. A single column that spans a tablet is
hard to read.

- Below about 700pt wide: one column.
- At and above it: two columns, or a list plus a detail pane.
- Keep the same data and the same order. Only the arrangement
  changes.

## Design tokens (Richardson)

Grounds (light): cream `#FAF6EF`, lavender `#E9E4F7`, mint
`#DDF3E3`, peach `#FFE7D2`, baby blue `#DCE9F7`, butter `#F7DC6F`,
blush `#F8C9D4`. Night: deep indigo `#14142B`, card `#1F1F3D`,
elevated `#2A2A4D`. Ink `#1A1A2E` on light, `#FFFFFF` on night.
Muted: 60% ink on light, `#9A9AB5` on night. One accent per
screen: violet `#7C5CFF`, green `#7BC96F`, pink `#FF6B9D`,
blue `#4A90D9`, amber `#F5A623`. Never pure black grounds, never
neon, never two accents competing.

Type: hero number 64-120pt, weight 800, tight tracking. Screen
title 28-34pt, weight 700, sentence case. Section label 15-17pt,
weight 600. Body 15-17pt, line height 1.4. Caption 12-14pt muted.

Shape: cards 24-32pt radius, soft shadow. Pills for buttons,
56-64pt tall. Floating tab bar 24-28pt radius, 16pt side margins,
12pt above the home indicator. Chips are pills for filters.

Layout: single column, 20-24pt gutters, 16-20pt card padding.
Rhythm: hero metric, then a week strip or ring, then a 2-3 column
stat grid, then cards. At most 5-6 cards before scrolling feels long.

## Checklist

- [ ] No fixed screen-size frames.
- [ ] Safe areas applied once, from the safe-area library.
- [ ] Wide screens reflow instead of stretching.
- [ ] Layout checked at narrow and wide extremes.
- [ ] One accent per screen, tokens from the list above.
