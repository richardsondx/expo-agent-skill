# Previews — Expo Go, Dev Client, Storybook (Expo)

SwiftUI Previews render a view with mock data in Xcode. Expo has
no direct equivalent. This file covers the three Expo ways to see
a screen early, with mock data, and no live service.

## Expo Go

- Expo Go runs the app from the dev server on a real phone.
- It only includes its bundled native modules. After adding a
  library with native code, Expo Go cannot run the app. You need
  a development build.
- Use Expo Go for fast UI checks on screens that need no new
  native code.

## Development build

- Build once per app with `npx expo run:ios` (or the Android
  equivalent), or `eas build --profile development`.
- Then start the dev server with the dev client. Hot reload works.
  Rebuild only when a native dependency, config, or SDK changes.
- This is the fastest inner loop for a real app. Expo Go is not
  a substitute once native modules are involved.

## Storybook

- Storybook renders one component at a time, in isolation, with
  mock data. That is the closest Expo form to a SwiftUI Preview.
- A story needs no live service and no network. If a story needs
  a server to render, the component is too coupled. Split it.
- Use stories for the shared primitives: Card, PillButton,
  HeroNumber, WeekStrip, charts. One story per state: loading,
  empty, error, full, large text.

## Mock data

- Preview and test data are self-contained. A screen must render
  from mock data alone.
- Keep mock data in one place, next to the story or the test.
  Do not scatter invented data through components.
- Mock data must look real: real names, real lengths, real edge
  cases (a very long title, an empty list, a missing image).

## Checklist

- [ ] Screens render from mock data, with no live service.
- [ ] Stories exist for shared primitives, in each state.
- [ ] Expo Go limits are respected; dev build used when needed.
- [ ] Mock data covers long text, empty, and error states.

## Worked example: a Card story

The shared `Card` component gets four stories: full (title, body,
chevron), empty (title only), long title (a title that wraps to
three lines), and large text (AX5). Each story uses mock data and
renders with no network.

- The long-title story catches clipping the full screen would hide.
- The large-text story catches a fixed height the developer set
  to make the design look tidy.
- The empty story catches a layout that assumes a body always exists.

## Common mistakes

- A story that needs a live server. The story is then a test of
  the server, not the component.
- Mock data that is too tidy: every title short, every list full.
  Real data is messy. Mock the mess.
- Only testing the happy state. Loading, empty, and error are
  states too, and they are the ones users actually see when
  something goes wrong.
- Using Expo Go to test a screen that needs a native module, then
  concluding the screen is broken. Expo Go cannot run it. Use a
  development build.

## Development build, in short

- Build once per app. Hot reload after that.
- Rebuild when a native dependency, a config, or the SDK changes.
- If the dev server and the build disagree, rebuild. Do not debug
  the server for a native mismatch.
