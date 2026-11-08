# Liquid Glass (Expo)

SwiftUI idea: Liquid Glass is an iOS 26 effect, adopted only when
the user explicitly asks, with a fallback for older systems. The
Expo rule is the same — and stricter, because there is no single
Expo glass component to reach for.

## Only when asked

- Do not add a glass effect on your own. Only when the user
  explicitly asks for Liquid Glass.
- When they ask, check the versioned Expo docs first. Do not
  invent a package name, a component name, or a prop. If the
  docs do not show a supported way to do it in the SDK version
  in `package.json`, say so and stop.
- Do not install an unvetted third-party glass library on a
  guess. Name it, show its docs, and get approval first.

## Fallback is mandatory

- Every glass surface ships an opaque fallback: a solid card
  with Richardson's tokens (24-32pt radius, soft shadow).
- The fallback must look finished, not like a missing effect.
  If the glass is unavailable, the screen still looks designed.
- Test the fallback on its own. If you cannot see the glass,
  you must still be able to use the app.

## Where glass fits

- Glass fits floating controls: a tab bar, a toolbar, a small
  chip over content.
- Glass does not fit body text, forms, or anything that must
  stay readable over a busy background. Legibility wins.
- Keep glass subtle. If the effect is the first thing a user
  notices, it is too much.

## Checklist

- [ ] Glass was explicitly requested.
- [ ] The approach comes from the versioned Expo docs, not memory.
- [ ] No package or component name was invented.
- [ ] An opaque fallback exists and looks finished.
- [ ] Text over glass stays readable.

## What good looks like

A floating tab bar with a subtle glass effect over a pastel
ground. The icons stay readable. The ground behind shows
through just enough to feel layered, not enough to fight the
icons. Turn the effect off, and the same bar is a solid card
with the same icons in the same places. Nothing moves. Nothing
is lost.

## What bad looks like

- Body text over a busy photo through glass. The text is
  unreadable, and the fix is not a stronger shadow. The fix is
  no glass there.
- A whole screen of glass cards. The effect stops being special
  and starts being noise.
- A glass effect that only exists on one phone and one OS
  version, with no fallback, so other users see a broken screen.

## Before you build it

1. Confirm the user asked for it, in words.
2. Open the versioned Expo docs for the SDK version in
   `package.json`. Search for the glass approach there.
3. If the docs show a supported way, name the component and
   its props from the docs, and build the opaque fallback first.
4. If the docs do not show a supported way, stop and say so.
   Do not invent a package.

## Checklist, continued

- [ ] The fallback was built first, not last.
- [ ] The fallback was tested on its own, with glass off.
- [ ] Glass is used on floating controls only, not body text.
- [ ] The effect is subtle enough that the content is still
  the loudest thing on screen.
