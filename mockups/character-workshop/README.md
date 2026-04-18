# Character Workshop — layout mockups

Three static HTML mockups for a proposed **Character Workshop** screen in
Doom's Enhancement Suite: a single modal that unifies everything a user can
currently do to a character across three different UIs (portrait-bar
right-click menu, Tracker Editor popup, Bunny Mo character sheet).

These files are **not** wired into the extension. They live under `mockups/`
so the shipped extension ignores them. Open each HTML file directly in a
browser to evaluate the layout.

```
open mockups/character-workshop/variant-a-tabbed.html
open mockups/character-workshop/variant-b-split.html
open mockups/character-workshop/variant-c-wizard.html
```

Or from the repo root:

```
python3 -m http.server 8000
# then visit http://localhost:8000/mockups/character-workshop/
```

## The three variants

All three cover the same data: name, relationship, portrait, dialogue color,
tracker fields (custom fields, thoughts, character stats), and Bunny Mo
character-sheet sections. They only differ in how that data is laid out.

### Variant A — Tabbed single-pane (`variant-a-tabbed.html`)
Mirrors today's Tracker Editor popup (`#rpg-tracker-editor-popup`). Horizontal
tab bar: **Identity · Appearance · Trackers · Sheet**, one pane at a time,
compact ~560px modal.
- **Pros:** zero learning curve for existing DES users; same shell as other
  modals; smallest footprint so it fits on narrow windows.
- **Cons:** you can't see the portrait preview while editing trackers or
  sheet sections; every field change is invisible until you flip back to the
  Appearance tab.

### Variant B — Split pane with live preview (`variant-b-split.html`) — recommended
Left rail (~35%) shows a live `.dooms-portrait-card` preview and a vertical
section nav. Right pane (~65%) holds the current section's editor. Uploading
a portrait or picking a swatch updates the preview card immediately.
- **Pros:** WYSIWYG — every change is visible; the preview validates the
  real portrait-card style in context; rail also doubles as quick-glance
  summary.
- **Cons:** wider modal (up to 900px) so it's less friendly on narrow
  screens; the right pane has slightly less horizontal room for the Sheet
  editor than Variant A.

### Variant C — Stepped wizard (`variant-c-wizard.html`)
Five-step flow: **1 Identity → 2 Appearance → 3 Trackers → 4 Sheet → 5
Review**. Back/Next in footer, with a live Review step summarizing all
choices plus a portrait preview.
- **Pros:** guided — great for creating a character from scratch; enforces
  completeness; Review step catches mistakes before save.
- **Cons:** tedious for quick edits ("I just want to change her dialogue
  color"); requires users to click through unrelated steps; doesn't match
  any existing DES flow.

## Recommendation

**Go with Variant B.** The live preview is the highest-value improvement
over the current fragmented UX, and the section nav still lets power users
jump straight to what they want (unlike the wizard's forced order). Variant
A is a fine fallback if we want to minimize scope / match existing modals
exactly.

## Visual contract

All three mockups share `shared.css`, which declares the DES theme tokens
from `style.css:90-95` on `:root` and re-implements the modal/tab/form
primitives from `.rpg-settings-popup*` (`style.css:5817-5950`) and
`.dooms-portrait-card` (`style.css:13933-13972`). The 30-color dialogue
palette is copied verbatim from `src/systems/ui/portraitBar.js:29-38`.
Relationship emoji and default tracker fields come from
`src/systems/ui/trackerEditor.js:289-357`.

## What's mocked vs. not

Cosmetic interactions work: tab/step switching, accordion toggles, swatch
selection highlight, relationship chip selection, portrait upload preview
(via `URL.createObjectURL`), live name/color propagation to the preview
card. Nothing persists, nothing calls SillyTavern, and the Save/Delete
buttons are no-ops.

## Known limitations

- Only the Default theme is rendered. The other 10 DES theme presets
  (Cyberpunk, Fantasy, etc.) should be validated as a follow-up.
- Mobile layout is minimally considered — it doesn't break at 600px wide
  but isn't polished below that.
- No keyboard navigation beyond native tab order.
