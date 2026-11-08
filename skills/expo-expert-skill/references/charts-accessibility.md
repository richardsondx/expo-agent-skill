# Charts Accessibility (Expo)

A chart without a text equivalent is invisible to a VoiceOver
user. The chart is the picture. The text is the chart, for
everyone who cannot see the picture.

## Always ship a text equivalent

- Under every chart, write one or two sentences: the trend, the
  highest and lowest values, and the current value.
- Example: "Sleep averaged 7 hours 12 minutes this week, up from
  6 hours 40 minutes last week. Best night: Friday, 8 hours 5 minutes."
- The text is not a caption that repeats the title. It carries
  the numbers.

## Structure

- Give the chart container an accessibility label that names the
  chart: "Sleep this week, line chart".
- Mark the chart itself as one accessible element, not a dozen
  small ones. A VoiceOver user should not have to swipe through
  every bar to hear the point.
- Put the text equivalent right after the chart in focus order.

## Colour and contrast

- Colour is never the only way to tell series apart. Use labels,
  shapes, or patterns as well.
- Check contrast of labels and axis text against the background.
  Quiet gridlines are fine. Unreadable labels are not.

## Interaction

- A selected point announces its value. Selection must work by
  VoiceOver, not only by touch.
- If the chart animates on appear, respect reduced motion.
  The text equivalent is already there, so nothing is lost.

## Checklist

- [ ] Every chart has a text equivalent with real numbers.
- [ ] The chart is one accessible element with a clear label.
- [ ] Series are told apart by more than colour.
- [ ] Selection announces its value.
- [ ] Reduced motion is respected.

## Worked example: the sleep sentence

Chart: seven bars, sleep per night. Text equivalent, under the
chart, in the same card:

"Sleep averaged 7 hours 12 minutes this week, up from 6 hours
40 minutes last week. Best night: Friday, 8 hours 5 minutes.
Shortest night: Tuesday, 5 hours 50 minutes."

- The trend is first: up from last week.
- The extremes are named: best and shortest.
- The current value is there: the average.
- A VoiceOver user hears the whole story in one swipe. A sighted
  user gets the same numbers in the chart.

## Common mistakes

- A text equivalent that repeats the title: "Sleep chart". That
  says nothing.
- A chart split into dozens of accessible elements. The user
  swipes through every bar before hearing the point.
- Colour as the only difference between two series. "The green
  line is sleep, the blue line is the target" means nothing to a
  user who cannot see green and blue.
- A selected point that changes colour only. VoiceOver hears
  nothing. The selection must announce its value.
- An animated chart that keeps animating while VoiceOver reads
  it. Respect reduced motion, and let the text carry the meaning.

## Testing

- Turn VoiceOver on. Swipe through the chart. You should hear
  the chart's name, then the text equivalent, then move on.
- Turn on the largest text size. The text equivalent must not
  clip, and the chart must not overlap it.
- Check the chart in greyscale. If two series become one, they
  were told apart by colour alone.
