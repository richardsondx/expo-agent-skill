# Performance Patterns (Expo)

SwiftUI idea: keep hot paths cheap and measure before optimising.
The Expo form is the same. Most slowness comes from too many
re-renders, too much work on the main thread, or images at the
wrong size.

## Re-renders

- Pass only the props a component reads. Extra props cause extra
  renders.
- Split contexts by topic (see `environment-patterns.md`). A ticking
  value in a top-level context re-renders the app.
- Memoise expensive derived data with `useMemo`. Memoise callbacks
  passed to memoised children with `useCallback`.
- Use `memo` only where a measurement shows a problem. Blanket
  `memo` adds cost and hides the real cause.

## Lists

- Use `FlatList` for long lists. Do not render hundreds of rows in
  a `ScrollView`.
- Stable `keyExtractor`, never the index (see `list-patterns.md`).
- Keep row components small and memoised where it helps.
- For very long or fast lists, evaluate FlashList — check the
  versioned Expo docs for the current install and API before
  using it. Do not guess its props from memory.
- Paginate. Load the next page before the user reaches the end,
  not after.

## Images

- Size images to their display size. A 4000px photo in a 64px
  avatar is wasted decode time and memory.
- Prefer `expo-image` for remote images. Check the versioned docs
  for its current props.
- Cache remote images. Do not re-download on every screen visit.

## Main thread

- Keep work off the main thread where the platform allows it.
  Heavy parsing, image work, and long loops belong elsewhere.
- Do not block input with a full-screen spinner that has no
  progress. Show progress, or let the user keep using the app.
- Animations run best when they stay on the native driver path.
  Check the versioned docs for the animation library you use
  before assuming a property animates natively.

## Startup

- Defer work that is not needed for the first screen.
- Do not fetch every tab's data at launch. Fetch on visit.
- Keep the first render light. A splash screen is not a fix for
  a slow first render.

## Measure first

See `expo-profiling.md`. A guess about slowness is usually wrong.
Measure, fix the hottest cause, measure again, and report the
before and after.

## Checklist

- [ ] Hot paths measured, not guessed.
- [ ] Lists use FlatList with stable keys.
- [ ] Images are sized to display size.
- [ ] No ticking value in a top-level context.
- [ ] Startup fetches only what the first screen needs.

## Worked example: a ticking timer

A timer screen ticks once a second. The ticking value lives in the
timer component, not in a top-level context. The rest of the app
never re-renders for a tick. The timer display updates. Nothing
else does.

- The fast-changing value is as low in the tree as it can be.
- The screen still shows the time. Nothing was lost by keeping
  the state local.
- If the timer had lived in the app context, every tick would
  re-render every consumer. That is the most common performance
  bug in a React Native app, and it looks like nothing is wrong
  until the app grows.

## Common mistakes

- `memo` everywhere. Blanket memo adds cost and hides the real
  cause, which is usually a value in the wrong place.
- Filtering a large list inside `renderItem`. Filter once, upstream.
- Fetching every tab's data at launch. Fetch on visit.
- A full-screen spinner with no progress. The user waits with no
  idea how long. Show progress, or let them keep using the app.
- Animating by calling `setState` in a loop. Use a shared value.

## Startup checklist

- [ ] First screen fetches only what it shows.
- [ ] Other tabs fetch on visit.
- [ ] Heavy work is deferred past the first render.
- [ ] No full-screen spinner without progress.
