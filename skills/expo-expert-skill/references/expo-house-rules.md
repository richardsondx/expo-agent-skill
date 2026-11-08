# Expo House Rules (Stride)

These rules come from Richardson's Stride project
(`~/workspace/stride/AGENTS.md`). They apply to every Expo app
built with this skill.

## Do not trust training data

Expo ships breaking changes every SDK release. Before writing
any code that touches an Expo, EAS, or React Native API:

1. Read the major version of the `expo` package in `package.json`.
2. Fetch the matching versioned docs:
   `https://docs.expo.dev/versions/v<major>.0.0/`
3. Fetch `https://docs.expo.dev/llms.txt` for anything else.
   Follow its links. Never answer from memory.

## Commands

```bash
npx expo install <package>  # ALWAYS, instead of npm/yarn/pnpm/bun add
npx expo start              # start the dev server
npx expo lint               # lint
npx tsc --noEmit            # typecheck
npx expo-doctor             # diagnose dependency and config issues
npx expo install --fix      # fix incompatible package versions
```

Use `bunx` instead of `npx` if the project uses bun
(`bun.lock` present). Run lint and typecheck before declaring
any task done.

## Navigation

- Expo Router for all navigation. Routes live in `src/app/`.
  `_layout.tsx` files define navigators.
- Keep non-route code outside `src/app/`.
- Import `Link`, `router`, and `useLocalSearchParams` from
  `expo-router`.

## EAS

- Use EAS to build, sign, and submit (`eas build`, `eas submit`)
  and to ship over-the-air updates (`eas update`).
- Run EAS CLI as `bunx eas-cli <command>` in Bun projects, or
  `npx eas-cli@latest <command>` otherwise.

## Generated native folders

- If `ios/` and `android/` do not exist, they are generated
  (Continuous Native Generation). Never create or edit them by
  hand. Configure native behaviour in `app.json` and config plugins.
- Expo Go only includes its bundled native modules. After adding
  a library with native code, the app needs a development build.

## Icons

- Use Ionicons via `@expo/vector-icons`, through a single Icon
  component that maps semantic names to glyphs.
- NEVER `expo-symbols` / `SymbolView`. It renders only on native
  iOS and draws nothing on web or Android. This shipped as
  invisible icons across a whole app, caught Oct 13, 2026 on Drift.
- SF Symbols are fine as the naming reference, not the renderer.

## Link-tap rule

- A build is not ready while any link tap can die silently.
- All external links go through one helper that shows the URL
  with a Copy Link option when opening fails.
- Never report links as working from code inspection alone.
  Test the tap.

## Checklist

- [ ] Packages installed with `npx expo install` only.
- [ ] Lint and typecheck pass.
- [ ] Routes in `src/app/`; other code outside it.
- [ ] No hand edits to `ios/` or `android/`.
- [ ] Icons go through the single Icon component (Ionicons).
- [ ] No link tap can fail silently.
