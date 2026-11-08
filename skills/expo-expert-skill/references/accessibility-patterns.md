# Accessibility Patterns (Expo)

SwiftUI idea: accessibility traits, grouping, and Dynamic Type
are correctness rules. The Expo form is the same, in React
Native props.

## Labels and roles

Every custom control gets a label and a role. A button that is
just an icon is silent to VoiceOver until you label it.

```tsx
<Pressable
  accessibilityLabel="Save"
  accessibilityRole="button"
  onPress={save}
>
  <Icon name="checkmark" />
</Pressable>
```

- The label says what the control does, not what it looks like.
  "Save", not "Green checkmark".
- A control that is disabled says so. Do not leave a disabled
  control that reads as active.

## Grouping

- Group a label and its value so VoiceOver reads them together:
  "Sleep, 7 hours 12 minutes", not "Sleep" then, separately,
  "7 hours 12 minutes".
- Do not group a whole screen into one element. The user must be
  able to move through it piece by piece.

## Dynamic Type

- Layout must survive Dynamic Type at AX5, the largest size.
  Test it. Hero numbers must not clip.
- Do not set a fixed height on text. Let text grow.
- Do not use 11pt body text to fit more in. If it does not fit,
  cut content, not size.
- Font scaling is on by default. Turning it off for a screen
  needs a real reason, and that reason is almost never good enough.

## Reduced motion and contrast

- Respect reduced motion everywhere (see `animation-basics.md`).
- Respect the system's contrast and bold-text settings where
  the platform exposes them. Check the versioned docs.
- Colour is never the only signal. A status shown only in red
  and green is invisible to a colour-blind user. Add a label,
  an icon, or text.

## Charts and images

- Every chart has a text equivalent (see `charts-accessibility.md`).
- Every meaningful image has a label. Decorative images are
  marked decorative so they are skipped.

## Permissions

- Show a primer screen before every system dialog (camera,
  motion, notifications). Explain the value first.
- A denied permission gets a clear path to settings, not a dead end.

## Checklist

- [ ] Every custom control has a label and a role.
- [ ] Labels say what a control does.
- [ ] Layout survives AX5 with no clipping.
- [ ] Colour is never the only signal.
- [ ] Charts have text equivalents.
- [ ] Permission primers come before system dialogs.

## Worked example: a custom toggle

A settings row has a custom switch: a pill with a dot. It is not
a system switch, so it needs everything the system switch gives
for free.

- `accessibilityRole="switch"` so VoiceOver says "switch".
- `accessibilityState={{ checked: on }}` so it says on or off.
- `accessibilityLabel="Sleep reminders"` so it says what it controls.
- A tap toggles it, and VoiceOver announces the new state.

Without those four, the control is a silent coloured pill.

## Common mistakes

- An icon button with no label. VoiceOver reads "button".
- A label that describes the look: "Green checkmark". Say what
  it does: "Save".
- A whole card grouped as one element, so the user cannot reach
  the button inside it. Group the label and value, not the card
  and its actions.
- Colour as the only signal for an error. A red border means
  nothing to a user who cannot see red. Add text: "This field
  is required."
- Turning off font scaling for a screen because the design looks
  better fixed. The design is not the user. The user is the user.

## Testing

- Turn VoiceOver on and use the app for five minutes. Every
  control you cannot use is a bug.
- Turn on AX5, the largest text size, and walk the main screens.
  Nothing clips, nothing overlaps.
- Turn on reduced motion and confirm loops stop and springs jump
  to their end state.
