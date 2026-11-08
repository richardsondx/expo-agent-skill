# Contributing to Expo Agent Skill

Thanks for helping make Expo guidance clearer and more correct.

## What this repo is

Expo / React Native guidance for iOS apps. Each skill lives in its
own folder under `skills/` and keeps its own source and attribution.

## Rules

- Short sentences. Plain language. No invented APIs.
- If you are unsure of an exact Expo or React Native API name, write
  "check the versioned Expo docs" instead of guessing.
- Never pin an Expo SDK version number. Say "read the expo version in
  `package.json`".
- Never present Swift code as Expo code. Translate the principle, then
  show TypeScript / TSX.
- Keep each reference file focused, 80-300 lines.
- Keep attribution intact. Do not remove the Antoine van der Lee or
  Sam Yost notices in LICENSE.

## How to contribute

1. Fork the repo (or branch if you have access).
2. Make one focused change.
3. Check `SKILL.md` and the reference files still agree with each other.
4. Open a pull request with a short summary and test notes.

## Updating SDK guidance

After a new Expo SDK release, update
`skills/expo-expert-skill/references/expo-sdk-versions.md` from the
versioned docs at `https://docs.expo.dev/versions/v<major>.0.0/` and
`https://docs.expo.dev/llms.txt`. Do not update it from memory.
