# Sheet and Navigation Patterns (Expo)

SwiftUI idea: navigation is type-safe, sheets are enum-based, and
the back gesture is sacred. The Expo form is Expo Router, with the
same discipline.

## Expo Router

- Routes live in `src/app/`. Every file there is a screen.
  `_layout.tsx` files define navigators.
- Keep non-route code (components, hooks, utils) outside `src/app/`.
- Import `Link`, `router`, and `useLocalSearchParams` from
  `expo-router`. Check the versioned Expo docs for the current
  router API before using a prop you have not used before.

```tsx
import { Link, router } from "expo-router";

// Declarative:
<Link href="/settings">Settings</Link>
// Imperative:
router.push("/items/42");
```

## Deep links

- Define the link scheme in `app.json`. Test a real link, not just
  the in-app navigation.
- A deep link must land on the right screen with the right data,
  or show a clear error. It must never land on a blank screen.
- Handle a link to an item that no longer exists: show "not found",
  not a crash.

## Sheets

- A sheet is for a short task: a picker, a detail, a form. It is
  not a second app.
- A sheet has a grabber, a clear close action, and a 24pt top radius
  in Richardson's design language.
- Closing a sheet must not lose the screen behind it. State stays
  where it was.
- Do not stack sheets. One sheet at a time. A sheet that opens
  another sheet is a sign the flow needs a real screen.

## Tabs

- Five slots at most in a tab bar. Six or more is a navigation
  problem, not a design choice.
- Richardson's language uses a floating tab bar with a centre
  action where it fits. Keep it clear of the home indicator.
- The edge-swipe back gesture stays sacred. Do not add a custom
  gesture that fights it.

## State and navigation

- Pass small params through the route (an id, not a whole object).
  Fetch the object on the destination screen.
- Do not pass functions through route params.
- After a destructive action, navigate away. Do not leave the user
  on a screen for something that no longer exists.

## Checklist

- [ ] Routes live in `src/app/`; other code lives outside it.
- [ ] Deep links tested end to end.
- [ ] Sheets are short tasks with a clear close.
- [ ] No stacked sheets.
- [ ] Tab bar has five slots at most.
- [ ] Route params carry ids, not whole objects or functions.

## Worked example: a filter sheet

A list screen has a Filter button. Tapping it opens a sheet with
filter options and Apply and Clear actions. Applying closes the
sheet and updates the list. Clearing resets the options and leaves
the sheet open, so the user can choose again.

- The sheet is a short task. Pick filters, apply, done.
- Closing the sheet does not lose the list behind it.
- The sheet does not open another sheet. If a filter needs a
  second screen, that is a navigation push, not a sheet.

## Common mistakes

- Passing a whole object through route params. Pass an id. Fetch
  the object on the destination.
- Passing a function through route params. Functions do not
  survive navigation. Use a shared store or a callback registry.
- A deep link that lands on a blank screen. Test real links.
- A sheet that stacks on a sheet. One sheet at a time.
- Six tabs. Five is the most. Six is a navigation problem.
- Leaving the user on a screen for an item that was just deleted.
  Navigate away after a destructive action.
