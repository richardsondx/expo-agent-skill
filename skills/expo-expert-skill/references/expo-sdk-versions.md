# Expo SDK Versions — Latest APIs (Expo)

This file replaces the SwiftUI `latest-apis.md`. SwiftUI's list of
deprecated-to-modern API mappings does not port: Expo APIs change
every SDK release, and a mapping written from memory goes stale.

## The rule

Do not trust training data for Expo, EAS, or React Native APIs.
APIs you remember are likely renamed, moved, or removed.

Before writing any code that touches an Expo, EAS, or React
Native API:

1. Read the major version of the `expo` package in `package.json`.
2. Fetch the matching versioned docs:
   `https://docs.expo.dev/versions/v<major>.0.0/`
3. For anything else, fetch `https://docs.expo.dev/llms.txt` — an
   index of all Expo docs, with corrections to common LLM
   misconceptions. Follow its links to the specific page you need.
   Never answer from memory.

## No pinned version in this skill

This skill never pins an Expo SDK version number. Every reference
that needs a version says "read the expo version in `package.json`".
If you find a version number in this repo, it is a bug — report it.

## Migrating

- When an API is removed in a new SDK, the versioned docs say
  what replaces it. Follow the docs for that SDK version, not a
  blog post about a different version.
- Migrate one SDK at a time. Do not skip versions and guess the
  combined changes.
- After a migration, run `npx expo install --fix`, then
  `npx expo lint` and `npx tsc --noEmit`.

## Installing packages

- Install only with `npx expo install <package>`. It resolves the
  version that matches the SDK. Never `npm install` a native
  package by hand.
- If a package has no SDK-compatible version, say so. Do not force
  an incompatible version in and hope.

## Checklist

- [ ] `expo` version read from `package.json`.
- [ ] Versioned docs fetched for that major version.
- [ ] `llms.txt` consulted for anything outside the versioned docs.
- [ ] Packages installed with `npx expo install` only.
- [ ] Lint and typecheck pass after any migration.

## Worked example: a renamed API

You remember an Expo API from a tutorial. You write it from
memory. Typecheck fails, or worse, it typechecks and behaves
differently at runtime. The fix is the same every time: read the
`expo` version in `package.json`, open the versioned docs for
that major version, and read the current page for that API. The
tutorial was written for a different SDK. The docs for your SDK
are the only source that matters.

## Common mistakes

- Answering from memory. The docs exist to stop this.
- Following a blog post for SDK N while the app runs SDK N+2.
- Installing with `npm install` and getting a version the SDK
  does not support, then debugging the mismatch for an hour.
- Pinning a version number in a skill or a doc. It goes stale
  on the next SDK release. Say "read the expo version in
  `package.json`" instead.
- Skipping SDK versions during migration. Each version's docs
  describe its own changes. Skipping means guessing the combined
  changes.

## After an upgrade

1. Run `npx expo install --fix`.
2. Run `npx expo-doctor`.
3. Run `npx expo lint` and `npx tsc --noEmit`.
4. Run the app in a development build and walk the main flows.
5. Ship the upgrade separately from feature work, so a regression
   has one suspect, not two.
