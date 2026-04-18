# Character Workshop — chosen mockup

Static HTML mockup of the **Character Workshop** screen for Doom's
Enhancement Suite: a single modal that unifies what today is split across
the portrait-bar right-click menu, the Tracker Editor popup, and the Bunny
Mo character-sheet modal.

Three layout variants were prototyped (tabbed / split / wizard). The
split-pane layout with a live portrait preview was selected and is the
only one kept here; the others were pruned to keep the branch clean.

```
mockups/character-workshop/
    workshop-mockup.html   # the chosen layout (formerly variant-b-split.html)
    shared.css             # DES theme tokens + modal/form primitives
```

## How to view

`workshop-mockup.html` depends on `shared.css`, so open it via any local
web server (or just open the file directly — most browsers will still
load the sibling stylesheet from `file://`). From the repo root:

```
python3 -m http.server 8000
# then visit http://localhost:8000/mockups/character-workshop/workshop-mockup.html
```

## Layout summary

Split pane inside the standard `.rpg-settings-popup` modal shell.

- **Left rail (~35%)** — live `.dooms-portrait-card` preview that reflects
  the name, dialogue color, relationship emoji, and uploaded portrait in
  real time. Below it, a vertical section nav: **Identity · Appearance ·
  Trackers · Sheet**.
- **Right pane (~65%)** — form for the currently selected section,
  scrollable.
- **Footer** — `Delete character` (left), `Cancel` / `Save` (right).

## Visual contract

- DES theme tokens from `style.css:90-95` (`--rpg-bg`, `--rpg-accent`,
  `--rpg-text`, `--rpg-highlight`, `--rpg-border`, `--rpg-shadow`).
- Modal shell selectors from `style.css:5817-5950`
  (`.rpg-settings-popup-content / -header / -body / -footer`).
- Portrait card from `style.css:13933-13972`.
- 30-color dialogue palette copied verbatim from
  `src/systems/ui/portraitBar.js:29-38`.
- Default relationship emoji and tracker field labels copied from
  `src/systems/ui/trackerEditor.js:289-357`.

## What's mocked vs. not

Cosmetic interactions work: section nav, accordion toggles, swatch
selection, relationship chips, portrait upload preview (via
`URL.createObjectURL`), live name/color propagation to the preview card.
Nothing persists, nothing calls SillyTavern, and Save/Delete are no-ops.

## Known limitations

- Only the Default theme is rendered. The other 10 DES theme presets
  (Cyberpunk, Fantasy, etc.) should be validated once this is wired up
  for real.
- No keyboard navigation beyond native tab order.
- The Trackers tab currently shows the global tracker field definitions
  (Appearance, Demeanor, Thoughts, stats) — during implementation we'll
  decide whether the Workshop edits per-character values here, global
  field defs, or both.
