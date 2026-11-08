# Image Optimisation (Expo)

SwiftUI idea: downsample to display size, cache, and never decode
a full photo for a thumbnail. The Expo form is the same, with
`expo-image`.

## Size to display

- A 64px avatar does not need a 4000px photo. Resize at the
  source, or request a sized variant from the server.
- For remote images, ask the server for the size you display.
  If the server cannot resize, do not fetch the full image and
  shrink it on device as a habit.
- For local images, ship the sizes you need. Do not ship one huge
  asset and scale it down everywhere.

## expo-image

- Prefer `expo-image` for remote images. It handles caching and
  sizing better than the built-in image component.
- Check the versioned Expo docs for its current props before
  using a prop you have not used before. Do not guess prop names
  from memory.
- Set an explicit size on every image. An image with no size
  collapses, jumps the layout when it loads, or both.

```tsx
// Size is explicit. The layout does not jump when the image loads.
<Image
  source={{ uri: item.photoUrl }}
  style={{ width: 64, height: 64, borderRadius: 32 }}
/>
```

## Placeholders and errors

- Show a placeholder colour or a skeleton while an image loads.
  A blank hole that suddenly fills is a layout shift.
- Show a fallback when an image fails to load. A broken image
  icon is not a fallback.
- Never block the screen on an image. Text and controls load
  first; images follow.

## Animated WebP

- Animated WebP from 3dicon renders natively in `expo-image`.
  No Lottie, no new native dependency.
- See `animated-icons-3dicon.md` for sizes, costs, and the
  reduced-motion rule: ship the still alongside the loop.

## Checklist

- [ ] Images are sized to their display size.
- [ ] Every image has an explicit size.
- [ ] Remote images are cached, not re-downloaded per visit.
- [ ] Loading shows a placeholder; failure shows a fallback.
- [ ] Images never block text and controls.

## Worked example: an avatar

A 64px avatar shows a user's photo. The server returns a 64px
variant. The component sets an explicit 64x64 size and a 32pt
radius. While it loads, a pastel circle with initials shows.
If it fails, the initials stay. The layout never jumps, because
the size was explicit before the image arrived.

## Common mistakes

- No explicit size. The image collapses to zero, then jumps the
  layout when it loads.
- A full-resolution photo in a thumbnail. Decode time and memory
  for pixels no one sees.
- Re-downloading on every visit. Cache it. A list that fetches
  every image on every visit is slow and wasteful.
- No fallback. A failed image leaves a broken icon or a hole.
- Blocking the screen on images. Text first, controls second,
  images last. A screen that waits for a photo to show its title
  is backwards.

## Sizing guide

- Avatar 64px: request 128px (2x for retina), display 64.
- Card image: request the card's display width, not the photo's
  full width.
- Hero image: request the screen width, not the camera's width.
- When in doubt, request the display size times the screen scale,
  and no more.
