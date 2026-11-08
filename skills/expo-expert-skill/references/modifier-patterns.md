# Modifier Patterns — Style Composition (Expo)

SwiftUI idea: modifiers compose, and conditional modifiers can
change a view's identity. The Expo form is style arrays and small
style helpers. The same care applies: compose styles, do not fork
components.

## Compose with style arrays

Later entries win. Use that order on purpose.

```tsx
<View style={[styles.card, isActive && styles.cardActive]} />
```

- Keep base styles in a `StyleSheet`. Put the varying part in the
  array, not in a second component.
- Do not build a style object inline on every render for a hot list
  row. Move it to the sheet, or memoise it.

## Conditional styles, not conditional components

When only the look changes, change the style. Do not swap the whole
component tree, or state inside it will reset.

```tsx
// Good: same component, different style.
<Pressable style={[styles.pill, selected && styles.pillSelected]}>

// Bad: two different components for one toggle.
{selected ? <SelectedPill /> : <PlainPill />}
```

## Variants as data

When a component has a few looks, name them and keep the map in
one place.

```tsx
const tones = {
  violet: { backgroundColor: "#7C5CFF" },
  green: { backgroundColor: "#7BC96F" },
  pink: { backgroundColor: "#FF6B9D" },
} as const;

function AccentDot({ tone }: { tone: keyof typeof tones }) {
  return <View style={[styles.dot, tones[tone]]} />;
}
```

## No-effect values

Do not apply a style that changes nothing. A style with the same
value as the default is noise. It also hides the real overrides.

## Shared primitives

Build the repeated pieces once: `Card`, `PillButton`, `Chip`,
`SectionLabel`. Reuse them. Do not invent a new primitive per
screen. When a screen needs a new look, extend the shared
primitive with a variant, or say why it cannot.

## Checklist

- [ ] Styles compose through arrays, in a deliberate order.
- [ ] A look change does not swap the component.
- [ ] Variants are named data, not scattered literals.
- [ ] No style object is rebuilt inline in a hot list row.
- [ ] Shared primitives are reused before new ones are made.

## Worked example: a selectable chip

A filter chip has two looks: plain and selected. One component,
one style array. The selected style is appended when selected.

```tsx
<Pressable
  style={[styles.chip, selected && styles.chipSelected]}
  accessibilityState={{ selected }}
>
  <Text style={[styles.chipText, selected && styles.chipTextSelected]}>
    {label}
  </Text>
</Pressable>
```

- Same component in both states. Its state never resets on toggle.
- The style array order is deliberate: selected wins.
- VoiceOver hears "selected" because the state is announced, not
  just shown in colour.

## Common mistakes

- Two components for two looks. Toggling swaps the tree and loses
  state inside it.
- A style object built inline in a hot list row, on every render.
  Move it to the sheet.
- A variant added as a boolean prop, then another, then five more.
  Name the variants as data and keep the map in one place.
- A style that changes nothing. It hides the real overrides in noise.
- A new primitive per screen. Extend the shared one with a variant
  first, and say why if you cannot.
