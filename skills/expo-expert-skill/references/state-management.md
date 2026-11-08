# State Management (Expo)

SwiftUI idea: each view owns only the state it reads. The Expo form
is the same: keep state close to where it is used, and split shared
state by topic so one change does not re-render the whole app.

## Choose the right home for state

| State | Home |
|---|---|
| One screen, one control (toggle, text input, sheet open) | `useState` in that component |
| A few related values that change together | `useReducer` |
| Shared by a subtree (theme, auth, settings) | Context, one context per topic |
| Server data (feed, profile, list) | A data layer with caching; check your available skills before adding a library |
| Derived data (filtered list, total) | Compute it during render, or `useMemo` when the work is heavy |

## Rules

- Local state stays local. Do not lift state to a parent "just in case".
- A child gets a setter only when it must change the parent's state.
  Read-only children get values, not setters.
- Do not copy a parent prop into child `useState` as a seed. The seed
  runs once and then goes stale. If you must seed once, document it
  as a one-time seed and name the prop it came from.
- Keep the store out of the component. Put fetch, parse, and save
  logic in a hook or a small module so it can be tested alone.
- State updates happen on user action or data arrival, never during
  render. No `setState` calls inside the render body.
- Keep frequently changing values (scroll offset, timer ticks, text
  input) close to the smallest subtree that reads them. Do not put a
  ticking value in a top-level context.

```tsx
// Good: state lives where it is read.
function CounterCard() {
  const [count, setCount] = useState(0);
  return (
    <Pressable onPress={() => setCount((c) => c + 1)}>
      <Text>{count}</Text>
    </Pressable>
  );
}

// Bad: a parent owns state only one child reads, so every parent
// re-render drags the child along.
```

```tsx
// Good: a reducer for values that change together.
type Action = { type: "increment" } | { type: "reset" };
const [state, dispatch] = useReducer(reducer, { count: 0 });
```

## Async state

- Track three states: loading, data, error. Never show stale data
  as if it were fresh.
- Cancel or ignore late responses when the screen unmounts or the
  request is superseded. A slow reply must not overwrite a newer one.
- Show an error state with a retry action. Do not fail silently.

## Checklist

- [ ] Each piece of state has one owner.
- [ ] No prop is copied into `useState` without a documented reason.
- [ ] No `setState` during render.
- [ ] Fast-changing state is not in a top-level context.
- [ ] Loading, data, and error states are all handled.

## Worked example: a settings screen

A settings screen has toggles for reminders, sound, and dark mode.
Each toggle is local `useState` in its row, and a save writes the
whole settings object at once. The screen does not lift every
toggle into a top-level store, because no other screen reads them
while the user is editing.

- Local state for editing. Shared state for the saved result.
- The save is one action, not three. A half-saved settings screen
  is a bug.
- If the user leaves without saving, the edits are discarded and
  the screen says so. Silent loss is worse than a prompt.

## Common mistakes

- Lifting state "just in case". A parent that owns state only one
  child reads is a re-render waiting to happen.
- A child that receives a setter it never uses. Read-only children
  get values.
- Seeding child state from a prop and never updating it. The seed
  runs once. The prop changes. The child shows stale data.
- `setState` during render. It loops, or it warns, and either way
  it is wrong. Effects and handlers are for updates.
- A loading screen that shows stale data as fresh. Loading, data,
  and error are three states. Show which one you are in.
