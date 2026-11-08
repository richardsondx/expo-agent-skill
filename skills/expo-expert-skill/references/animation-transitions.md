# Animation Transitions (Expo)

SwiftUI idea: transitions describe how a view enters and leaves,
and matched-geometry moves an element between two places. The
Expo form is layout animations and shared-element style moves,
with the same rule: one element, one journey.

## Entering and leaving

- An element that appears should enter from a place that makes
  sense: a sheet rises from the bottom, a card fades and rises
  16pt, a detail slides in from the side it came from.
- An element that leaves should exit the way it came in, not
  vanish or fly off in a new direction.
- Keep enter and exit symmetric. An asymmetric pair feels broken.

## Shared elements

When one element becomes another — a thumbnail grows into a
detail image, a chip becomes a header — move the same element.
Do not fade one out and fade a copy in. The eye notices the swap.

- If your animation library offers a shared-element transition,
  check its versioned docs for the current API. Do not guess the
  prop names from memory.
- Keep the moving element's content stable during the move. Text
  that reflows mid-flight looks broken.

## Screen transitions

- Screen transitions belong to the navigator. Do not add a second
  transition on top of the navigator's own animation.
- A modal sheet rises. A pushed screen slides. A dismissed screen
  reverses its entry. Keep those meanings stable across the app.

## Interruptible transitions

- A transition the user can interrupt (a swipe to dismiss, a
  drag) must handle being cancelled halfway. It must land in a
  real state, never halfway between two screens.
- Test the cancel path. The happy path is easy; the cancelled
  gesture is where transitions break.

## Checklist

- [ ] Enter and exit are symmetric.
- [ ] Shared elements move, they are not swapped for copies.
- [ ] No second transition on top of the navigator's.
- [ ] Cancelled gestures land in a real state.

## Worked example: a card becomes a detail

A user taps a small card. The card's image grows into the detail
screen's header image. The title moves with it. The body text
fades in after the move, not during it.

- The image is the shared element. It moves. Nothing else moves
  with it.
- The title travels because it belongs to the image, not because
  it was animated separately.
- The body waits. Fading body text in mid-flight makes the whole
  move look cheap, because the eye follows the image and the text
  is noise until the image lands.

If the detail screen has no image, there is no shared element.
Do not invent one. A plain slide is better than a fake shared move.

## Common mistakes

- Two transitions on one navigation: the navigator slides the
  screen in, and the screen also fades itself in. The result is
  a double move that feels slow.
- An exit that does not match its entry. A sheet that rises on
  entry and fades on exit feels like it went somewhere else.
- A transition that starts before the data arrives. The screen
  animates in empty, then the content pops in. Load first, or
  show a skeleton that is part of the transition.
- Animating a list reorder by re-rendering the whole list. The
  rows flash. Animate the move of the rows that changed.

## Timing

- A screen transition takes 300-500ms. Longer feels heavy.
- A card growing into a detail takes about 400ms. Shorter feels
  abrupt; longer feels like waiting.
- An element that leaves takes the same time as it took to enter.
  Symmetry is what makes a transition feel honest.
