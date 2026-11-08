# Charts — Concepts Only (Expo)

SwiftUI Charts has a native API with marks, axes, and selection.
Expo has no built-in equivalent. This file covers concepts only.
It names two libraries as options — Victory and
react-native-gifted-charts — and invents no API for either. Check
the versioned docs of the library you choose before writing code.

## Choose the chart by the question

| Question | Chart |
|---|---|
| How does a value change over time? | Line or area |
| How do categories compare? | Bar |
| What share is each part? | Donut, with the values also in text |
| What is the range of a value? | Range bar |
| Is one value above a target? | Bar with a target line, plus text |

- One chart answers one question. A chart that tries to answer
  three questions answers none of them.
- If the answer fits in one sentence, use the sentence, not a chart.

## Data shape

- Keep chart data as a plain array of small objects: a label, a
  value, and optionally a category. Do not pass component state
  or screen objects into a chart.
- Sort time data by time. Sort categories by value, largest first,
  unless the order itself means something.
- Missing data is a gap, not a zero. A zero says "we measured
  nothing". A gap says "we did not measure".

## Styling

- Use Richardson's tokens: pastel segments, rounded caps, one
  accent per screen. No neon, no pure black grounds.
- Axes and gridlines are quiet. The data is the loudest thing on
  the chart.
- Keep labels short. A long label on a small chart is unreadable.
- Animate a chart on appear (count-up, sweep), once, and respect
  reduced motion. Do not re-animate on every render.

## Selection and interaction

- Tapping a point shows its exact value. The tap target must be
  generous — at least 44pt.
- Selection state lives in the screen, not inside the chart.
- When a point is selected, say so in text as well as in colour.

## What this file does not do

- It does not document Victory or react-native-gifted-charts props.
  Their APIs change. Read their docs at the version you install.
- It does not recommend one library over the other. Pick by the
  chart types you need, the bundle cost, and the accessibility
  support in the version you install.

## Checklist

- [ ] One chart, one question.
- [ ] Data is a plain array of label and value.
- [ ] Missing data is a gap, not a zero.
- [ ] Tap targets are at least 44pt.
- [ ] A text equivalent exists (see `charts-accessibility.md`).

## Worked example: a week of sleep

The question: did I sleep more this week than last week? The
chart is a bar chart, seven bars, one per night, with last
week's average as a faint target line.

- Seven bars, not thirty. A week is the unit the user thinks in.
- The target line is labelled in text: "Last week: 6h 40m".
- Tapping a bar shows that night: "Friday, 8h 5m".
- Under the chart, one sentence: "You averaged 7h 12m, up from
  6h 40m last week."

If the sentence answers the question, the chart is a bonus. If
only the chart answers it, the chart has failed the users who
cannot see it.

## Common mistakes

- A chart with no question. It shows data because data exists.
- A donut with eight slices. No one can compare eight thin slices.
  Show the top three and group the rest as "Other", with the
  values in text.
- A line chart that starts the axis above zero to exaggerate a
  change. A 5% change that looks like a doubling is a lie.
- Missing data drawn as zero. A night with no measurement is a
  gap. Drawing it as zero says the user slept no hours.
- A chart that re-animates on every render. It sweeps again each
  time the screen updates, and the user cannot read a moving chart.

## Choosing a library

- List the chart types you need first. Then check which library
  covers them in the version you would install.
- Check the bundle cost. A chart library is heavy; do not add one
  for a single bar chart you could draw with plain views.
- Check accessibility support in that version: can a selected
  point announce its value? If not, the text equivalent carries
  the whole weight, and that must be a conscious choice.
