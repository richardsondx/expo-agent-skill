# Handling Soft-Deprecated APIs (Expo)

SwiftUI idea: hard-deprecated APIs get replaced; soft-deprecated
APIs get flagged and left in place during feature work. The Expo
form is the same split.

## Hard vs soft

| Kind | What to do |
|---|---|
| Removed in the current SDK | Replace now. The code does not run otherwise. |
| Deprecated but still working | Flag it. Leave it in place during feature work. |

## During feature work

- Do not rewrite a working screen to chase a deprecation.
  Feature work ships the feature. Migration is a separate task.
- Flag the soft-deprecated API in your report: where it is, what
  replaces it (from the versioned docs), and that it still works.
- If the replacement is a one-line change with no behaviour
  change, you may make it — and say you did.

## During migration work

- When the user asks to migrate, replace soft-deprecated APIs
  with the versioned-docs equivalent.
- Migrate one area at a time. Run lint and typecheck after each area.
- If the replacement changes behaviour, say so before changing it.
  Do not silently change what the user sees.

## Checklist

- [ ] Removed APIs are replaced, not worked around.
- [ ] Soft-deprecated APIs are flagged, not silently rewritten.
- [ ] Replacements come from the versioned docs.
- [ ] Lint and typecheck pass after migration.

## Worked example

A screen uses a list prop that the versioned docs mark as
deprecated but still working. The feature task is to add a
filter to that screen. You add the filter. You leave the
deprecated prop alone. In your report you write one line:
"Uses the deprecated `X` prop; the versioned docs point to
`Y`; it still works; migration is a separate task." That is
the whole handling. The feature ships, the debt is named,
and no behaviour changed by accident.

## When to break the rule

- The deprecated API is removed in the SDK version you are
  already migrating to. Then it is hard-deprecated, not soft.
  Replace it.
- The deprecated API has a known bug that affects this feature.
  Say so, replace it, and name the bug in your report.
- The replacement is a one-line change with no behaviour change.
  Make it, and say you made it.

## What not to do

- Do not rewrite a whole screen to remove one deprecated prop.
- Do not mix a migration into a feature pull request. The
  reviewer cannot tell which change broke what.
- Do not ignore a deprecation warning forever. Flagged means
  someone reads the flag. If no one ever migrates, the flag was
  decoration.

## Tracking

- Keep a short list of flagged APIs: where each one lives, what
  replaces it, and which SDK version removed it, if any.
- Revisit the list at each SDK upgrade. That is the cheapest
  time to migrate, because you are already testing that area.
## Reporting format

When you flag a soft-deprecated API, write one line with four
facts: where it is, what it is, what replaces it, and whether it
still works. Example: "`Feed.tsx` uses the deprecated `X` prop;
the versioned docs point to `Y`; it still works in the current
SDK." A flag without the replacement is a warning with no way out.
A flag without the location is a warning no one can act on.
