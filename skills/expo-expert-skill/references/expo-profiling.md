# Profiling (Expo)

This file replaces the SwiftUI Instruments `.trace` tooling.
`xctrace` and `.trace` files do not port to Expo. The Expo
tooling is different, and this file covers it as concepts only.
It invents no CLI flags. Check the docs of each tool before
running it.

## Start with the symptom

- Say what feels slow, and on what device: startup, a list
  scroll, a navigation, an animation, a tap response.
- Reproduce it in a development build, not Expo Go, when native
  modules are involved.
- A slowness you cannot reproduce is a guess. Do not optimise
  from a guess.

## Tools

| Tool | What it shows |
|---|---|
| Hermes profiler | Where JavaScript time goes |
| React DevTools profiler | Which components re-render, and how often |
| Sentry | Real-user slowness and crashes in production |
| Maestro | Repeatable end-to-end runs on a simulator or device |

- Use the Hermes profiler for JavaScript work. Use React DevTools
  for re-render causes. Use Sentry for what real users hit.
  Use Maestro to repeat a slow flow the same way every time.
- Check each tool's docs for its current commands. Do not guess
  flags from memory.

## Reading a result

- Find the hottest cause first. One hot cause often explains
  many slow screens.
- A blocked main thread and a busy main thread are different
  problems. A blocked thread waits (network, lock, storage).
  A busy thread computes (a large render, a heavy parse).
  Fix the right one.
- A list that re-renders every row on every keystroke has a
  state problem, not a list problem. Move the fast-changing
  state down (see `state-management.md`).

## FlashList checks

- For a long list, check whether the list itself is the problem:
  are rows too large, are keys unstable, is filtering done inside
  `renderItem`? See `list-patterns.md` and
  `performance-patterns.md`.
- Do not switch list libraries to fix a state bug.

## Report

- Report the before and after: what was slow, what changed, and
  the new measurement. A fix with no measurement is a claim.
- Only edit code when the user asked for edits, or when the
  profiling task included fixing.

## Checklist

- [ ] Symptom named, device named, reproduced in a dev build.
- [ ] Measured with a real tool, not guessed.
- [ ] Hottest cause fixed first.
- [ ] Before and after reported.

## Worked example: a slow list

Symptom: a feed stutters when scrolling on a real phone. Measure
with React DevTools first: every row re-renders on every scroll
tick. The cause is not the list. The scroll offset lives in a
top-level context, so every tick re-renders every row. Move the
offset down to the component that reads it. Measure again: rows
render once. The list is smooth. No list library was changed.

## Common mistakes

- Optimising from a guess. "It must be the images" is a guess.
  Measure, then name the cause.
- Fixing the symptom in the wrong layer: switching list libraries
  to fix a state bug, or adding memo everywhere instead of moving
  one value down.
- Profiling in Expo Go when the slowness involves native modules.
  Expo Go cannot run them. Profile in a development build.
- Reporting a fix with no measurement. "It feels faster" is not
  a result. Before and after, with the same flow, on the same
  device.
- Chasing a cold-start number by deferring work the user needs
  immediately. Defer what is not needed for the first screen, not
  what the first screen shows.

## What to measure

- Startup: time to first useful screen, not to splash.
- Scroll: dropped frames while scrolling a long list.
- Tap response: time from tap to visible result.
- Navigation: time from tap to the next screen being usable.
- Pick one, measure it, fix the hottest cause, measure again.
