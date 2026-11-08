# List Patterns (Expo)

SwiftUI idea: stable identity in `ForEach` is a correctness rule.
The Expo form is the same: a stable `keyExtractor` is not optional.

## Stable keys

```tsx
<FlatList
  data={items}
  keyExtractor={(item) => item.id}
  renderItem={({ item }) => <Row item={item} />}
/>
```

- Never use the index as a key. Reordering, inserting, or deleting
  then shows the wrong row, keeps the wrong state, and animates
  the wrong item.
- The key must outlive the row and must not come from mutable
  content. An id from the server is right. A title is not.
- One item per row. A row component renders one item and nothing else.

## FlatList, not ScrollView, for long lists

- `ScrollView` renders every child at once. Fine for a short
  settings screen. Wrong for a feed.
- `FlatList` renders a window of rows. Use it for anything that can
  grow.
- For very long lists, evaluate FlashList. Check the versioned Expo
  docs for install and props. Do not guess from memory.

## Tuning

- Keep rows small. A row that reads a large context re-renders
  when that context changes.
- Do not filter a large list inside `renderItem`. Filter the data
  once, upstream, and pass the filtered array.
- Paginate with `onEndReached`. Set a sensible threshold and guard
  against double-fetching while a page is loading.
- Show a loading row at the end, an empty state when there is no
  data, and an error state with retry when loading fails.

```tsx
<FlatList
  data={items}
  keyExtractor={(item) => item.id}
  renderItem={({ item }) => <Row item={item} />}
  onEndReached={loadMore}
  onEndReachedThreshold={0.5}
  ListEmptyComponent={<EmptyState />}
  ListFooterComponent={loading ? <Spinner /> : null}
/>
```

## SectionList

Use `SectionList` when the data has real sections with headers.
Do not fake sections by inserting header rows into a flat list;
that breaks keys and windowing.

## Swipe actions and reorder

- Keep swipe actions simple: one or two actions, with clear labels.
- Reorder needs stable ids and a drag handle. Do not invent a
  reorder gesture that fights the scroll gesture.
- After a reorder, persist the new order. A reorder that resets on
  the next visit is a bug.

## Checklist

- [ ] `keyExtractor` returns a stable id, never the index.
- [ ] Long lists use FlatList or SectionList.
- [ ] Filtering happens upstream, not in `renderItem`.
- [ ] Pagination guards against double-fetch.
- [ ] Empty, loading, and error states all exist.
## Worked example: a feed that grows

A feed loads 20 items, then 20 more as the user scrolls. Each item
has a server id. The key is that id. When an item is deleted, the
rows below it keep their state, because their keys did not change.
When the same list used the index as a key, deleting the first item
shifted every key, and every row showed the wrong content for a
frame. Stable keys are not a performance tip. They are correctness.

## Refresh

- Pull to refresh reloads the current page, not the whole list.
- A refresh that fails keeps the old data and shows a retry. It
  does not empty the list.
- A refresh that succeeds keeps the scroll position unless the
  data set itself changed.
