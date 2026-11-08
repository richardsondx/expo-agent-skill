# Scroll Patterns (Expo)

SwiftUI idea: scroll position is state, and programmatic scrolling
needs a real target. The Expo form is `ScrollView` and `FlatList`
refs, with the same care.

## Choose the right scroller

| Content | Use |
|---|---|
| Short, fixed content (settings, a form) | `ScrollView` |
| Long or growing content | `FlatList` (see `list-patterns.md`) |
| Horizontal paging (onboarding, cards) | `ScrollView` horizontal with paging, or FlatList horizontal |

Do not put a `FlatList` inside a `ScrollView` in the same direction.
Nested scrolling in one direction breaks windowing and gestures.

## Scroll position

- Keep a ref to the scroller when you need to scroll in code.
- Scroll to a target after layout, not before. Scrolling before
  the content has measured does nothing, silently.
- Restore scroll position when returning to a list. Losing your
  place on back-navigation is a bug.
- Do not fight the user's scroll. A scroll that jumps while the
  user is reading is worse than no scroll at all.

```tsx
const listRef = useRef<FlatList>(null);
// After an item is added, scroll to it only if the user asked.
listRef.current?.scrollToIndex({ index, animated: true });
```

## Keyboard and scroll

- A form that scrolls must keep the focused field visible above
  the keyboard.
- Use the platform's keyboard-avoiding behaviour. Check the
  versioned Expo docs for the current recommended approach; do not
  guess from memory.
- Tapping outside a text field should dismiss the keyboard, unless
  the screen has a real reason not to.

## Pull to refresh and pagination

- Pull to refresh reloads the current data. It does not reset the
  user's place to the top unless the data set changed.
- Paginate before the user reaches the end (see `list-patterns.md`).
- Show a loading indicator for refresh and for the next page.
  Never load silently with no feedback.

## Checklist

- [ ] Right scroller for the content size.
- [ ] No nested same-direction scrollers.
- [ ] Programmatic scrolls happen after layout.
- [ ] Scroll position survives back-navigation.
- [ ] Focused fields stay visible above the keyboard.

## Worked example: returning to a list

A user scrolls a feed, taps an item, reads the detail, and goes
back. The feed is where they left it. Same scroll position, same
items. Losing that place is a bug, not a quirk.

- The list keeps its scroll position because the screen was not
  unmounted and rebuilt on return.
- If the list must reload, it reloads the data, not the position.
- A refresh that resets the user to the top without asking is
  the same bug in a different coat.

## Common mistakes

- A FlatList inside a ScrollView in the same direction. The inner
  list cannot window, and gestures fight.
- Scrolling to an index before layout. The content has not
  measured yet, so the scroll does nothing, silently.
- A keyboard that covers the field being typed in. The field must
  stay visible above the keyboard.
- Pull to refresh that resets the list to the top when the data
  did not change. Refresh the data, keep the place.
- A scroll that jumps while the user reads. Do not move the
  content under a reading user unless they asked for the move.
