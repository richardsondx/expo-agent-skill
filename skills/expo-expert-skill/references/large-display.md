# Large Display, Tablet, and Foldable (Expo)

SwiftUI idea: `iphone-duo.md` chose a technique by screen
structure — sidebar, two-column reflow, foldable grid — and warned
that resizing is continuous, not a set of named poses. The Expo
form is the same choice, made with `useWindowDimensions`.

## Choose by structure

| Screen structure | Technique |
|---|---|
| List plus detail | Two-pane layout on wide screens, stacked on narrow |
| Grid of cards | More columns as width grows |
| Single-column reading | Cap the width, centre the column |
| Navigation with many sections | Sidebar or tab bar on wide screens; tabs on narrow |

## Read size where it is used

```tsx
import { useWindowDimensions } from "react-native";
const { width, height } = useWindowDimensions();
const wide = width >= 700;
```

- `useWindowDimensions` updates on rotation and resize. A value
  read once at module load does not.
- Check the extremes, not just named sizes: narrowest, widest,
  shortest, tallest, roughly square.
- Look for clipped content, overlapping controls, and text that
  sprawls across a wide screen.

## Two-pane layout

```tsx
{wide ? (
  <View style={{ flexDirection: "row" }}>
    <View style={{ width: 320 }}><ItemList /></View>
    <View style={{ flex: 1 }}><ItemDetail /></View>
  </View>
) : (
  <ItemList />
)}
```

- Keep the same data and order in both forms.
- On narrow screens, tapping an item navigates to the detail.
  On wide screens, tapping selects it in place.
- Do not keep two copies of the list logic. One list, two
  arrangements.

## Foldables and hinges

- Treat a fold as a wide screen with a seam. Do not put a control
  or a line of text across the middle.
- Keep primary actions on one side of the seam.
- If you cannot test a fold on a real device or simulator, say so.
  Do not claim a fold layout works from a phone screenshot.

## Orientation

- Support both orientations unless the app has a real reason not to.
- Rotation must not lose state, scroll position, or an in-progress
  form. If it does, that is a bug, not a quirk.

## Checklist

- [ ] Layout reads size where it is consumed.
- [ ] Wide screens reflow; narrow screens stack.
- [ ] No control sits across a fold seam.
- [ ] Rotation keeps state and scroll position.
- [ ] Extremes checked, not just one phone size.

## Worked example: a list plus detail

A reading app shows a list of items. On a phone, tapping an item
pushes the detail screen. On a tablet, the list sits on the left
at 320pt and the detail sits on the right. Same data, same order,
same list component. Only the arrangement changes.

- The list component is shared. Two copies would drift apart.
- Selection on a tablet highlights the item in place. On a phone,
  selection navigates.
- Rotation from portrait to landscape reflows without losing the
  selected item or the scroll position.

## Common mistakes

- Reading the screen size once at module load. Rotation changes
  it, and the layout is stuck in the old size.
- Stretching one column across a tablet. Text lines run the full
  width and are hard to read. Cap the width and centre the column.
- Two copies of the same screen: one for phones, one for tablets.
  They drift. One screen, two arrangements.
- A control across a fold seam. The user cannot tap half a button.
- Claiming a fold layout works without testing a fold. Say it is
  untested. Do not claim it from a phone screenshot.
