# Text Patterns (Expo)

SwiftUI idea: text is a layout citizen, not a string pasted on
top. The Expo form is the same: type scale, wrapping, and
Dynamic Type.

## Type scale

Use Richardson's scale. Hero number 64-120pt, weight 800.
Screen title 28-34pt, weight 700, sentence case. Section label
15-17pt, weight 600. Body 15-17pt, line height 1.4. Caption
12-14pt muted.

- One scale per app. Do not invent a new size per screen.
- Sentence case for titles. No ALL CAPS headings.
- Warm, plain, second-person copy: "You slept better today."
  No jargon, no exclamation spam.

## Wrapping and clipping

- Text wraps. Do not set a fixed height on text that can grow.
- A hero number must survive AX5 without clipping. Test it.
- Truncate with an ellipsis only when the full text is reachable
  somewhere else (a detail screen, an expand control). Truncation
  that hides the only copy is a bug.

## Numbers

- Hero numbers are the screen. A giant number with a unit and one
  line of warm subcopy beats a paragraph.
- Format numbers for the user's locale. Do not hard-code a decimal
  separator or a date format.
- Count-up on appear is fine. It must respect reduced motion.

## Links in text

- A link inside text must be tappable, with a generous target,
  and must never fail silently (see `expo-house-rules.md`).
- A link's text says where it goes: "Read the privacy policy",
  not "Click here".

## Checklist

- [ ] Type scale is shared, not invented per screen.
- [ ] Titles are sentence case.
- [ ] Text survives AX5 without clipping.
- [ ] Truncated text is reachable in full elsewhere.
- [ ] Numbers and dates follow the user's locale.

## Worked example: a hero number

Screen: sleep. The hero is "7h 12m" at 96pt, weight 800. Under
it, one line: "You slept better today." Under that, a small
caption: "Up 32 minutes from last week."

- The number is the screen. Everything else supports it.
- The subcopy is warm and plain. It says what changed, in
  words a person would use.
- The caption adds the comparison. It does not repeat the number.

## Common mistakes

- ALL CAPS headings. They read as shouting, and they are harder
  to scan than sentence case.
- Jargon microcopy: "Leverage your wellness journey." No one
  talks like that. Say what happened.
- Exclamation spam: "Great job!!!" One exclamation is enough,
  and most screens need none.
- A paragraph where a sentence would do. If the user must read
  three lines to know what a card means, the card is wrong.
- Fixed-height text that clips at AX5. The largest text size
  is a real user, not an edge case.

## Empty states

- An empty state says what is missing and what to do next, in
  one or two lines: "No entries yet. Add your first one below."
- It does not apologise at length, and it does not blame the user.
- It has one action, not three.
## Numbers in context

- A number alone is not a story. "7h 12m" needs its line: "You
  slept better today." The number is the fact. The line is the meaning.
- A change needs its baseline: "Up 32 minutes from last week."
  Without the baseline, the number floats.
