# View Structure — Component Composition (Expo)

SwiftUI idea: extract subviews early so each view is small, cheap
to diff, and easy to read. The Expo form is the same: small
components, one job each.

## Extract early

A component over about 150 lines is a sign to split it. Extract by
job, not by position on screen.

```tsx
// Good: each piece has one job.
function FeedScreen() {
  return (
    <FlatList
      data={items}
      renderItem={({ item }) => <FeedCard item={item} />}
    />
  );
}

function FeedCard({ item }: { item: FeedItem }) {
  return (
    <View>
      <CardTitle title={item.title} />
      <CardBody body={item.body} />
    </View>
  );
}
```

## Pass only what is read

A component that receives the whole screen object re-renders when
any field changes, even fields it never shows. Pass only the props
it reads.

```tsx
// Good: the card reads a title and a body, nothing else.
<FeedCard title={item.title} body={item.body} />
```

## Composition over configuration

Prefer small components combined in the parent over one component
with many boolean props. A `Card` that takes `children` beats a
`Card` with twelve flags.

```tsx
function Card({ children }: { children: React.ReactNode }) {
  return <View style={styles.card}>{children}</View>;
}

// Use it by composing, not by configuring.
<Card>
  <CardTitle title="Sleep" />
  <Text>7h 12m</Text>
</Card>
```

## Keep render pure

- Render reads props and state and returns UI. Nothing else.
- No network calls, no storage writes, no random ids in render.
- Side effects belong in `useEffect`, event handlers, or a hook.
- A component must render the same output for the same props and
  state. If it does not, find the hidden input.

## Identity

- Give list items stable keys (see `list-patterns.md`).
- Do not create a new component type inside render. A component
  defined in another component's body is a new type each render
  and loses its state.
- Memoise a component only when a measurement shows it re-renders
  too often. `memo` is a tool, not a default.

## Checklist

- [ ] Each component has one job.
- [ ] Props are only what the component reads.
- [ ] No side effects in render.
- [ ] No component type is defined inside another render.
- [ ] Large bodies are split into subcomponents.
