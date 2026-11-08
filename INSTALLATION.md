# Installation

## Option A: skills.sh (recommended)

```bash
npx skills@latest add https://github.com/richardsondx/expo-agent-skill --skill expo-expert-skill
```

Then ask your agent to use the expo expert skill, for example:

> Use the expo expert skill and review this Expo code for state and performance problems.

## Option B: Claude Code plugin

```bash
/plugin marketplace add richardsondx/expo-agent-skill
/plugin install expo-expert@expo-expert-skill
```

## Option C: Manual install

1. Clone this repository.
2. Copy `skills/expo-expert-skill/` (and, if you want it,
   `skills/3dicon/`) into your tool's skills folder. See your tool's
   docs for the exact path.
3. Use your agent as usual and ask it to use the "expo-expert" skill.

## 3dicon setup (only if you use it)

```bash
cd skills/3dicon
pip install -r requirements.txt
cp .env.example .env   # then fill in your own keys; never commit .env
```

`ffmpeg` must be on PATH. See `skills/3dicon/SKILL.md` and
`skills/3dicon/EXPO-USAGE.md`.

## How to verify

Your agent should reference the workflow and checklist in
`skills/expo-expert-skill/SKILL.md` and open only the reference file
for the topic at hand.
