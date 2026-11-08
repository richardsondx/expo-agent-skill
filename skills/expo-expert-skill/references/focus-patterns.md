# Focus Patterns (Expo)

SwiftUI idea: focus is private state, and redundant focus writes
in tap handlers cause bugs. The Expo form is refs and `TextInput`
focus, with the same restraint.

## Text input focus

- Give each field a ref when you need to move focus in code.
- Move focus on submit: the return key advances to the next field,
  and the last field submits.
- Do not steal focus on mount unless the screen is a search field
  the user opened on purpose. A keyboard that appears uninvited
  hides the content the user came to see.

```tsx
const emailRef = useRef<TextInput>(null);
const passwordRef = useRef<TextInput>(null);

<TextInput
  ref={emailRef}
  returnKeyType="next"
  onSubmitEditing={() => passwordRef.current?.focus()}
/>
```

## Focus and taps

- A tap on a field focuses it. Do not add a tap handler that also
  writes focus state; the double write is the bug SwiftUI's
  guidance warns about, in Expo form.
- A tap outside a field dismisses the keyboard. A tap on a button
  must still work — dismissing the keyboard must not swallow the
  tap that follows it.

## Focus order

- Focus moves in reading order: top to bottom, left to right.
- A screen reader's focus order must match the visual order.
  If they differ, fix the layout, not the focus order.
- Modal sheets trap focus while open. When the sheet closes,
  focus returns to the control that opened it.

## Errors and focus

- When a form fails validation, move focus to the first field with
  an error and announce the error.
- An error message must be linked to its field so a screen reader
  reads them together.

## Checklist

- [ ] Return key advances; last field submits.
- [ ] Focus is not stolen on mount without a reason.
- [ ] No double focus writes in tap handlers.
- [ ] Focus order matches visual order.
- [ ] Validation moves focus to the first error.

## Worked example: a login form

Two fields: email, then password. Email's return key says "next"
and moves focus to password. Password's return key says "go" and
submits. A wrong password moves focus back to the password field
and announces "Incorrect password. Try again."

- The user never taps a field to move. The keyboard does it.
- The error moves focus to the field that needs fixing.
- The keyboard does not appear until the user opens the screen
  to log in. It is a login screen, so focus on mount is right
  here — and the exception proves the rule: focus on mount needs
  a reason, and this screen has one.

## Common mistakes

- A tap handler that also writes focus state. The field focuses
  itself on tap. The extra write fights it.
- Dismissing the keyboard and swallowing the tap that follows.
  The user taps a button, the keyboard closes, and the button
  does nothing. The tap must still land.
- Focus order that follows the code order, not the visual order.
  The screen reads top to bottom; focus must too.
- A sheet that opens and leaves focus on the screen behind it.
  A screen reader user is still on the old screen, hearing
  nothing about the sheet.
- An error message that appears far from its field, with no link
  between them. The user hears the error and cannot find the field.
