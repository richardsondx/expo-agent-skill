# Localisation (Expo)

SwiftUI idea: strings live in one place, dates and numbers follow
the locale, and layout survives long translations. The Expo form
is the same discipline, in TypeScript.

## Strings

- Keep user-facing strings in one place: a strings file per
  language, or a localisation library. Check your available
  skills before adding a library.
- Never build a sentence by joining fragments: "You have " + n +
  " new messages". Word order changes by language. Use a full
  string with a placeholder, and plural rules from the library.
- Never hard-code English in a component and translate "later".
  Later never comes.

## Dates, numbers, and currency

- Format dates, times, numbers, and currency with the platform's
  locale-aware formatting. Do not hard-code a format.
- A date shown as "11/08/2026" means different days in different
  countries. Use a locale-aware format, or spell the month out.

## Layout

- German text runs long. A button that fits in English may clip
  in German. Test with a long language.
- Text grows with Dynamic Type and with translation. Layout must
  flex, not clip.
- Icons and images do not need translation, but their labels do.

## Checklist

- [ ] Strings live in one place, not in components.
- [ ] No sentence is built by joining fragments.
- [ ] Dates, numbers, and currency are locale-formatted.
- [ ] Layout is tested with a long language.

## Worked example: a plural

English: "You have 3 new messages." A naive join — "You have " +
count + " new messages" — breaks in languages where the number
sits elsewhere, or where the word for "messages" changes with
the count. Use a full string with a placeholder and the library's
plural rules, so the translator sees the whole sentence.

## Common mistakes

- A sentence built from fragments. Translators cannot reorder
  fragments they cannot see.
- A date hard-coded as month/day/year. In much of the world that
  order is wrong, and "11/08" is a different day.
- A button sized to its English label. "Settings" fits. Its
  German translation does not, and it clips.
- An icon with an English-only label baked into the image. The
  label must be text, so it can be translated.
- Sorting by hard-coded rules. Alphabetical order differs by
  language. Use the platform's locale-aware sort.

## Testing

- Switch the device to a long language, such as German, and
  walk the main screens. Look for clipped buttons and labels.
- Switch to a right-to-left language if the app claims to
  support one. Layout must mirror; a back arrow that points
  the wrong way is a bug.
- Switch the region, not just the language. A phone in English
  with a German region formats dates and currency differently,
  and the app must follow the region.
## Right-to-left

- If the app supports a right-to-left language, layout mirrors:
  back points the other way, lists align the other way.
- Do not hard-code left and right in styles. Use start and end
  where the platform supports it, so mirroring happens for free.
- Numbers and dates still follow the locale. Mirroring the layout
  does not mean mirroring the numbers.

## Strings review

- Read every new string out loud in its full sentence before it
  ships. A string that only makes sense with the code around it
  will confuse a translator.
- Keep a glossary for product names and terms that never translate.
  A brand name translated by accident is a bug.
