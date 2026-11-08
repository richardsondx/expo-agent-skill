# Environment Patterns — Context and Theme (Expo)

SwiftUI idea: `@Environment` passes values down the tree without
prop drilling, and stable defaults keep views from re-rendering for
no reason. The Expo form is React Context, plus a theme object.

## One context per topic

Do not make one giant `AppContext` that holds theme, auth, settings,
and navigation. Every value change re-renders every consumer.

```tsx
// Good: split by topic.
const ThemeContext = createContext<Theme>(lightTheme);
const AuthContext = createContext<AuthState | null>(null);
```

A component that reads only the theme should not re-render when
auth changes. Split contexts make that true.

## Stable context values

A new object literal on every render is a new value, even when the
contents are the same. Consumers re-render for nothing.

```tsx
// Bad: new object every render.
<ThemeContext.Provider value={{ mode, accent }}>
  {children}
</ThemeContext.Provider>

// Good: memoise the value.
const value = useMemo(() => ({ mode, accent }), [mode, accent]);
<ThemeContext.Provider value={value}>{children}</ThemeContext.Provider>
```

## Theme

Keep one theme object with named tokens. Do not scatter colour
literals through components.

```tsx
export const lightTheme = {
  ground: "#FAF6EF",
  card: "#FFFFFF",
  ink: "#1A1A2E",
  muted: "rgba(26, 26, 46, 0.6)",
  accent: "#7C5CFF",
  radius: 24,
};

export const darkTheme = {
  ground: "#14142B",
  card: "#1F1F3D",
  ink: "#FFFFFF",
  muted: "#9A9AB5",
  accent: "#7C5CFF",
  radius: 24,
};
```

- Read the theme from context in the component that uses it.
- Never read a global screen or theme value from a module-level
  variable that can go stale. Read it where it is consumed.
- Dark mode is a full theme, not an inverted filter. Every screen
  ships a night variant.

## Defaults

- A context default must be stable and safe to render with. Do not
  put a fresh object, a new date, or a random id in a default value.
- If a context has no sensible default, make the default `null` and
  fail loudly in the hook that reads it, so a missing provider is
  found at once instead of rendering wrong data.

```tsx
function useTheme() {
  const theme = useContext(ThemeContext);
  if (!theme) throw new Error("useTheme needs a ThemeProvider");
  return theme;
}
```

## Checklist

- [ ] One context per topic.
- [ ] Context values are memoised.
- [ ] Theme tokens live in one place.
- [ ] No new object, date, or id in a default value.
- [ ] Missing providers fail loudly.
