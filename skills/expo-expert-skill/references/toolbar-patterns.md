# Header and Toolbar Patterns (Expo)

SwiftUI idea: toolbars hold actions, overflow holds the rest, and
visibility follows the content. The Expo form is the screen header
in Expo Router, with the same rules.

## One header, one job

- The header names the screen and holds its primary actions.
  Two primary actions at most. The rest go in a menu or on the
  screen itself.
- A header action is an icon with a label for VoiceOver. An icon
  with no label is silent.
- Keep the header stable across screens. A header that changes
  shape on every screen feels broken.

## Overflow

- Put secondary actions in an overflow menu, not in a second row
  of icons.
- The menu opens from one clear control. Its items are named
  plainly: "Share", "Delete", "Edit".
- A destructive action in a menu asks for confirmation before
  it runs.

## Visibility

- A header can hide on scroll for reading screens, and return
  on scroll up. It must never hide the only way back.
- The back control is always visible. Hiding it is a bug.
- A floating action button is not a second header. One primary
  action per screen, in one place.

## Large titles

- A large title is for a top-level screen, not for every screen.
  Detail screens use a small title.
- The title is sentence case, plain language, and short.

## Checklist

- [ ] Header holds at most two primary actions.
- [ ] Secondary actions live in a menu.
- [ ] Every header icon has a VoiceOver label.
- [ ] Back is always reachable.
- [ ] Titles are short, sentence case, plain language.

## Worked example: a detail header

Screen: one saved item. The header shows the item's title and
two actions: Share and Edit. Delete lives in the overflow menu,
because it is destructive and secondary.

- Two primary actions. The header is calm.
- Delete asks for confirmation. A destructive action never runs
  from a single tap in a menu.
- The title is short. A long title truncates, and the full title
  is the first line of the screen body.

## Common mistakes

- Four icons in a header. The user cannot tell which matters.
- A header action with no VoiceOver label. VoiceOver reads
  "button" and nothing else.
- A second row of actions below the header. That is a toolbar
  pretending not to be one. Put the extras in a menu.
- A floating action button plus a header action that does the
  same thing. One primary action, in one place.
- Hiding the back button to save space. There is no space saving
  worth a user who cannot leave.

## Menus

- Menu items are verbs: "Share", "Edit", "Delete". Not nouns.
- A menu closes after one tap. A menu that stays open after its
  action ran is a bug.
- A menu must be reachable by VoiceOver and by keyboard where
  the platform supports it.
## Back behaviour

- Back goes to the screen the user came from, not to the top of
  the app. A back button that goes home is a bug.
- Back from a form with unsaved changes asks first. Losing work
  on a back tap is worse than one extra prompt.
